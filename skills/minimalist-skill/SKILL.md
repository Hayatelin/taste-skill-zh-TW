---
name: minimalist-ui
description: 乾淨的編輯風（editorial）介面。暖色調單色（warm monochrome）色盤、字體排印對比、扁平 bento grid、低飽和粉彩色。不用漸層、不用厚重陰影。
---

# Protocol：Premium Utilitarian Minimalism UI 架構師

## 1. Protocol 總覽
名稱：Premium Utilitarian Minimalism & Editorial UI
說明：一套進階的前端工程指令，用來產出高度精緻、極簡到底、「文件感（document-style）」的網頁介面，類似頂級 workspace 平台的風格。本 protocol 嚴格執行高對比的暖色調單色色盤、量身打造的字體排印階層、講究的結構性大尺度留白（macro-whitespace）、bento grid 版面，以及搭配刻意選用之低飽和粉彩點綴色的超扁平元件架構。它主動排除一般常見的 generic SaaS 設計潮流。

## 2. 絕對負面約束（禁用元素）
AI 必須嚴格避免以下常見的網頁開發預設做法：
- 不准使用「Inter」、「Roboto」或「Open Sans」字型。
- 不准使用泛用的細線條 icon 庫，例如「Lucide」、「Feather」或標準「Heroicons」。
- 不准使用 Tailwind 預設的厚重陰影（例如 `shadow-md`、`shadow-lg`、`shadow-xl`）。陰影必須幾乎不存在，或大幅客製成超柔和擴散、低不透明度（< 0.05）。
- 不准在大面積元素或 section 使用鮮豔主色背景（例如不要有亮藍、亮綠或亮紅的 hero section）。
- 不准使用漸層、螢光色或 3D glassmorphism（僅允許 navbar 的細微模糊效果）。
- 不准對大型容器、卡片或主要按鈕使用 `rounded-full`（膠囊形）。
- 不准在程式碼、標記、文字內容、標題或 alt 文字中的任何地方使用 emoji。改用正式的 icon 或乾淨的 SVG 基本圖形。
- 不准使用「John Doe」、「Acme Corp」或「Lorem Ipsum」這類泛用佔位名稱。改用貼近情境的真實內容。
- 不准使用 AI 文案陳腔濫調：「Elevate」、「Seamless」、「Unleash」、「Next-Gen」、「Game-changer」、「Delve」。寫直白、具體的語言。

## 3. 字體排印架構
介面必須仰賴極端的字體排印對比與高級字型選擇，來建立編輯風的質感。
- 主要無襯線字型（內文、UI、按鈕）：使用乾淨、幾何或有個性的系統原生字型。目標：`font-family: 'SF Pro Display', 'Geist Sans', 'Helvetica Neue', 'Switzer', sans-serif`。
- 編輯風襯線字型（hero 標題與引言）：目標：`font-family: 'Lyon Text', 'Newsreader', 'Playfair Display', 'Instrument Serif', serif`。套用緊湊字距（`letter-spacing: -0.02em` 到 `-0.04em`）與緊湊行高（`1.1`）。
- 等寬字型（程式碼、按鍵、中繼資料）：目標：`font-family: 'Geist Mono', 'SF Mono', 'JetBrains Mono', monospace`。
- 文字顏色：內文絕不能用純黑（`#000000`）。使用近黑／炭灰色（`#111111` 或 `#2F3437`），並搭配寬鬆的 `line-height`（`1.6`）確保易讀性。次要文字使用柔和灰（`#787774`）。

## 4. 色盤（暖色調單色 + 局部粉彩點綴）
顏色是稀缺資源，只用於語意表達或細微點綴。
- 畫布／背景：純白 `#FFFFFF`，或暖骨白／米白 `#F7F6F3` / `#FBFBFA`。
- 主要表面（卡片）：`#FFFFFF` 或 `#F9F9F8`。
- 結構性邊框／分隔線：超淺灰 `#EAEAEA` 或 `rgba(0,0,0,0.06)`。
- 點綴色：僅使用高度去飽和、洗白感的粉彩色，用在 tag、行內程式碼背景或細微的 icon 背景。
  - 淡紅：`#FDEBEC`（文字：`#9F2F2D`）
  - 淡藍：`#E1F3FE`（文字：`#1F6C9F`）
  - 淡綠：`#EDF3EC`（文字：`#346538`）
  - 淡黃：`#FBF3DB`（文字：`#956400`）

## 5. 元件規格
- Bento Box 功能格（feature grid）：
  - 採用不對稱的 CSS Grid 版面。
  - 卡片邊框必須精確為 `border: 1px solid #EAEAEA`。
  - border-radius 必須俐落：最多 `8px` 或 `12px`。
  - 內距必須充裕（例如 `24px` 到 `40px`）。
- 主要 CTA（按鈕）：
  - 實心背景 `#111111`，文字 `#FFFFFF`。
  - 輕微的 border-radius（`4px` 到 `6px`）。不加 box-shadow。
  - hover 狀態應是細微的顏色變化到 `#333333`，或微縮放 `transform: scale(0.98)`。
- Tag 與狀態徽章：
  - 膠囊形（`border-radius: 9999px`），極小字級（`text-xs`），大寫加寬字距（`letter-spacing: 0.05em`）。
  - 背景必須使用前述定義的低飽和粉彩色。
- 手風琴（FAQ）：
  - 拿掉所有容器外框。項目之間只用 `border-bottom: 1px solid #EAEAEA` 分隔。
  - 開合狀態用乾淨、銳利的 `+` 與 `-` icon。
- 按鍵微型 UI：
  - 用 `<kbd>` 標籤把快捷鍵渲染成實體按鍵：`border: 1px solid #EAEAEA`、`border-radius: 4px`、`background: #F7F6F3`，並使用等寬字型。
- 仿 OS 視窗外框（Faux-OS Window Chrome）：
  - 模擬軟體畫面時，包在一個極簡容器內，頂部白色橫條放三顆淺灰小圓點（重現 macOS 視窗控制鈕）。

## 6. Icon 與影像指令
- 系統 icon：使用「Phosphor Icons（Bold 或 Fill 字重）」或「Radix UI Icons」，營造技術感、筆畫略粗的美學。所有 icon 的筆畫粗細必須統一。
- 插畫：單色、粗獷的連續線條墨水速寫，置於白色背景上，搭配單一偏移的幾何形狀並填入低飽和粉彩色。
- 攝影：使用高品質、去飽和的暖色調影像。套用細微疊層（`opacity: 0.04` 的暖色顆粒）讓照片融入單色色盤。絕不使用過飽和的 stock 照片。找不到真實素材時，使用可靠的佔位圖，如 `https://picsum.photos/seed/{context}/1200/800`。
- Hero 與 section 背景：section 不應顯得空洞扁平。使用極低不透明度的細微全寬背景影像、柔和的放射狀光斑（暖色調 `radial-gradient`、`opacity: 0.03`），或極簡的幾何線條紋樣來增加深度，同時不破壞乾淨的美學。

## 7. 細微動態與微動畫
動態應該「隱形」——存在但絕不搶戲。目標是安靜的高級感，不是炫技。
- 捲動進場：元素進入 viewport 時輕柔淡入。使用 `translateY(12px)` + `opacity: 0`，在 `600ms` 內以 `cubic-bezier(0.16, 1, 0.3, 1)` 完成。使用 `IntersectionObserver`，絕不使用 `window.addEventListener('scroll')`。
- Hover 狀態：卡片以超細微的陰影變化抬升（`box-shadow` 從 `0 0 0` 過渡到 `0 2px 8px rgba(0,0,0,0.04)`，歷時 `200ms`）。按鈕在 `:active` 時以 `scale(0.98)` 回應。
- 交錯進場：清單與 grid 項目以階梯延遲進場（`animation-delay: calc(var(--index) * 80ms)`）。絕不一次全部掛載。
- 背景環境動態：可選。單一、移動極慢的放射狀漸層色塊（`animation-duration: 20s+`、`opacity: 0.02-0.04`）在 hero section 後方漂移。必須套在 `position: fixed; pointer-events: none` 圖層上。絕不放在會捲動的容器上。
- 效能：只透過 `transform` 與 `opacity` 做動畫。不使用會觸發 layout 的屬性（`top`、`left`、`width`、`height`）。`will-change: transform` 節制使用，且只用在正在動的元素上。

## 8. 執行 Protocol
接到撰寫前端程式碼（HTML、React、Tailwind、Vue）或設計版面的任務時：
1. 先建立大尺度留白。section 之間使用大量垂直內距（例如 Tailwind 的 `py-24` 或 `py-32`）。
2. 將主要文字內容寬度限制在 `max-w-4xl` 或 `max-w-5xl`。
3. 立刻套用客製的字體排印階層與單色色彩變數。
4. 確保每張卡片、每條分隔線與邊框都嚴格遵守 `1px solid #EAEAEA` 規則。
5. 為所有主要內容區塊加上捲動進場動畫。
6. 確保 section 透過影像、環境漸層或細微紋理具有視覺深度——不要空洞扁平的背景。
7. 產出的程式碼要原生體現這種高級、不雜亂的編輯風美學，無需手動調整。
