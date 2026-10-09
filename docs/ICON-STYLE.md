# 封面圖示風格

載入器的選單是圖形化的 cover-flow，每個 `.uf2` 配一張 96×96 的封面。
這份文件定義封面該長什麼樣，讓新韌體加進來時不會變成異物。

規格細節（檔名、格式、為什麼不是 PNG）見
[`rp2040-retro-loader`](https://github.com/pondahai/rp2040-retro-loader) 的 README。
這裡只談**風格**。

---

## 1. 現況

五個既有封面的共同語言（2026-09-12 分析）：

| 韌體 | 主體 | 不重複顏色 |
| :--- | :--- | ---: |
| `DOOM` | 士兵頭盔 | 404 |
| `InfoNES` | 紅白機主機 | 1348 |
| `PicoApple2` | Apple II 整機 | 553 |
| `RetroDict` | 打開的電子辭典 | 479 |
| `retro-editor` | 打字機 | 11 |

---

## 2. 五條規則

### 2.1 畫一個**具體的物件**，不是抽象符號

每張封面都是那個作品**最具辨識度的實體**——Doom 是頭盔、NES 是那台紅白相間
的主機、Apple II 是整台米色電腦。不是齒輪、不是資料夾、不是圓角方塊裡放一個
字母。

使用者在 cover-flow 裡是靠「認出那個東西」來選的，不是靠讀圖示語彙。

### 2.2 淺色或去背，**不要滿版深色底**

主體浮在白／淺色背景上，靠自身輪廓辨識。滿版深色底會讓那張封面在整排裡
特別黑一塊，而且 loader 的背景牆（1bpp logo 磁磚）會被完全擋住。

### 2.3 主體**填滿畫面**

邊緣要接近 96×96 的邊界。封面在選單裡本來就小，留白等於浪費解析度。

### 2.4 pixel art：硬邊、看得見顆粒，但**要有明暗**

不是扁平向量。既有封面（`retro-editor` 除外，見 §5）都有亮面、暗面與描邊，色數落在數百到一千多。
純平塗（十幾色）在這排裡會顯得單薄。

具體做法：主色至少配一個亮面、一個暗面，加上深色描邊；受光面統一在左上。

### 2.5 等角或正面的「產品照」視角

像在拍那台機器的型錄照，不是側影剪影。

---

## 3. 配色

生態系的年代感是 **1980 年代硬體**，所以：

| 用途 | 建議 |
| :--- | :--- |
| 機殼 | 米色／米白（`#C4BAA6` 一帶）—— Apple II 那種泛黃的塑膠 |
| 描邊 | 深褐或深灰（`#3A302C`），**不要純黑** |
| 重點色 | 每張封面挑**一個**暖色（紅／橘）當視覺焦點，面積要小 |
| 背景 | 白或極淺灰 |

⚠️ **RGB565 只有 5/6/5 位元**。大面積漸層會出現明顯色階，寧可用兩三階的
硬邊明暗，那也更符合 pixel art 的調性。

---

## 4. 檢查清單

新封面做好之後，跟現有那幾張**並排放大看一次**：

```bash
# 把 .RAW 轉回 PNG 並排比較（96x96 RGB565 big-endian），放大 4 倍輸出 covers.png
python - *.RAW <<'PY'
import sys
from PIL import Image
W = H = 96; SCALE = 4
tiles = []
for path in sys.argv[1:]:
    data = open(path, "rb").read()
    assert len(data) == W * H * 2, f"{path}: {len(data)} bytes, expected {W*H*2}"
    img = Image.new("RGB", (W, H))
    px = []
    for i in range(0, len(data), 2):
        v = (data[i] << 8) | data[i + 1]
        r, g, b = (v >> 11) & 0x1F, (v >> 5) & 0x3F, v & 0x1F
        px.append((r << 3 | r >> 2, g << 2 | g >> 4, b << 3 | b >> 2))
    img.putdata(px)
    print(f"{path}: {len(set(px))} colors")
    tiles.append(img.resize((W * SCALE, H * SCALE), Image.NEAREST))
sheet = Image.new("RGB", (W * SCALE * len(tiles), H * SCALE), "white")
for i, t in enumerate(tiles):
    sheet.paste(t, (i * W * SCALE, 0))
sheet.save("covers.png")
PY
```

- [ ] 縮到 96×96 還認得出是什麼嗎？
- [ ] 放在其他封面旁邊，會不會特別亮／特別暗／特別空？
- [ ] 色數有沒有到三位數？只有十幾色通常代表忘了畫明暗
- [ ] 主體有沒有填滿？四邊留白是不是太多？
- [ ] 檔案剛好 18432 bytes 嗎？

---

## 5. 已知偏離

`RetroDict` 帶了中文字（電子辭典／文），並且用了較粗的外描邊，比其他封面
更接近貼紙風。可接受——它要表達的是「查字典」這個行為而不是一台機器——但
新封面不建議再往這個方向走，優先畫物件。

`retro-editor` 的打字機封面只有 11 色，是純平塗，沒有亮面／暗面，違反 §2.4。
放在整排裡會顯得單薄。暫列為已知偏離，之後應依 §2.4 重畫。
