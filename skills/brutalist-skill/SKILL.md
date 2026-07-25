---
name: industrial-brutalist-ui
description: 粗獷的機械感介面，把瑞士字體排印印刷風（Swiss typographic print）與軍用終端機美學融合在一起。剛性 grid、極端的 type scale 對比、實用主義配色、類比劣化（analog degradation）效果。適合資料密度高的 dashboard、作品集，或想營造解密藍圖質感的編輯型網站。Raw mechanical interfaces fusing Swiss typographic print with military terminal aesthetics. Rigid grids, extreme type scale contrast, utilitarian color, analog degradation effects. For data-heavy dashboards, portfolios, or editorial sites that need to feel like declassified blueprints.
---

# SKILL：Industrial Brutalism & Tactical Telemetry UI

## 1. Skill 基本資料
**名稱：** Industrial Brutalism & Tactical Telemetry Interface Engineering
**說明：** 進階的網頁介面架構能力，融合二十世紀中期的瑞士字體排印設計（Swiss Typographic design）、工業製造手冊，以及復古未來感的航太／軍用終端機介面。這門功夫要求你徹底掌握剛性的模組化 grid、極端的字體排印尺度對比、純粹實用主義的色盤，以及用程式模擬類比劣化效果（halftone 網點、CRT 掃描線、bitmap dithering 抖動）。目標是打造出散發粗獷功能性、機械精準度與高資料密度的數位環境，並刻意捨棄一般消費級 UI 的慣用模式。

## 2. 視覺原型（Visual Archetypes）
這套設計系統靠融合兩種截然不同但高度相容的視覺典範來運作。**每個專案只挑一種，然後貫徹到底。不要在同一個介面裡交替或混用兩種模式。**

### 2.1 Swiss Industrial Print（瑞士工業印刷）
源自 1960 年代的企業識別系統與重型機械藍圖。
*   **特徵：** 高對比的 light mode（新聞紙／米白紙材質感）。仰賴厚重如磐石的粗黑無襯線字體排印。用可見的分隔線勾勒出毫不留情的結構性 grid。負空間的運用強勢而不對稱，並以超大尺寸、直接出血到 viewport 邊緣的數字或字母點題。大量使用原色紅作為警示／點綴色。

### 2.2 Tactical Telemetry & CRT Terminal（戰術遙測與 CRT 終端機）
源自機密軍用資料庫、老式大型主機，以及航太的抬頭顯示器（HUD）。
*   **特徵：** 只用 dark mode。高密度的表格式資料呈現。等寬（monospace）字體排印絕對主導。整合技術性的框飾元件（ASCII 括號、準星）。套用模擬的硬體限制（螢光餘暉、掃描線、低位元深度渲染）。

## 3. 字體排印架構
字體排印就是主要的結構與裝飾基礎建設，影像只是次要的。這套系統要求尺度、字重與間距上都有極端落差。

### 3.1 Macro-Typography（結構性標題）
*   **分類：** Neo-Grotesque／粗黑無襯線。
*   **最佳網頁字型：** Neue Haas Grotesk (Black)、Inter (Extra Bold/Black)、Archivo Black、Roboto Flex (Heavy)、Monument Extended。
*   **實作參數：**
    *   **尺度（Scale）：** 用流動字體排印（fluid typography）拉到超大尺寸（例如 `clamp(4rem, 10vw, 15rem)`）。
    *   **字距（Tracking／Letter-spacing）：** 極度緊縮，常常是負值（`-0.03em` 到 `-0.06em`），逼字符擠成實心的建築式量體。
    *   **行距（Leading／Line-height）：** 高度壓縮（`0.85` 到 `0.95`）。
    *   **大小寫（Casing）：** 一律全大寫，強化結構衝擊力。

### 3.2 Micro-Typography（資料與遙測）
*   **分類：** Monospace／技術感無襯線。
*   **最佳網頁字型：** JetBrains Mono、IBM Plex Mono、Space Mono、VT323、Courier Prime。
*   **實作參數：**
    *   **尺度：** 固定且小（`10px` 到 `14px`／`0.7rem` 到 `0.875rem`）。
    *   **字距：** 放寬（`0.05em` 到 `0.1em`），模擬機械打字機的間距或終端機的字元矩陣。
    *   **行距：** 標準到偏緊（`1.2` 到 `1.4`）。
    *   **大小寫：** 一律全大寫。所有 metadata、導覽、單元編號與座標都用它。

### 3.3 質地對比（藝術性破壞）
*   **分類：** 高對比襯線體。
*   **最佳網頁字型：** Playfair Display、EB Garamond、Times New Roman。
*   **實作參數：** 極度節制地使用。必須經過重度後製（halftone 濾鏡、1-bit dithering）來破壞向量的完美感，跟乾淨的無襯線體形成質地上的對照。

## 4. 色彩系統
色彩架構毫不妥協。嚴禁漸層、柔和的 drop shadow 與現代感的半透明效果。顏色是用來模擬實體媒材或原始的發光顯示器。

**關鍵：每個專案只選一種基底（substrate）色盤，並貫徹使用。絕不在同一個介面裡混用亮色與暗色基底。**

### 若採 Swiss Industrial Print（Light）：
*   **背景：** `#F4F4F0` 或 `#EAE8E3`（霧面、未漂白的文件用紙）。
*   **前景：** `#050505` 到 `#111111`（碳墨黑）。
*   **點綴色：** `#E61919` 或 `#FF2A2A`（航空／警示紅）。這是唯一的點綴色。用於刪除線、粗的結構性分隔線，或關鍵資料的強調。

### 若採 Tactical Telemetry（Dark）：
*   **背景：** `#0A0A0A` 或 `#121212`（關機狀態的 CRT。避免純 `#000000`）。
*   **前景：** `#EAEAEA`（白色螢光）。這是主要的文字顏色。
*   **點綴色：** `#E61919` 或 `#FF2A2A`（航空／警示紅）。同一個紅，同一套規則。
*   **終端機綠（`#4AF626`）：** 可選。只用在單一個特定的 UI 元素上（例如一個狀態指示燈或一個資料讀數）——絕不當成通用文字顏色。如果它沒有明確的用途，就整個不要用。

## 5. 版面與空間工程
版面必須看起來像經過數學計算設計出來的。它拒絕慣例的網頁 padding，改用看得見的分區隔間。

*   **藍圖 Grid：** 嚴格遵守 CSS Grid 架構。元素不會漂浮；它們精準錨定在 grid 的軌道與交會點上。
*   **可見的分區隔間：** 大量使用實線邊框（`1px` 或 `2px solid`）來劃分不同的資訊區域。水平線（`<hr>`）常常橫跨整個容器寬度，用來區隔各個作業單元。
*   **雙峰密度（Bimodal Density）：** 版面在兩個極端之間擺盪——一邊是極高的資料密度（緊密堆疊在一起的等寬 metadata），一邊是大片經過計算、用來襯托 macro-typography 的負空間。
*   **幾何：** 徹底拒絕 `border-radius`。所有轉角都必須是精確的 90 度，以貫徹機械式的剛性。

## 6. UI 元件與符號系統
標準的網頁 UI 慣例，一律換成實用主義的工業圖形元素。

*   **語法裝飾：** 用 ASCII 字元把資料點框起來。
    *   *框飾：* `[ DELIVERY SYSTEMS ]`、`< RE-IND >`
    *   *方向指示：* `>>>`、`///`、`\\\\`
*   **工業標記：** 大量整合註冊商標（`®`）、著作權（`©`）與商標（`™`）符號，把它們當成結構性的幾何元素，而不是法律聲明文字。
*   **技術素材：** 在 grid 交會點放上準星（`+`）、重複的垂直線（條碼）、粗的水平警示條紋，以及隨機字串資料（例如 `REV 2.6`、`UNIT / D-01`），來模擬機械正在運轉的感覺。

## 7. 質地與後製效果
為了避免設計看起來太純數位，要透過 CSS 與 SVG 濾鏡，把模擬的類比劣化效果實作進前端。

*   **Halftone 與 1-Bit Dithering：** 把連續色調的影像或大尺寸襯線字體轉成點矩陣圖樣。做法是預先處理影像，或用 CSS `mix-blend-mode: multiply` 疊層搭配 SVG 的放射狀網點圖樣。
*   **CRT 掃描線：** 終端機風格的介面，可對背景套用 `repeating-linear-gradient` 來模擬水平電子束掃描（例如 `repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,0,0,0.1) 2px, rgba(0,0,0,0.1) 4px)`）。
*   **機械雜訊：** 在 DOM 根節點套用一層全域、低不透明度的 SVG 雜訊濾鏡，讓 dark mode 與 light mode 都帶有統一的實體顆粒感。

## 8. 網頁工程指令
1.  **Grid 決定論：** 用 `display: grid; gap: 1px;` 搭配父子元素對比的背景色，不必寫複雜的 border 宣告，就能產生數學上完美、細如刀鋒的分隔線。
2.  **語意剛性：** 用精確的語意標籤（`<data>`、`<samp>`、`<kbd>`、`<output>`、`<dl>`）來建構 DOM，準確反映遙測資料的技術本質。
3.  **Typography Clamping：** CSS `clamp()` 函式只用在 macro-typography 上，確保超大文字能強勢縮放，同時在各種 viewport 下都維持結構完整性。
