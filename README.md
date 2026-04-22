# 🎮 RP2040-Retro-Handheld

> 🚀 榨乾 RP2040 效能的終極開源掌機與軟體生態系。支援 Doom、NES 模擬、Apple II 電腦，並完美相容 MakeCode Arcade 遊戲開發。

![Hero Image - 實體掌機照片預留區](https://via.placeholder.com/800x400?text=Insert+Hero+Image+Here)

*(預留區：這裡未來放一張最帥的實體掌機照片，最好是正在跑 Doom 或高亮顯示螢幕畫面的照片)*

## ✨ 核心特色 (Features)

這不僅僅是一個硬體專案，而是一個圍繞著 **Raspberry Pi Pico (RP2040)** 打造的完整軟硬體生態系：

- 🏎️ **硬體極限火力展示**：透過高度優化的底層驅動與 ILI9341 SPI 螢幕，流暢執行 Doom 與全速 NES 模擬。
- 🍎 **跨語言前沿開發**：使用 Rust + C++ 打造的 Apple II 模擬器，展現現代嵌入式開發潛力。
- 🎓 **教育與開源友善**：支援微軟 MakeCode Arcade，拖拉積木即可輕鬆開發專屬遊戲。
- ⌨️ **生產力工具切換**：接上電腦秒變自定義 USB 巨集機械鍵盤。

---

## 📦 軟體生態系 (The Software Stack)

本專案的核心由以下 5 個獨立且高度優化的開源儲存庫組成。您可以依據需求燒錄不同的 `.uf2` 韌體：

### 1.  FPS 經典移植
🔫 **[rp2040-doom-ili9341](https://github.com/pondahai/rp2040-doom-ili9341)**
將經典的 Doom 完美移植到 RP2040 上，針對 ILI9341 螢幕進行了底層渲染優化，展現微控制器的極限效能。

### 2. 復古遊戲模擬
👾 **[rp2040-ili9341-infones](https://github.com/pondahai/rp2040-ili9341-infones)**
基於 InfoNES 的紅白機模擬器，在有限的 SRAM 中實現流暢的遊戲體驗與音效輸出。

### 3. 經典電腦復刻 (Rust + C++)
🖥️ **[PicoApple2](https://github.com/pondahai/PicoApple2)**
一個高效能的 Apple II 模擬器。這是一個結合了 Rust 記憶體安全性與 C++ 硬體控制優勢的跨語言指標性專案。

### 4. 原生遊戲與 STEM 教育
🧩 **[makecode_arcade_console](https://github.com/pondahai/makecode_arcade_console)**
支援微軟 MakeCode Arcade 平台。讓這台掌機不僅能玩老遊戲，還能成為程式教育的絕佳載體。

### 5. 生產力與輸入設備
⌨️ **[pico_keyboard](https://github.com/pondahai/pico_keyboard)**
不玩遊戲時，它就是一把基於 RP2040 的自訂機械鍵盤/巨集鍵盤，支援完整按鍵矩陣掃描。

---

## 🛠️ 硬體開源 (Hardware)

*(預留區：未來補上硬體爆炸圖或 PCB 渲染圖)*
![Hardware Image Placeholder](https://via.placeholder.com/600x300?text=Insert+PCB/3D+Case+Image+Here)

這台掌機的硬體設計完全開源，您可以自己打造一台：

### BOM (材料清單)
*   **微控制器**: Raspberry Pi Pico (RP2040)
*   **螢幕**: 2.4" / 2.8" ILI9341 SPI TFT 顯示器
*   *(請依實際情況補充：按鍵微動開關、電池模組、喇叭/音效晶片、PCB 等)*

### 硬體檔案下載
*   **PCB Gerber 檔**: `[預留連結]` (可直接送 JLCPCB 打板)
*   **3D 列印外殼 (STL)**: `[預留連結]`
*   **電路圖 (Schematic)**: `[預留連結]`

---

## 🚀 快速開始 (Quick Start)

想要立刻體驗？您不需要自己編譯程式碼！

1. 前往本專案的 **[Releases 頁面](#)** 或是上述各軟體專案中下載編譯好的 `.uf2` 檔案。
2. 按住您掌機（或 Pico 核心板）上的 `BOOTSEL` 按鍵不放，然後插上 USB 線連接電腦。
3. 電腦會將掌機識別為一個名為 `RPI-RP2` 的隨身碟。
4. 將下載的 `.uf2` 檔案拖曳進去，掌機會自動重啟並執行程式！

---

## 🤝 參與貢獻與聯絡

歡迎在各個子專案中發起 Issue 或 Pull Request！如果您喜歡這個專案，請不吝給予一個 ⭐️ Star。

**Author**: [pondahai](https://github.com/pondahai)  
**License**: MIT License
