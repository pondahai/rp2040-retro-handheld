# 硬體規格 — 共用骨架的單一事實來源

> 這份文件是**接腳定義的單一事實來源**。各韌體 repo 的 README 只列自己實際
> 用到的子集，新的接腳定義或修訂一律先改這裡。
>
> 這跟 `rp2040-retro-loader` 的 `common/boot_map.h` 是同一個模式：flash 佈局
> 已經有單一事實來源了，硬體接腳以前沒有 —— 完整的表只存在於
> `PicoApple2/README.md` 裡，其他 repo 各有一份殘缺的子集。這份文件補上這個缺口。

整個生態系的核心理念是「同一塊硬體，燒不同 `.uf2`，就變成不同的機器」。
能成立的前提就是下面這張表在所有韌體之間保持一致。

---

## 1. 完整接腳表

MCU：Raspberry Pi Pico (RP2040)，雙核 Cortex-M0+，可超頻至 ~250MHz
（PicoApple2 預設 250MHz）。

| 子系統 | 訊號 | GPIO | 備註 |
| :--- | :--- | ---: | :--- |
| **顯示器** (spi0) | SCK | 18 | ILI9341 320×240，**16 位色**（不是單色） |
| | MOSI | 19 | |
| | MISO | 16 | 多數韌體不讀顯示器，可不接 |
| | CS | 17 | |
| | DC | 20 | 命令/資料選擇 |
| | RST | 21 | |
| | BL | 22 | 背光，可接 PWM 調亮度 |
| **SD 卡** (spi1) | SCK | 10 | 與顯示器分開的 bus —— 兩者可同時動作 |
| | MOSI | 11 | |
| | MISO | 12 | |
| | CS | 13 | |
| **鍵盤矩陣** | DATA_OUT | 15 | → 74HC595（驅動掃描列，同時驅動 LED） |
| | LATCH | 14 | |
| | CLOCK | 26 | 595 與 165 共用 |
| | DATA_IN | 27 | ← 74HC165（讀回行狀態） |
| **音效** | Sound Out | 7 | 1-bit PWM → RC 低通 → PAM8403 → 4Ω/8Ω 喇叭 |
| **LoRa** *(選配)* | UART | — | Meshtastic 節點，接腳依實際模組而定 |

### 兩條 SPI bus 是刻意分開的

顯示器在 **spi0**、SD 卡在 **spi1**。這表示推畫面與讀 SD 可以同時進行，
不需要共用匯流排的仲裁。寫新韌體時不要把它們併到同一條 bus 上。

---

## 2. 鍵盤：64 鍵實體 QWERTY，不是 D-pad

這點常被誤解。這台機器的輸入是**完整的 64 鍵鍵盤**：

- 實體佈局 5 列 × 13 格，SPACE 佔兩格 —— 恰好 64 鍵 = **8×8 矩陣**
- 74HC595 逐列拉低掃描，74HC165 讀回該列的 8 個 bit
- 佈局規律是**奇偶列交錯**：r0/r4 是數字列、r1/r5 是 QWERTY 上排、
  r2/r6 是中排、r3/r7 是下排；col 5–7 集中放修飾鍵與功能鍵

因此這台機器做文字輸入、注音中文、終端機是合理的，不必遷就遊戲手把的按鍵數。

### ⚠️ 真值表要用實測版

**權威來源：`PicoApple2-KeyboardTester/README.md` 的「完整真值表」一節。**
那份是用檢查器在真機上跑到 `map is complete and one-to-one` 得出來的。

**不要用 `PicoApple2.ino` 的 `keymap_base`** —— 那張表有 **4 組錯位與 3 個
未定義鍵**（見 `rp2040-retro-dict/docs/PLAN.md §2.2`）。

Shift 排列是照**現代 PC 鍵盤**排的，不是 Apple II+ 的排法。鍵帽印什麼就出什麼。
這是刻意的設計，不是錯位。

### 現成可重用的掃描邏輯

`rp2040-retro-dict/firmware/keys.c` 已經把「掃描結果 → 按鍵事件」寫成
**純 C、不碰 GPIO** 的一層：輸入是 8 個 byte 的 bit mask，輸出是事件結構。
去彈跳（30ms）、修飾鍵、連發（400ms 起跳 / 60ms 間隔）這些真正容易寫錯的
邏輯因此能在 PC 上測。新韌體直接搬這個檔案，只要自己寫「餵 74HC165 的結果
進去」那十幾行。

---

## 3. 給新韌體作者

### 接腳以外，還有兩件事會咬人

1. **flash 佈局**：要上 SD 卡給載入器用的韌體必須編偏移版（link 到
   `0x10004000`）。完整規則見 `rp2040-retro-loader/README.md` 與
   `common/boot_map.h`。
2. **重置的語意被載入器改掉了**：軟重置（watchdog、`picotool reboot`）會
   **直接穿透**跳回你的韌體，不顯示選單。只有冷開機、實體 RESET、或
   開機時按住 **B** 才會進選單。如果你的韌體內部用 watchdog 重置做狀態
   切換，行為會跟單獨燒錄時不一樣。

### 可以直接搬的東西

| 要做什麼 | 搬哪裡 |
| :--- | :--- |
| 鍵盤掃描 → 事件 | `rp2040-retro-dict/firmware/keys.c`（純 C，PC 可測） |
| 注音輸入法 | `rp2040-retro-dict/firmware/ime.c` + `ime_tables.h` |
| 中文字型 16×16 2bit 灰階 | `rp2040-retro-dict`（Noto，OFL 1.1） |
| 中文點陣字型（選單用） | `rp2040-ili9341-infones`（Cubic 11） |
| FatFs + SD 驅動 | `rp2040-ili9341-infones/software/infones/drivers/` |

> ⚠️ **注音碼表的授權未定**：碼表來自 `pico_keyboard_ime_terminal`，
> 該 repo 目前沒有 LICENSE 檔、碼表出處也未寫明。**散布前要先補上。**

---

## 4. 這張表的出處與交叉驗證

2026-09-12 盤點了生態系內所有記載接腳的 repo，**數字全部一致、沒有矛盾**：

| 來源 | 涵蓋範圍 |
| :--- | :--- |
| `PicoApple2/README.md` | **唯一完整的一份**（顯示器 + SD + 音效 + 矩陣） |
| `PicoApple2-KeyboardTester/README.md` | 矩陣 + 顯示器，另含實測真值表 |
| `rp2040-retro-loader/README.md` | 顯示器 + SD |
| `rp2040-ili9341-infones/.../README.md` | 顯示器 + SD |

**音效的 GPIO 7 全生態系只有 `PicoApple2/README.md` 記載過一次**，尚未經第二
來源交叉驗證。做音效的新韌體請先在真機上確認。

PCB Gerber、3D 列印外殼 (STL) 與電路圖尚未發布。
