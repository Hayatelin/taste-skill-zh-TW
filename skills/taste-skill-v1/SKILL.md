---
name: design-taste-frontend-v1
description: 原始 v1 版 taste-skill，為依賴其精確行為的專案保留。目前預設版本是 `design-taste-frontend`（v2 實驗版），屬於大幅改寫。只有在需要完全向下相容時才使用這個 v1 安裝名稱。
---

# High-Agency Frontend Skill（高自主性前端 Skill）

## 1. 現行基準設定（ACTIVE BASELINE CONFIGURATION）
* DESIGN_VARIANCE: 8（1=完美對稱，10=藝術性混沌）
* MOTION_INTENSITY: 6（1=靜態/無動效，10=電影級/魔法物理）
* VISUAL_DENSITY: 4（1=美術館/空靈，10=飛機駕駛艙/資料密集）

**AI 指令：**所有產出的標準基準嚴格設定為這些值（8、6、4）。不要要求使用者編輯此檔案。除此之外，永遠聽從使用者：根據他們在對話 prompt 中明確要求的內容，動態調整這些值。以這些基準值（或使用者覆寫後的值）作為全域變數，驅動第 3 到第 7 節的具體邏輯。

## 2. 預設架構與慣例（DEFAULT ARCHITECTURE & CONVENTIONS）
除非使用者明確指定不同的技術堆疊，否則遵守以下結構性約束以維持一致性：

* **相依套件驗證【強制】：**在 import 任何第三方函式庫（例如 `framer-motion`、`lucide-react`、`zustand`）之前，你必須先檢查 `package.json`。如果套件不存在，你必須在提供程式碼之前先輸出安裝指令（例如 `npm install package-name`）。**絕不**假設函式庫已存在。
* **框架與互動性：**React 或 Next.js。預設使用 Server Components（`RSC`）。
    * **RSC 安全守則：**全域狀態只能在 Client Components 中運作。在 Next.js 中，把 provider 包進一個 `"use client"` 元件裡。
    * **互動性隔離：**如果第 4 或第 7 節（Motion/Liquid Glass）啟用，該互動 UI 元件必須抽出成獨立的葉節點元件，並在檔案最頂端加上 `'use client'`。Server Components 只能負責渲染靜態版面。
* **狀態管理：**孤立的 UI 使用本地 `useState`/`useReducer`。全域狀態只用來避免深層 prop-drilling。
* **樣式策略：**90% 的樣式使用 Tailwind CSS（v3/v4）。
    * **TAILWIND 版本鎖定：**先檢查 `package.json`。不要在 v3 專案中使用 v4 語法。
    * **T4 設定守則：**v4 專案中，不要在 `postcss.config.js` 使用 `tailwindcss` plugin。改用 `@tailwindcss/postcss` 或 Vite plugin。
* **禁用 EMOJI 政策【關鍵】：**絕不在程式碼、標記、文字內容或 alt 文字中使用 emoji。改用高品質圖示（Radix、Phosphor）或乾淨的 SVG 基本圖形取代符號。Emoji 一律禁用。
* **響應式與間距：**
  * 統一斷點（`sm`、`md`、`lg`、`xl`）。
  * 頁面版面用 `max-w-[1400px] mx-auto` 或 `max-w-7xl` 收束。
  * **視口穩定性【關鍵】：**全高 Hero 區塊絕不使用 `h-screen`。永遠使用 `min-h-[100dvh]`，避免行動瀏覽器（iOS Safari）上災難性的版面跳動。
  * **Grid 優先於 Flex 算式：**絕不使用複雜的 flexbox 百分比運算（`w-[calc(33%-1rem)]`）。永遠使用 CSS Grid（`grid grid-cols-1 md:grid-cols-3 gap-6`）建立可靠的結構。
* **圖示：**你必須精確使用 `@phosphor-icons/react` 或 `@radix-ui/react-icons` 作為 import 路徑（檢查已安裝版本）。全域統一 `strokeWidth`（例如只用 `1.5` 或 `2.0`）。


## 3. 設計工程指令（偏誤修正）
LLM 對特定 UI 陳腔濫調模式有統計性偏誤。請主動運用以下工程化規則建構高級介面：

**規則 1：確定性字體排印（Deterministic Typography）**
* **Display/標題：**預設 `text-4xl md:text-6xl tracking-tighter leading-none`。
    * **反 SLOP：**「高級感」或「創意感」的場合不建議用 `Inter`。強制使用 `Geist`、`Outfit`、`Cabinet Grotesk` 或 `Satoshi` 塑造獨特個性。
    * **技術類 UI 規則：**Dashboard/軟體 UI 嚴禁使用襯線字體（Serif）。這類情境只能使用高階無襯線（Sans-Serif）配對（`Geist` + `Geist Mono` 或 `Satoshi` + `JetBrains Mono`）。
* **內文/段落：**預設 `text-base text-gray-600 leading-relaxed max-w-[65ch]`。

**規則 2：色彩校準（Color Calibration）**
* **限制：**最多 1 個強調色（Accent Color）。飽和度 < 80%。
* **紫色禁令（THE LILA BAN）：**「AI 紫/藍」美學嚴格禁用。不要紫色按鈕光暈，不要霓虹漸層。使用絕對中性的基底（Zinc/Slate），搭配高對比、單一的強調色（例如 Emerald、Electric Blue 或 Deep Rose）。
* **色彩一致性：**整份輸出堅持同一套色盤。同一個專案內不要在暖灰與冷灰之間搖擺。

**規則 3：版面多樣化（Layout Diversification）**
* **反置中偏誤：**當 `DESIGN_VARIANCE > 4` 時，置中的 Hero/H1 區塊嚴格禁用。強制採用「Split Screen」（50/50）、「左對齊內容/右對齊素材」或「非對稱留白」結構。

**規則 4：材質、陰影與「反卡片濫用」**
* **DASHBOARD 強化：**當 `VISUAL_DENSITY > 7` 時，泛用的卡片容器嚴格禁用。改用 `border-t`、`divide-y` 做邏輯分組，或純粹以負空間（negative space）處理。除非功能上真的需要高度感（z-index），資料指標應該自由呼吸，而不是被框起來。
* **執行方式：**只有當高度感（elevation）能傳達層級時才使用卡片。使用陰影時，將其染上背景色調。

**規則 5：互動式 UI 狀態**
* **強制產出：**LLM 天生只產「靜態」的成功狀態。你必須實作完整的互動循環：
  * **載入中：**與版面尺寸相符的 skeleton loader（避免泛用的圓形 spinner）。
  * **空狀態（Empty States）：**構圖優美的空狀態，指引使用者如何填入資料。
  * **錯誤狀態：**清楚的行內錯誤回報（例如表單）。
  * **觸覺回饋：**在 `:active` 時使用 `-translate-y-[1px]` 或 `scale-[0.98]` 模擬實體按壓，傳達成功/動作。

**規則 6：資料與表單模式**
* **表單：**Label 必須位於 input 上方。輔助文字可有可無，但應存在於標記中。錯誤文字放 input 下方。input 區塊統一使用 `gap-2`。

## 4. 創意主動性（反 Slop 實作）
為了主動對抗千篇一律的 AI 設計，系統性地把以下高階程式概念實作為你的基準：
* **「Liquid Glass」折射：**需要 glassmorphism 時，不要只做 `backdrop-blur`。加上 1px 內邊框（`border-white/10`）與細微的內陰影（`shadow-[inset_0_1px_0_rgba(255,255,255,0.1)]`）模擬實體邊緣折射。
* **磁吸微物理（若 MOTION_INTENSITY > 5）：**實作會微微被滑鼠游標吸引的按鈕。**關鍵：**磁吸 hover 或連續動畫絕不使用 React `useState`。只能使用 Framer Motion 的 `useMotionValue` 與 `useTransform`，在 React 渲染循環之外運作，避免行動裝置上的效能崩潰。
* **恆動微互動（Perpetual Micro-Interactions）：**當 `MOTION_INTENSITY > 5` 時，在標準元件（頭像、狀態點、背景）中嵌入連續、無限循環的微動畫（Pulse、Typewriter、Float、Shimmer、Carousel）。所有互動元素套用高級 Spring 物理（`type: "spring", stiffness: 100, damping: 20`）——不使用線性 easing。
* **版面轉場（Layout Transitions）：**永遠利用 Framer Motion 的 `layout` 與 `layoutId` prop，實現流暢的重排、縮放與跨狀態的共享元素轉場。
* **交錯編排（Staggered Orchestration）：**清單或網格不要瞬間掛載。使用 `staggerChildren`（Framer）或 CSS 級聯（`animation-delay: calc(var(--index) * 100ms)`）製造依序瀑布式顯現。**關鍵：**使用 `staggerChildren` 時，父層（`variants`）與子層必須位於同一棵 Client Component 樹中。若資料是非同步取得，將資料以 props 傳入集中式的父層 Motion 包裝元件。

## 5. 效能護欄（PERFORMANCE GUARDRAILS）
* **DOM 成本：**顆粒/雜訊濾鏡只能套用在 fixed、pointer-event-none 的偽元素上（例如 `fixed inset-0 z-50 pointer-events-none`），絕不能套在會捲動的容器上，以避免持續的 GPU 重繪與行動裝置效能劣化。
* **硬體加速：**絕不對 `top`、`left`、`width`、`height` 做動畫。動畫只透過 `transform` 與 `opacity`。
* **Z-Index 克制：**絕不無故濫用 `z-50` 或 `z-10`。z-index 只用於系統性的圖層情境（Sticky 導覽列、Modal、Overlay）。

## 6. 技術參考（旋鈕定義）

### DESIGN_VARIANCE（等級 1-10）
* **1-3（可預測）：**Flexbox `justify-center`、嚴格的 12 欄對稱網格、相等的 padding。
* **4-7（偏移）：**使用 `margin-top: -2rem` 疊合、多樣的圖片長寬比（例如 4:3 緊鄰 16:9）、左對齊標題搭配置中的資料。
* **8-10（非對稱）：**Masonry 版面、使用分數單位的 CSS Grid（例如 `grid-template-columns: 2fr 1fr 1fr`）、大面積留白區（`padding-left: 20vw`）。
* **行動裝置覆寫：**等級 4-10 中，任何 `md:` 以上的非對稱版面在 `< 768px` 視口必須強勢退回嚴格的單欄版面（`w-full`、`px-4`、`py-8`），避免水平捲動與版面破損。

### MOTION_INTENSITY（等級 1-10）
* **1-3（靜態）：**沒有自動動畫。只有 CSS `:hover` 與 `:active` 狀態。
* **4-7（流暢 CSS）：**使用 `transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1)`。載入動畫使用 `animation-delay` 級聯。嚴格聚焦 `transform` 與 `opacity`。`will-change: transform` 節制使用。
* **8-10（進階編舞）：**複雜的捲動觸發顯現或視差（parallax）。使用 Framer Motion hooks。絕不使用 `window.addEventListener('scroll')`。

### VISUAL_DENSITY（等級 1-10）
* **1-3（美術館模式）：**大量留白。巨大的區塊間距。一切都顯得非常昂貴、乾淨。
* **4-7（日常 App 模式）：**標準 web app 的正常間距。
* **8-10（駕駛艙模式）：**極小的 padding。不用卡片框；只用 1px 線條分隔資料。一切都很緊湊。**強制：**所有數字使用等寬字體（`font-mono`）。

## 7. AI 痕跡（禁用模式）
為了確保輸出高級、不落俗套，除非使用者明確要求，你必須嚴格避免以下常見的 AI 設計特徵：

### 視覺與 CSS
* **禁止霓虹/外發光：**不要使用預設的 `box-shadow` 光暈或自動發光。改用內邊框或細微的染色陰影。
* **禁止純黑：**絕不使用 `#000000`。改用 Off-Black、Zinc-950 或 Charcoal。
* **禁止過飽和強調色：**降低強調色飽和度，讓它優雅地融入中性色。
* **禁止過度的漸層文字：**大型標題不要使用文字填色漸層。
* **禁止自訂滑鼠游標：**已經過時，且會摧毀效能與無障礙性。

### 字體排印
* **禁用 Inter 字體：**一律禁止。改用 `Geist`、`Outfit`、`Cabinet Grotesk` 或 `Satoshi`。
* **禁止過大的 H1：**第一個標題不該用吼的。用字重與顏色控制層級，不要只靠巨大的字級。
* **襯線字體限制：**Serif 只能用於創意/編輯類設計。**絕不**在乾淨的 Dashboard 上使用 Serif。

### 版面與間距
* **對齊與間距要完美：**確保 padding 與 margin 在數學上完美。避免帶著彆扭空隙的漂浮元素。
* **禁止 3 欄卡片版面：**泛用的「3 張等寬卡片橫排」功能列一律禁止。改用 2 欄 Zig-Zag、非對稱網格或水平捲動的做法。

### 內容與資料（「Jane Doe」效應）
* **禁止泛用名字：**「John Doe」、「Sarah Chan」、「Jack Su」一律禁止。使用高度創意、聽起來真實的名字。
* **禁止泛用頭像：**不要用標準 SVG「蛋形」或 Lucide user 圖示當頭像。使用有創意、可信的照片佔位圖或特定的風格處理。
* **禁止假數字：**避免可預測的輸出如 `99.99%`、`50%` 或陽春的電話號碼（`1234567`）。使用有機、帶點凌亂感的資料（`47.2%`、`+1 (312) 847-1928`）。
* **禁止新創 Slop 名稱：**「Acme」、「Nexus」、「SmartFlow」。發明高級、貼合情境的品牌名稱。
* **禁止填充詞：**避免 AI 文案陳腔濫調如「Elevate」、「Seamless」、「Unleash」、「Next-Gen」。使用具體的動詞。

### 外部資源與元件
* **禁止失效的 Unsplash 連結：**不要使用 Unsplash。使用絕對可靠的佔位圖，如 `https://picsum.photos/seed/{random_string}/800/600` 或 SVG UI Avatars。
* **shadcn/ui 客製化：**你可以使用 `shadcn/ui`，但絕不能用它泛用的預設狀態。你必須客製化圓角（radii）、顏色與陰影，以符合高階專案美學。
* **正式環境等級的乾淨度：**程式碼必須極度乾淨、視覺震撼、令人難忘，且每個細節都經過精雕細琢。

## 8. 創意軍火庫（高階靈感）
不要預設產出泛用 UI。從這座進階概念庫中取材，確保輸出在視覺上震撼且令人難忘。適當時機，複雜的 scrolltelling 用 **GSAP（ScrollTrigger/Parallax）**、3D/Canvas 動畫用 **ThreeJS/WebGL**，而不是陽春的 CSS 動效。**關鍵：**絕不在同一棵元件樹中混用 GSAP/ThreeJS 與 Framer Motion。UI/Bento 互動預設用 Framer Motion。GSAP/ThreeJS 只用於獨立的整頁 scrolltelling 或 canvas 背景，並以嚴格的 useEffect cleanup 區塊包裝。

### 標準 Hero 範式
* 別再做深色圖片上的置中文字了。試試非對稱 Hero 區塊：文字乾淨地靠左或靠右對齊。背景使用高品質、切題的圖片，並帶著細膩的風格化淡出（依 Light 或 Dark 模式，優雅地暗化或亮化融入背景色）。

### 導覽與選單
* **Mac OS Dock 放大效果：**貼齊邊緣的導覽列；圖示在 hover 時流暢縮放。
* **磁吸按鈕（Magnetic Button）：**會實際被游標吸過去的按鈕。
* **Gooey Menu：**子項目像黏稠液體般從主按鈕分離出來。
* **Dynamic Island：**藥丸形 UI 元件，會變形以顯示狀態/通知。
* **情境式放射選單（Contextual Radial Menu）：**在點擊座標精確展開的圓形選單。
* **懸浮快速撥號（Floating Speed Dial）：**FAB 彈開成一條弧線排列的次要動作。
* **Mega Menu 展開：**全螢幕下拉選單，交錯淡入複雜內容。

### 版面與網格
* **Bento Grid：**非對稱、磚塊式的分組（例如 Apple 控制中心）。
* **Masonry 版面：**列高不固定的交錯網格（例如 Pinterest）。
* **Chroma Grid：**網格邊框或磚塊帶著細微、持續變動的色彩漸層。
* **Split Screen Scroll：**捲動時螢幕兩半往相反方向滑動。
* **Curtain Reveal：**Hero 區塊隨捲動像布幕一樣從中間拉開。

### 卡片與容器
* **Parallax Tilt Card：**追蹤滑鼠座標的 3D 傾斜卡片。
* **Spotlight Border Card：**卡片邊框在游標下方動態發亮。
* **Glassmorphism Panel：**真正的磨砂玻璃，帶內折射邊框。
* **Holographic Foil Card：**hover 時流轉的虹彩光澤反射。
* **Tinder Swipe Stack：**可以實際滑掉的卡片堆疊。
* **Morphing Modal：**按鈕無縫展開成自己的全螢幕對話框容器。

### 捲動動畫
* **Sticky Scroll Stack：**卡片黏在頂端，實體般層層堆疊。
* **Horizontal Scroll Hijack：**垂直捲動轉換為流暢的水平藝廊平移。
* **Locomotive Scroll Sequence：**影格率直接綁定捲軸的影片/3D 序列。
* **Zoom Parallax：**中央背景圖隨捲動無縫放大/縮小。
* **Scroll Progress Path：**隨使用者捲動自行描繪的 SVG 向量線條或路徑。
* **Liquid Swipe Transition：**像黏稠液體般擦過螢幕的頁面轉場。

### 藝廊與媒體
* **Dome Gallery：**宛如全景穹頂的 3D 藝廊。
* **Coverflow Carousel：**中央聚焦、兩側向後傾斜的 3D 輪播。
* **Drag-to-Pan Grid：**可朝任意方向自由拖曳的無邊界網格。
* **Accordion Image Slider：**窄長的直/橫向圖片條，hover 時完全展開。
* **Hover Image Trail：**滑鼠身後留下一串彈出/淡出的圖片軌跡。
* **Glitch Effect Image：**hover 時短暫的 RGB 通道偏移數位失真。

### 字體排印與文字
* **Kinetic Marquee：**無盡的文字跑馬燈，隨捲動反轉方向或加速。
* **Text Mask Reveal：**巨型字體排印作為透明窗口，透出影片背景。
* **Text Scramble Effect：**載入或 hover 時的駭客任務式字元解碼。
* **Circular Text Path：**沿旋轉圓形路徑彎曲排列的文字。
* **Gradient Stroke Animation：**描邊文字，漸層沿筆畫持續流動。
* **Kinetic Typography Grid：**一格格字母閃避游標或隨之旋轉的網格。

### 微互動與特效
* **Particle Explosion Button：**成功時碎裂成粒子的 CTA。
* **Liquid Pull-to-Refresh：**行動裝置重新整理指示器像脫落的水滴。
* **Skeleton Shimmer：**佔位框上流動的光澤反射。
* **Directional Hover Aware Button：**hover 填色從滑鼠實際進入的那一側流入。
* **Ripple Click Effect：**從點擊座標精確擴散的視覺波紋。
* **Animated SVG Line Drawing：**即時描繪自身輪廓的向量圖。
* **Mesh Gradient Background：**有機、熔岩燈般流動的色彩團塊。
* **Lens Blur Depth：**動態失焦模糊背景 UI 圖層，凸顯前景動作。

## 9. 「MOTION-ENGINE」BENTO 範式
產生現代 SaaS dashboard 或功能區塊時，你必須採用以下「Bento 2.0」架構與動效哲學。這超越了靜態卡片，強制實現「Vercel 核心 x Dribbble 潔淨」、高度依賴恆動物理的美學。

### A. 核心設計哲學
* **美學：**高階、極簡、實用。
* **色盤：**背景 `#f9fafb`。卡片純白（`#ffffff`），搭配 1px 的 `border-slate-200/50` 邊框。
* **表面：**所有主要容器使用 `rounded-[2.5rem]`。套用「擴散陰影」（極淡、大範圍擴散的陰影，例如 `shadow-[0_20px_40px_-15px_rgba(0,0,0,0.05)]`）創造不雜亂的深度。
* **字體排印：**嚴格使用 `Geist`、`Satoshi` 或 `Cabinet Grotesk` 字體堆疊。標題用細膩的字距（`tracking-tight`）。
* **標籤：**標題與說明必須放在卡片**外部下方**，維持乾淨、藝廊式的呈現。
* **像素級完美：**卡片內部使用充裕的 `p-8` 或 `p-10` padding。

### B. 動畫引擎規格（恆動）
所有卡片都必須包含**「恆動微互動」**。遵循以下 Framer Motion 原則：
* **Spring 物理：**不用線性 easing。使用 `type: "spring", stiffness: 100, damping: 20` 營造高級、有重量的手感。
* **版面轉場：**大量運用 `layout` 與 `layoutId` prop，確保流暢的重排、縮放與共享元素狀態轉場。
* **無限循環：**每張卡片都必須有無限循環的「Active State」（Pulse、Typewriter、Float 或 Carousel），確保 dashboard 感覺「活著」。
* **效能：**動態清單包在 `<AnimatePresence>` 中並最佳化到 60fps。**效能關鍵：**任何恆動或無限循環動畫都必須 memoize（React.memo），並完全隔離在自己微小的 Client Component 中。絕不觸發父層版面的重新渲染。

### C. 五大卡片原型（微動畫規格）
建構 Bento grid（例如第 1 列 3 欄 | 第 2 列 70/30 分割 2 欄）時，實作這些特定的微動畫：
1. **智慧清單（The Intelligent List）：**垂直堆疊的項目，帶無限自動排序循環。項目用 `layoutId` 互換位置，模擬 AI 即時排定任務優先序。
2. **指令輸入框（The Command Input）：**帶多段式打字機效果的搜尋/AI 輸入列。它循環展示複雜的 prompt，包含閃爍游標與帶著微光載入漸層的「處理中」狀態。
3. **即時狀態（The Live Status）：**帶「呼吸感」狀態指示燈的排程介面。包含一個以「Overshoot」spring 效果彈出的通知徽章，停留 3 秒後消失。
4. **寬幅資料流（The Wide Data Stream）：**資料卡片或指標的水平「無限輪播」。確保循環無縫（使用 `x: ["0%", "-100%"]`），速度要有毫不費力的感覺。
5. **情境 UI／專注模式（The Contextual UI）：**文件檢視畫面，先交錯地醒目標示一段文字，接著讓帶微圖示的浮動操作工具列「飄入」。

## 10. 最終起飛前檢查（FINAL PRE-FLIGHT CHECK）
輸出前用這個矩陣檢視你的程式碼。這是你套用在邏輯上的**最後**一道濾網。
- [ ] 全域狀態是為了避免深層 prop-drilling 而恰當使用，而非隨意使用？
- [ ] 高變異度設計是否保證了行動裝置的版面收合（`w-full`、`px-4`、`max-w-7xl mx-auto`）？
- [ ] 全高區塊是否安全地使用 `min-h-[100dvh]` 而非有 bug 的 `h-screen`？
- [ ] `useEffect` 動畫是否都有嚴格的 cleanup 函式？
- [ ] 是否提供了空狀態、載入與錯誤狀態？
- [ ] 能用留白取代卡片的地方是否都省略了卡片？
- [ ] 吃 CPU 的恆動動畫是否嚴格隔離在自己的 Client Components 中？
