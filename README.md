# 🎮 RP2040-Retro-Handheld

> 🚀 榨乾 RP2040 效能的終極開源掌機與軟體生態系。一塊硬體、多種韌體：支援 Doom、NES 模擬、Apple II 電腦、MakeCode Arcade 遊戲開發，接上電腦還能秒變自訂機械鍵盤。

![Hero Image - 實體掌機照片預留區](https://via.placeholder.com/800x400?text=Insert+Hero+Image+Here)
<img width="595" height="794" alt="image" src="https://github.com/user-attachments/assets/a8c45978-43c5-4510-b026-743aaa89b75c" />

*(預留區：這裡未來放一張最帥的實體掌機照片，最好是正在跑 Doom 或高亮顯示螢幕畫面的照片)*

---

## 💡 這是什麼？(What is this?)

這是一個 **傘狀總綱專案 (umbrella project)**。它本身不含韌體原始碼，而是把圍繞 **Raspberry Pi Pico (RP2040)** 打造的一系列獨立韌體與開源硬體**整合成一個統一的生態系**。

核心理念是 **「同一塊硬體，燒不同 `.uf2`，就變成不同的機器」**：

- 🏎️ **硬體極限火力展示**：透過高度優化的底層驅動與 ILI9341 SPI 螢幕，流暢執行 Doom 與全速 NES 模擬。
- 🍎 **跨語言前沿開發**：使用 Rust + C++ 打造的 Apple II 模擬器，展現現代嵌入式開發潛力。
- 🎓 **教育與開源友善**：支援微軟 MakeCode Arcade，拖拉積木即可輕鬆開發專屬遊戲。
- ⌨️ **生產力工具切換**：接上電腦秒變自定義 USB 巨集機械鍵盤，甚至是支援中文注音輸入與 Meshtastic 通訊的離網終端機。

---

## 📦 軟體生態系 (The Software Stack)

本生態系由以下幾個**獨立且高度優化**的開源儲存庫組成。它們共用同一套硬體骨架（見下方硬體章節），您只需依需求燒錄不同的 `.uf2` 韌體即可切換功能。

| # | 韌體 | 角色 | 語言 / 技術 | 狀態 |
| :-: | :--- | :--- | :--- | :--- |
| 1 | 🔫 **[rp2040-doom-ili9341](https://github.com/pondahai/rp2040-doom-ili9341)** | FPS 經典移植 | C，底層渲染優化 | 子專案 |
| 2 | 👾 **[rp2040-ili9341-infones](https://github.com/pondahai/rp2040-ili9341-infones)** | NES 紅白機模擬 | C，InfoNES 移植 | 子專案 |
| 3 | 🖥️ **[PicoApple2](https://github.com/pondahai/PicoApple2)** | Apple II 電腦復刻 | Rust + C++ 雙核 | 子專案 |
| 4 | 🧩 **[makecode_arcade_console](https://github.com/pondahai/makecode_arcade_console)** | 原生遊戲與 STEM 教育 | MakeCode Arcade | 子專案 |
| 5 | ⌨️ **[pico_keyboard](https://github.com/pondahai/pico_keyboard)** | USB 巨集機械鍵盤 | C++，矩陣掃描 | 子專案 |
| 6 | 📡 **[pico_keyboard_ime_terminal](https://github.com/pondahai/pico_keyboard_ime_terminal)** | 中文注音輸入 / Meshtastic 離網終端機 | C++，嵌入式 IME + nanopb | 子專案 |

### 1. FPS 經典移植
🔫 **[rp2040-doom-ili9341](https://github.com/pondahai/rp2040-doom-ili9341)**
將經典的 Doom 完美移植到 RP2040 上，針對 ILI9341 螢幕進行了底層渲染優化，展現微控制器的極限效能。

### 2. 復古遊戲模擬
👾 **[rp2040-ili9341-infones](https://github.com/pondahai/rp2040-ili9341-infones)**
基於 InfoNES 的紅白機模擬器，在有限的 SRAM 中實現流暢的遊戲體驗與音效輸出。

### 3. 經典電腦復刻 (Rust + C++)
🖥️ **[PicoApple2](https://github.com/pondahai/PicoApple2)**
一個高效能的 Apple II 模擬器。Rust (`thumbv6m-none-eabi`) 撰寫精確的 6502/Disk II 模擬核心，C++/Arduino 實作雙核渲染架構（Core 1 專職 JIT 視訊渲染），預設超頻 250MHz，支援 `.DSK` 讀寫與週期精確音訊。這是一個結合 Rust 記憶體安全性與 C++ 硬體控制優勢的跨語言指標性專案。
<img width="595" height="794" alt="image" src="https://github.com/user-attachments/assets/ea9605cf-3d5e-4310-8ca1-cffb8a36d97c" />

### 4. 原生遊戲與 STEM 教育
🧩 **[makecode_arcade_console](https://github.com/pondahai/makecode_arcade_console)**
支援微軟 MakeCode Arcade 平台。讓這台掌機不僅能玩老遊戲，還能成為程式教育的絕佳載體——拖拉積木即可開發專屬遊戲。
<img width="1059" height="794" alt="image" src="https://github.com/user-attachments/assets/ae91dffb-be38-4c77-919c-9c19417db682" />

### 5. 生產力與輸入設備
⌨️ **[pico_keyboard](https://github.com/pondahai/pico_keyboard)**
不玩遊戲時，它就是一把基於 RP2040 的自訂機械鍵盤/巨集鍵盤，支援完整按鍵矩陣掃描。
<img width="595" height="794" alt="image" src="https://github.com/user-attachments/assets/5aaf62c8-2399-4e1f-8beb-bdb8fbc080a8" />

### 6. 離網通訊終端機 (進階)
📡 **[pico_keyboard_ime_terminal](https://github.com/pondahai/pico_keyboard_ime_terminal)**
鍵盤韌體的進階演化版。在資源有限的 MCU 上實現了**嵌入式中文注音輸入法引擎**（兩階段二分搜尋查詢、PROGMEM 字型渲染）、模組化分頁 UI 框架，並透過 UART 與 **Meshtastic** 節點通訊（nanopb 解析 Protobuf），讓掌機化身為可離網收發訊息的 LoRa 終端機。

---

## 🛠️ 硬體開源 (Hardware)

*(預留區：未來補上硬體爆炸圖或 PCB 渲染圖)*
![Hardware Image Placeholder](https://via.placeholder.com/600x300?text=Insert+PCB/3D+Case+Image+Here)

這台掌機的硬體設計完全開源。整個生態系之所以能「一塊板子跑所有韌體」，是因為各韌體共用同一套**硬體骨架**：

### BOM (材料清單 / 共用硬體骨架)
| 類別 | 元件 | 說明 |
| :--- | :--- | :--- |
| **微控制器** | Raspberry Pi Pico (RP2040) | 雙核 Cortex-M0+，可超頻至 ~250MHz |
| **顯示器** | 2.4" / 2.8" ILI9341 SPI TFT (320×240) | 所有韌體共用的顯示介面 |
| **輸入** | 74HC165 / 74HC595 移位暫存器 | 按鍵矩陣掃描（595 輸出掃描列、165 讀回狀態），同時驅動 LED |
| **儲存** | SD 卡模組 (SPI) | 用於 PicoApple2 載入 `.DSK`、模擬器讀取 ROM/遊戲檔 |
| **音效** | 1-bit PWM 輸出 + RC 低通 + PAM8403 D 類放大器 | 驅動 4Ω/8Ω 喇叭 |
| **離網通訊** *(選配)* | Meshtastic LoRa 節點 (UART) | 供 `pico_keyboard_ime_terminal` 收發訊息 |
| **其他** | 按鍵微動開關、電池/電源模組、PCB、3D 列印外殼 | 依實際組裝補充 |

> 📌 **設計重點**：因為螢幕（ILI9341 SPI）與輸入（74HC165/595 矩陣）的接腳定義在各韌體間保持一致，使用者才能在同一塊實體板上自由切換 Doom / NES / Apple II / 鍵盤等不同韌體。各韌體的詳細 GPIO 接線表請見其對應子專案的 README。

### 硬體檔案下載
*(以下為預留連結，待硬體定稿後補上)*
*   **PCB Gerber 檔**: `[預留連結]` (可直接送 JLCPCB 打板)
*   **3D 列印外殼 (STL)**: `[預留連結]`
*   **電路圖 (Schematic)**: `[預留連結]`

---

## 🚀 快速開始 (Quick Start)

想要立刻體驗？您不需要自己編譯程式碼：

1. 前往您想體驗的**子專案**儲存庫（見上方軟體生態系表格），在其 **Releases 頁面** 下載編譯好的 `.uf2` 檔案。
2. 按住您掌機（或 Pico 核心板）上的 `BOOTSEL` 按鍵不放，然後插上 USB 線連接電腦。
3. 電腦會將掌機識別為一個名為 `RPI-RP2` 的隨身碟。
4. 將下載的 `.uf2` 檔案拖曳進去，掌機會自動重啟並執行程式！
5. 想換一套功能？重複以上步驟，拖入另一個韌體的 `.uf2` 即可——硬體不變，韌體隨心切換。

> 💡 部分韌體（如 PicoApple2）需要自行合法取得對應的 ROM / 遊戲檔並放入 SD 卡，詳見各子專案說明。

### 📦 進階：用選單一次帶著多套韌體

不想每次換功能都要拔電按 BOOTSEL？搭配 **[rp2040-retro-loader](https://github.com/pondahai/rp2040-retro-loader)** 載入器，開機會出現圖形選單，從 SD 卡上的多個 `.uf2` 直接選一個燒錄並執行。

整包已編譯好的成品（載入器、跳板、Doom / InfoNES / PicoApple2 三個 standalone 韌體，以及選單封面）收在 **[rp2040-handheld-bundle](https://github.com/pondahai/rp2040-handheld-bundle)**，韌體掛在其 [Releases](https://github.com/pondahai/rp2040-handheld-bundle/releases) 並附 sha256 校驗碼，下載即可用。

---

## 🗺️ 專案定位 (About This Repo)

本儲存庫是整個生態系的**總綱與入口頁 (landing page)**，本身不含韌體原始碼。各韌體的程式碼、開發歷程 (DevLog)、編譯說明與接線細節，請至對應的子專案儲存庫查閱。

---

## 💖 開源致謝與致敬 (Credits & Acknowledgements)

本專案及相關子專案的實現，高度仰賴並參照了開源社群中多位先驅者的傑出貢獻，特別在此致謝與表彰：

### 🔫 Doom 移植相關致謝
* **Graham Sanderson ([@kilograham](https://github.com/kilograham))**：
  主導並開發了令人讚嘆的 **[rp2040-doom](https://github.com/kilograham/rp2040-doom)**，將 Chocolate Doom 完美且不失真地移植到 RP2040 上，並研發了獨特的 WHD 壓縮算法。本專案的 `rp2040-doom-ili9341` 即是基於此核心程式碼進行 SPI 顯示器的適配與優化。
* **rsheldiii ([@rsheldiii](https://github.com/rsheldiii))**：
  其 **[rp2040-doom-LCD](https://github.com/rsheldiii/rp2040-doom-LCD)** 專案為在微控制器上驅動小螢幕提供了寶貴的 LCD/SPI 渲染修改思路。

### 👾 NES 紅白機模擬相關致謝
* **Jay Kumogata ([@jay-kumogata](https://github.com/jay-kumogata))**：
  經典紅白機模擬核心 **InfoNES** 的原始作者。
* **Shuichi Takano ([@shuichitakano](https://github.com/shuichitakano))**：
  主導開發了 **[pico-infones](https://github.com/shuichitakano/pico-infones)**，將 InfoNES 移植至 RP2040。
* **Frank Hoedemakers ([@fhoedemakers](https://github.com/fhoedemakers))**：
  開發了 **[pico-infonesPlus](https://github.com/fhoedemakers/pico-infonesPlus)**，引入了 SD 卡 ROM 選擇選單與多種手把支援，本專案的 `rp2040-ili9341-infones` 深度參考了其架構設計。

感謝這些優秀的開源創作者，沒有他們的基礎與奉獻，本掌機生態系便無法實現。

---

## 🤝 參與貢獻與聯絡

歡迎在各個子專案中發起 Issue 或 Pull Request！如果您喜歡這個專案，請不吝給予一個 ⭐️ Star。

**Author**: [pondahai](https://github.com/pondahai)
**License**: MIT License
