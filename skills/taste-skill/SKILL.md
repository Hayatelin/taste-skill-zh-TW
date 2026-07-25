---
name: design-taste-frontend
description: 防止 AI 味（anti-slop）的前端設計 skill，適用於 landing page、作品集與改版案。Agent 會先讀懂需求、推斷正確的設計方向，並產出不像套範本的介面。適用時採用真實的設計系統、改版案先做盤點稽核、出貨前執行嚴格的 pre-flight 檢查。Anti-slop frontend skill for landing pages, portfolios, and redesigns.
---

# tasteskill：Anti-Slop 前端設計 Skill

> 適用範圍：landing page、作品集（portfolio）、改版（redesign）。不適用於 dashboard、資料表格、多步驟產品 UI。
> 以下每條規則都是**情境式的**，沒有任何一條會自動觸發。先讀懂需求，再挑出真正適用的部分。

---

## 0. 需求推斷（BRIEF INFERENCE：動手前先讀懂現場）

在碰程式碼或調整任何參數之前，先**推斷使用者真正想要什麼**。大多數 LLM 設計產出之所以糟糕，是因為模型直接跳到預設美學，而不是先讀懂現場。

### 0.A 先讀這些訊號
1. **頁面類型**——landing page（SaaS / 消費性 / agency / 活動）、作品集（開發者 / 設計師 / 創意工作室）、改版（保留 vs 全面翻新）、editorial / 部落格。
2. **使用者用到的氛圍詞**——「minimalist」、「calm」、「Linear 風」、「Awwwards」、「brutalist」、「premium consumer」、「Apple 感」、「playful」、「serious B2B」、「editorial」、「agency 味」、「glassy」、「dark tech」。
3. **參考訊號**——使用者附的 URL、貼的截圖、點名的產品、正在競爭的品牌。
4. **受眾**——B2B 採購委員會 vs. 有設計品味的消費者 vs. 快速掃過作品集的招募主管。美學由受眾決定，不是由你的品味決定。
5. **既有的品牌資產**——logo、色彩、字型、攝影。對改版案來說，這些是起始素材，不是可有可無的輸入（見第 11 節）。
6. **隱性限制**——無障礙優先的受眾、公部門、受監管產業、以信任為先的電商、兒童產品。這些限制**凌駕於**美學偏好之上。

### 0.B 產出前先給一行「設計判讀」（Design Read）
在寫任何程式碼之前，先用一行話陳述：**「我把這個案子讀作：給〈受眾〉的〈頁面類型〉，採用〈氛圍〉語彙，傾向〈設計系統或美學家族〉。」**

判讀範例：
- *「我把這個案子讀作：給技術買家的 B2B SaaS landing page，採用 Linear 風的 minimalist 語彙，傾向 Tailwind utilities + Geist + 克制的動態。」*
- *「我把這個案子讀作：給招募主管看的個人設計師作品集，採用 editorial / kinetic-type 語彙，傾向原生 CSS + scroll-driven animation + 客製字體排印。」*
- *「我把這個案子讀作：公部門服務網站改版，採用以信任為先的語彙，傾向 GOV.UK Frontend 或 USWDS。」*

### 0.C 需求模糊時，問一個問題，不要用猜的
只問**一個**釐清問題——絕不一次丟一堆問題——而且只在設計判讀真的出現分歧時才問。範例：*「這個應該偏 Linear 式的乾淨，還是 Awwwards 式的實驗性？」*

如果能從上下文有把握地推斷出來，就**不要問**。直接宣告設計判讀然後繼續。

### 0.D 反預設紀律（Anti-Default Discipline）
不要預設使用：AI 紫漸層、深色 mesh 上置中的 hero、三張等寬 feature card、到處套 generic glassmorphism、滿頁無限循環微動畫、Inter + slate-900。這些是 LLM 的預設值。要根據設計判讀，刻意伸手越過它們。

---

## 1. 三個轉盤（THE THREE DIALS：核心設定）

完成設計判讀後，設定三個轉盤（dial）。下面所有版面（layout）、動態、密度的決策都由這三個值把關。

* **`DESIGN_VARIANCE: 8`**——1 = 完美對稱，10 = 藝術式混亂
* **`MOTION_INTENSITY: 6`**——1 = 靜態，10 = 電影感 / 物理模擬
* **`VISUAL_DENSITY: 4`**——1 = 美術館 / 留白通透，10 = 駕駛艙 / 資料密集

**基準值：**`8 / 6 / 4`。除非設計判讀另有指示，否則使用這組。不要叫使用者來改這個檔案——調整值的覆寫在對話中進行。

### 1.A 轉盤推斷（設計判讀 → 轉盤值）
| 訊號 | VARIANCE | MOTION | DENSITY |
|---|---|---|---|
| 「minimalist / clean / calm / editorial / Linear 風」 | 5-6 | 3-4 | 2-3 |
| 「premium consumer / Apple 感 / 奢華 / 品牌向」 | 7-8 | 5-7 | 3-4 |
| 「playful / wild / Dribbble / Awwwards / 實驗性 / agency」 | 9-10 | 8-10 | 3-4 |
| 「landing page / 作品集 / 行銷網站（預設）」 | 7-9 | 6-8 | 3-5 |
| 「信任為先 / 公部門 / 受監管 / 無障礙關鍵」 | 3-4 | 2-3 | 4-5 |
| 「改版——保留」 | 比照現況 | +1 | 比照現況 |
| 「改版——翻新」 | +2 | +2 | 比照現況 |

### 1.B 使用情境預設組
| 使用情境 | VARIANCE | MOTION | DENSITY |
|---|---|---|---|
| Landing（SaaS，主流） | 7 | 6 | 4 |
| Landing（Agency / 創意） | 9 | 8 | 3 |
| Landing（Premium consumer） | 7 | 6 | 3 |
| 作品集（設計師 / 工作室） | 8 | 7 | 3 |
| 作品集（開發者） | 6 | 5 | 4 |
| Editorial / 部落格 | 6 | 4 | 3 |
| 公部門服務 | 3 | 2 | 5 |
| 改版——保留 | 比照 | 比照+1 | 比照 |
| 改版——翻新 | +2 | +2 | 比照 |

### 1.C 轉盤如何驅動輸出
把這些值（或使用者覆寫後的值）當作全域變數。本文件各處的交叉引用都指向這些確切的變數名稱——絕不要自創別名，例如 `LAYOUT_VARIANCE` 或 `ANIM_LEVEL`。

---

## 2. 需求 → 設計系統對照表

有了設計判讀（第 0 節）和轉盤值（第 1 節）後，挑選正確的基礎。已有官方套件的東西不要自己發明 CSS；也不要把美學潮流假裝成官方系統。

### 2.A 何時該用真正的設計系統（使用官方套件）
| 需求讀起來像…… | 使用 | 原因 |
|---|---|---|
| Microsoft / 企業 SaaS / dashboard | `@fluentui/react-components` 或 `@fluentui/web-components` | 官方 Fluent UI、Microsoft tokens、無障礙已做好 |
| Google 風 UI、Material 味的產品 | `@material/web` + Material 3 tokens | 官方套件，可透過 Material Theming 客製主題 |
| IBM 風 B2B / 企業分析 | `@carbon/react` + `@carbon/styles` | 官方 Carbon，成熟的資料密度模式 |
| Shopify 應用介面 | `polaris.js` web components / Polaris React | Shopify admin UI 的必要選擇 |
| Atlassian / Jira 風產品 | `@atlaskit/*` + `@atlaskit/tokens` | 官方 Atlassian DS |
| GitHub 風開發工具 / 社群頁 | `@primer/css` 或 `@primer/react-brand` | 官方 Primer；行銷頁用 Brand 變體 |
| 英國公部門服務 | `govuk-frontend` | 法規 / 監理上的預期 |
| 美國公部門 / 信任為先 | `uswds` | 同上 |
| 快速的在地商家 / agency MVP | Bootstrap 5.3 | 無聊、快速、能用 |
| 現代化無障礙 React 基礎 | `@radix-ui/themes` | Primitives + 打磨過的主題 |
| 元件要自己掌控的現代 SaaS | shadcn/ui（`npx shadcn@latest add ...`） | 程式碼歸你所有、容易客製；絕不以預設狀態出貨 |
| Tailwind 基底的現代 SaaS / AI 行銷頁 | Tailwind v4 utilities + `dark:` variant | 獨立開發者與小團隊的預設選擇 |

**誠實規則：**如果需求讀起來就是上面某個系統，就安裝並使用**官方**套件。不要手刻重現它的 CSS。也不要匯入某系統的 tokens 卻覆寫掉九成。

**一個專案一個系統。**不要在同一棵元件樹裡混用 Fluent React 和 Carbon。不要把 shadcn/ui 元件匯進 Material 3 應用。

### 2.B 當需求是一種美學、而不是一個系統時
以下這些方向**沒有單一官方套件**。用原生 CSS + Tailwind + 有維護的元件庫來實作。在程式碼註解裡誠實標明哪些是借用的靈感、哪些是官方素材。

| 美學 | 誠實的實作方式 |
|---|---|
| Glassmorphism /「毛玻璃」 | `backdrop-filter`、多層邊框、highlight 疊層。為 `prefers-reduced-transparency` 提供實色 fallback。 |
| Bento（Apple 風磁磚格） | CSS Grid 搭配混合尺寸的格子。沒有哪個函式庫獨佔這個模式。 |
| Brutalism | 原生 CSS、monospace、粗獷邊框。沒有函式庫。 |
| Editorial / 雜誌風 | 襯線字型、非對稱 grid、大量留白（whitespace）。沒有函式庫。 |
| Dark tech / 駭客風 | Mono + 霓虹強調色、終端機母題。沒有函式庫。 |
| Aurora / mesh 漸層 | SVG 或多層 radial gradient。沒有函式庫。 |
| Kinetic typography | 原生 CSS 動畫、scroll-driven animation、滾動劫持用 GSAP。沒有函式庫。 |
| **Apple Liquid Glass** | Apple 只為 Apple 平台撰寫文件。**沒有官方的 `liquid-glass.css`。**Web 實作是用 `backdrop-filter` + 多層邊框 + highlight 做的近似。務必清楚標示為近似。 |

---

## 3. 預設架構與慣例

除非設計判讀選了真正的設計系統（第 2.A 節），否則以下是預設：

### 3.A 技術堆疊
* **框架：**React 或 Next.js。預設使用 Server Components（RSC）。
  * **RSC 安全守則：**全域狀態只能在 Client Components 裡運作。在 Next.js 中，把 provider 包進一個 `"use client"` 元件。
  * **互動隔離：**任何用到 Motion、scroll listener 或指標物理效果的元件，都必須是頂端標了 `'use client'` 的獨立葉節點元件。Server Components 只負責渲染靜態版面。
* **樣式：** **Tailwind v4**（預設）。只有當既有專案要求時才用 Tailwind v3。
  * v4 注意：`postcss.config.js` 裡不要用 `tailwindcss` plugin，改用 `@tailwindcss/postcss` 或 Vite plugin。
* **動畫：** **Motion**（就是以前的 Framer Motion）。從 `motion/react` 匯入（`import { motion } from "motion/react"`）。`framer-motion` 套件仍可當作舊別名使用——新程式碼優先用 `motion/react`。
* **字型：**一律用 `next/font`（Next.js）或自架搭配 `@font-face` + `font-display: swap`。正式環境絕不用 `<link>` 連 Google Fonts。

### 3.B 狀態管理
* 孤立的 UI 用本地 `useState` / `useReducer`。
* 全域狀態只用來避免深層 prop-drilling——Zustand、Jotai 或 React context。
* **絕不**用 `useState` 追蹤由使用者輸入驅動的連續值（滑鼠位置、捲動進度、指標物理、磁性 hover）。改用 Motion 的 `useMotionValue` / `useTransform` / `useScroll`。`useState` 每次變動都會重新渲染整棵 React 樹，在行動裝置上直接垮掉。

### 3.C 圖示
* **允許的函式庫（優先順序）：**`@phosphor-icons/react`、`hugeicons-react`、`@radix-ui/react-icons`、`@tabler/icons-react`。
* **不建議：**`lucide-react`。只有在使用者明確要求、或專案已相依它時才可接受。
* **絕不手刻 SVG 圖示。**缺某個字符（glyph）時，就再裝一套函式庫或用基本形狀組合——不要從零畫 icon path。
* **一個專案一個圖示家族。**不要在同一棵元件樹裡混用 Phosphor 和 Lucide。
* **全域統一 `strokeWidth`**（例如 `1.5` 或 `2.0`）。

### 3.D Emoji 政策
在程式碼、標記與可見文字中預設不建議使用。以圖示庫的字符取代符號。**覆寫條件：**只有當使用者明確要求 playful / 聊天感 / 社群原生的氛圍時才允許 emoji——即使如此也要有意圖地節制使用。

### 3.E 響應式與版面機制
* 統一斷點（`sm 640`、`md 768`、`lg 1024`、`xl 1280`、`2xl 1536`）。
* 用 `max-w-[1400px] mx-auto` 或 `max-w-7xl` 收攏頁面版面。
* **視窗穩定性：**全高 Hero 區塊**絕不**用 `h-screen`，**一律**用 `min-h-[100dvh]`，避免行動裝置上版面跳動（iOS Safari 網址列）。
* **Grid 優先於 Flex 數學：**絕不用複雜的 flexbox 百分比計算（`w-[calc(33%-1rem)]`），一律用 CSS Grid（`grid grid-cols-1 md:grid-cols-3 gap-6`）。

### 3.F 相依套件驗證（強制）
匯入任何第三方函式庫之前，先檢查 `package.json`。套件不存在時，先輸出安裝指令。**絕不**假設函式庫已存在。

---

## 4. 設計工程指令（偏誤矯正）

LLM 預設會產出陳腔濫調。要主動覆寫這些預設。每條規則都有情境感知的覆寫路徑。

### 4.1 字體排印（Typography）
* **Display / 標題：**預設 `text-4xl md:text-6xl tracking-tighter leading-none`。
* **內文 / 段落：**預設 `text-base text-gray-600 leading-relaxed max-w-[65ch]`。
* **無襯線字型選擇：**
  * **不建議當預設：**`Inter`。優先選 `Geist`、`Outfit`、`Cabinet Grotesk`、`Satoshi`，或符合品牌的襯線字型。
  * **覆寫條件：**當使用者明確要求中性 / 標準 / Linear 風的感覺，或需求是公部門 / 無障礙優先的網站時，Inter 可以接受。
* **值得記住的搭配：**`Geist` + `Geist Mono`、`Satoshi` + `JetBrains Mono`、`Cabinet Grotesk` + `Inter Tight`、`GT America` + `IBM Plex Mono`。

* **襯線字型紀律（強烈不建議當預設）：**
  * 襯線字型**強烈不建議作為任何專案的預設字型。**「感覺有創意 / 高級 / editorial」不是伸手拿襯線字型的理由。Agent 心中「創意需求 = 襯線」的預設模型，是實測回合中被驗出最多次的 AI 特徵（AI tell）。
  * **只有以下其中一項明確成立時，襯線字型才可接受：**
    - 品牌需求白紙黑字點名了某個襯線字型，或
    - 美學家族確實是 editorial / 奢華 / 出版 / 手稿 / 傳承 / 復古，而且你能說清楚為什麼「這個」襯線字型適合「這個」品牌
  * 其他所有情況（創意 agency、設計工作室、現代品牌、premium consumer、作品集、生活風格）**預設用無襯線 display 字型**（Geist Display、ABC Diatype、Söhne Breit、Cabinet Grotesk Display、Migra Sans、GT Walsheim、Inter Display、PP Neue Montreal）。無襯線 display 字型不「無聊」——它們是預設，就像黑色是時尚界的預設一樣。
  * **強調規則（相關）：**想強調標題裡的某個詞（例如 kinetic 式的「and `spatial` design」手法）時，用**同一字型的 italic 或 bold**。不要為了視覺趣味把一個突兀的襯線詞塞進無襯線標題（反之亦然）。跨字族強調是外行做法；同字族的 italic / bold 強調才是正解。
  * **明確禁止當預設：**`Fraunces` 和 `Instrument_Serif`（LLM 最愛的兩款 display 襯線字型）。
  * **若襯線確有正當理由**（如上所述，很罕見），從這個池子輪替，不要連續專案重用同一款：PP Editorial New、GT Sectra Display、Cardinal Grotesque、Reckless Neue、Tiempos Headline、Recoleta、Cormorant Garamond、Playfair Display、EB Garamond、IvyPresto、Migra、Editorial Old、Saol Display、Söhne Breit Kursiv、Domaine Display、Canela、Schnyder、Tobias、NB Architekt、ITC Galliard。

* **Italic 下伸部淨空（強制）：**Display 字級使用 italic 且單字含下伸部字母（`y g j p q`）時，`leading-[1]` 或 `leading-none` 會裁到下伸部。至少用 `leading-[1.1]`，並在外層元素加 `pb-1` 或 `mb-1` 預留空間。出貨前逐一檢查 display 標題裡的每個 italic 單字。

### 4.2 色彩校準
* 最多 1 個強調色。飽和度預設 < 80%。
* **紫色守則（THE LILA RULE）：**「AI 紫 / 藍色光暈」美學不建議當預設。不要自動加紫色按鈕光暈，不要隨機的霓虹漸層。用中性基底（Zinc / Slate / Stone）搭配高對比的單一強調色（Emerald、Electric Blue、Deep Rose、Burnt Orange 等）。
* **覆寫條件：**如果品牌或需求明確要求紫色 / 紫羅蘭 / lila，就擁抱它。但要有意圖地執行：一致的調色盤、調和過的中性色、克制的漸層。不是 generic 的 AI 漸層垃圾。
* **一個專案一組調色盤。**不要在同一個專案裡在暖灰和冷灰之間搖擺。
* **色彩一致性鎖（強制）：**一旦為頁面選定強調色，就要用在**整頁**。暖灰網站不會在第 7 個 section 突然冒出藍色 CTA；玫瑰色系網站的 footer 不會出現藍綠色狀態徽章。選一個強調色，鎖住它，出貨前逐元件稽核。

* **Premium-consumer 調色盤禁令（強制，第二常見的 AI 特徵）：**
  * 面對 premium-consumer 需求（鍋具、wellness、職人、奢華、傳承工藝、DTC 居家用品等），LLM 的預設是**暖米白/奶油 + 黃銅/陶土/牛血紅/赭石 + 深咖啡/墨色深色文字**。以下 hex 家族明確禁止作為預設背景與強調色：
    - 背景：`#f5f1ea`、`#f7f5f1`、`#fbf8f1`、`#efeae0`、`#ece6db`、`#faf7f1`、`#e8dfcb`（全是「暖紙 / 奶油 / 粉筆 / 骨白」）
    - 強調：`#b08947`、`#b6553a`、`#9a2436`、`#9c6e2a`、`#bc7c3a`、`#7d5621`（全是「黃銅 / 陶土 / 牛血紅 / 赭石」）
    - 文字：`#1a1714`、`#1a1814`、`#1b1814`（全是「深咖啡 / 暖近黑」）
  * 這組調色盤禁止作為 premium-consumer 需求的預設選擇。你出貨過的每一個 premium-consumer 網站都用了這組一模一樣的調色盤，品牌因此變得隱形。
  * **預設替代方案（輪替使用，不要重複）：**
    - **Cold Luxury：**銀灰 + 鉻 + 煙灰（想想 Tesla、去掉皮革的 Apple Watch Hermes）
    - **Forest：**深綠 + 骨白 + 琥珀強調（想想 Filson、Patagonia 高階線）
    - **Black and Tan：**真正的 off-black + 暖棕褐，銳利對比，沒有米白
    - **Cobalt + Cream：**飽和藍配單一中性色，沒有黃銅
    - **Terracotta + Slate：**暖鏽紅配冷灰，沒有黃銅
    - **Olive + Brick + Paper：**低調橄欖綠加磚紅強調
    - **純單色 + 單一飽和亮點：**off-white + off-black + 一個鮮明強調色（electric blue、emerald、hot pink 等）
  * **調色盤輪替規則：**如果你上一個 premium-consumer 專案用了米白+黃銅家族，這一個就**必須**換家族。不要連續兩次出同一組暖工藝調色盤。
  * **覆寫條件：**只有當品牌需求明確點名那些顏色，或品牌識別確實是復古 / 職人 / 暖工藝，而且你能說清楚為什麼這組調色盤適合這個品牌時，米白+黃銅+深咖啡才可接受。因為「這是鍋具需求」就預設伸手去拿，是被禁止的。

### 4.3 版面多樣化
* **反置中偏誤（ANTI-CENTER BIAS）：**當 `DESIGN_VARIANCE > 4` 時，避免置中的 Hero / H1 區塊。強制改用「Split Screen（50/50）」、「內容靠左 / 資產靠右」、「非對稱留白」或 scroll-pinned 結構。
* **覆寫條件：**editorial / 宣言式 / 發表公告類需求，訊息本身就是設計時，置中 hero 沒問題。

### 4.4 材質、陰影、卡片
* 只有當高度（elevation）能傳達真實層級時才用卡片。否則用 `border-t`、`divide-y` 或負空間來分組。
* 使用陰影時，把陰影染上背景色相。淺色背景上不要有純黑 drop shadow。
* 當 `VISUAL_DENSITY > 7`：禁止 generic 卡片容器。資料指標要在素樸的版面裡呼吸。
* **形狀一致性鎖（強制）：**為頁面選定**一套**圓角尺度並貫徹到底。選項：全直角（radius 0）、全柔角（radius 12-16px）、全膠囊（互動元件用 full radius）。混合系統只有在有明文規則時才允許（例如「按鈕全膠囊、卡片 16px、輸入框 8px」），而且該規則要處處遵守。方正版面裡冒出圓按鈕、或膠囊按鈕頁面上出現方卡片，就是壞掉的設計。

### 4.5 互動 UI 狀態
LLM 預設只做「靜態的成功狀態」。一律實作完整循環：
* **Loading：**用符合最終版面形狀的 skeleton loader。避免 generic 的圓形 spinner。
* **空狀態：**精心構圖；指出如何填入內容。
* **錯誤狀態：**清楚、行內顯示（表單），或情境式（toast 只用於暫時性訊息）。
* **觸覺回饋：**在 `:active` 時用 `-translate-y-[1px]` 或 `scale-[0.98]` 模擬實體按壓。
* **按鈕對比檢查（強制，a11y）：**出貨任何按鈕前，確認按鈕文字在按鈕背景上可讀。白按鈕 + 白字、`bg-white` CTA 配 `text-white` 標籤、無邊框透明按鈕直接壓在頁面背景上 → 全部禁止。稽核每個 CTA：對比至少達 WCAG AA（內文 4.5:1，18px+ 大字 3:1）。壓在攝影背景上的 ghost button 同樣適用（加 backdrop、scrim 或描邊）。
* **CTA 按鈕換行禁令（強制）：**桌面版按鈕文字**必須**單行放得下。像「VIEW SELECTED WORK」這種標籤換成 2、3 行，按鈕就是壞的。修法二選一：縮短標籤（主要 CTA 最多 3 個詞，最好 1-2 個），或加寬按鈕（不要人為限制 CTA 的 `max-width`）。桌面版 CTA 換行是 Pre-Flight Fail。
* **禁止重複的 CTA 意圖（強制）：**同一頁有兩個相同意圖的 CTA 就是 Pre-Flight Fail。相同意圖的例子：「Get in touch」+「Contact us」+「Let's talk」+「Start a project」+「Start something」+「Reach out」全是「聯絡」意圖 → 選**一個**標籤，整頁（nav、hero、footer）都用它。「Try free」+「Get started」+「Sign up free」（全是「註冊」意圖）和「View work」+「See selected work」+「Browse projects」（全是「作品集」意圖）同理。一個意圖一個標籤。
* **表單對比檢查（強制，a11y）：**表單輸入框、placeholder 文字、focus ring、輔助文字、錯誤文字，全部都要對 section 背景通過 WCAG AA 對比。近白表單上的淺色 placeholder、白頁面 section 上的白表單、對比灰於 4.5:1 的表單標籤 → 全部禁止。出貨前稽核每個表單。

### 4.6 資料與表單模式
* 標籤在輸入框**上方**。輔助文字可選但要存在於標記中。錯誤文字在輸入框**下方**。輸入區塊標準用 `gap-2`。
* 不准用 placeholder 當標籤。永遠不准。

### 4.7 版面紀律（硬規則。違反任何一條就是出貨壞掉的作品）

* **Hero 必須塞進初始視窗。**桌面版標題最多 2 行，副文案最多 **20 個詞**且最多 3-4 行，CTA 不捲動就看得到。文案太長時：縮小字級或砍文案。如果 20 個詞的副文案講不清價值主張，那是價值主張不清楚，不是規則太緊。絕不讓 hero 溢出、逼使用者捲動才找得到 CTA。
* **Hero 字級紀律。**字級和圖片尺寸要*一起*規劃。hero 資產很大、標題又超過 6 個詞時，不要從 `text-7xl/text-8xl` 起手。合理的預設範圍：多數 hero 用 `text-4xl md:text-5xl lg:text-6xl`；只有標題 3-5 個詞時才用 `text-6xl md:text-7xl`。4 行的 hero 標題永遠是字級錯誤，不是文案長度錯誤。
* **Hero 頂部 padding 上限（強制）：**桌面版 hero 頂部 padding 最多 `pt-24`（約 6rem）。再多，hero 內容就會飄到視窗一半的位置，讀起來像版面 bug 而不是刻意留白。hero 需要更多呼吸空間時，加大字級或資產尺寸，而不是加頂部 padding。
* **Hero 堆疊紀律（最多 4 個文字元素）。**Hero 是單一時刻，不是功能清單。允許的文字元素，總共最多 4 個：
  1. Eyebrow（小型大寫標籤）或品牌列（brand strip）或都不要——選零或一個
  2. 標題（最多 2 行，見上）
  3. 副文案（最多 20 個詞，最多 4 行）
  4. CTA（1 個主要 + 最多 1 個次要）
  - **Hero 內禁止：**CTA 下方的小 tagline（「Works with GitHub, GitLab, and self-hosted Git」）、信任微條（「Used by engineering teams at...」）、定價預告（「Free for solo, $10/user for teams」）、功能 bullet 清單、社會證明頭像列。這些全部移到 hero 正下方的專屬 section。
  - 同一個 hero 裡如果既有 eyebrow 又有 CTA 下方的 tagline，砍掉 tagline。既有品牌列又有 tagline，砍掉 tagline。每個 hero 最多一個小型文字元素。
* **「Used by」/「Trusted by」logo 牆放在 hero 底下，絕不放進 hero 裡。**Hero 屬於價值主張和主要 CTA。logo 牆是緊接其下的獨立 section。不要把信任 logo 塞進和 hero 文案同一個 flex row。
* **桌面版導覽列必須渲染成單行。**在 `lg`（1024px）放不下時，就精簡標籤、砍次要項目、或改用漢堡選單。桌面版兩行的 nav 是壞掉的設計。
* **導覽列高度上限：桌面版最高 80px，預設 64-72px。**不要那種吃掉 15% 視窗的巨型「agency」nav bar。
* **Bento grid 必須有節奏，不能單邊重複。**不要疊 6 個左圖右文的 row。變化構圖：交錯全寬 feature row、非對稱磁磚尺寸、垂直斷點。
* **Bento 格數規則（強制）：**Bento grid 的格數**正好**等於你有的內容數。3 個項目 → 3 格（1+2、2+1、或非對稱三格）。5 個項目 → 5 格（2+3、3+2、hero+4 等）。如果 grid 中間或結尾出現空格，是你規劃錯了。重塑 grid，不要貼一塊空白磁磚。
* **Section 版面重複禁令。**某個版面家族（例如三欄圖卡、全寬引言、圖文分割）在頁面上用過一次後，最多只能出現**一次**。「Selected commissions」不能長得像「What we do」。有 8 個 section 的 landing page 至少要用 4 種不同的版面家族。
* **Zigzag 交錯上限（強制）。**「左圖右文」接著「左文右圖」的 zigzag 版面 = 平庸。同一種圖文分割模式最多連續 2 個 section。第 3 個連續的圖文分割是 Pre-Flight Fail。用全寬 section、垂直堆疊 section、bento grid、marquee 或不同的版面家族打破模式。
* **Eyebrow 節制（強制，實測中違反率第一名的規則）。**「Eyebrow」是 section 標題上方那行小型大寫、寬字距的標籤（例如 `FOUR COLORWAYS`、`SELECTED WORK`、`THE HARDWARE`、`Git-native task management`）。典型 CSS 特徵：`text-[11px] uppercase tracking-[0.18em]`、`font-mono text-[10.5px] uppercase tracking-[0.22em]`。每個 AI 蓋的網站都在**每個** section 標題上放 eyebrow，產生同一種範本化節奏。硬規則：
  - **每 3 個 section 最多 1 個 eyebrow。**Hero 算 1 個。所以 9 個 section 的頁面全頁最多 3 個 eyebrow。
  - 如果 section A 有 eyebrow，接下來 2 個 section 不能有。
  - **Pre-Flight 檢查是機械式的：**統計所有 section 元件裡 `uppercase tracking`（或類似的小型大寫 mono 標籤在標題上方）的出現次數。若次數 > ceil(sectionCount / 3)，輸出不合格。
  - **不放 eyebrow 該怎麼辦：**整個拿掉。標題本身就夠了。如果需要為 section 分類，它在頁面上的位置已經分類了它；不需要標籤。
* **分割式標頭禁令（SPLIT-HEADER BAN，強制）。**「左邊大標題 + 右邊小段說明文」的 section 標頭模式（左 col-span-7/8、右 col-span-4/5 飄著一小段內文）**禁止作為預設**。Section 應該只有一個聚焦的訊息。真的同時需要標題和說明段落時，垂直堆疊（標題在上、內文在下、max-width 65ch）。只有存在真正的構圖理由時才伸手拿分割式標頭（例如右欄承載視覺或互動元素，而不只是填充文字）。
* **Bento 背景多樣性（強制）。**Bento 和 feature-grid section 不能是 6 張白底白卡、裡面只有文字。任何多格 grid 至少要有 2-3 格具備真正的視覺變化：真實圖片、符合品牌的漸層（不是 AI 紫）、圖樣、或帶色背景。奶油底配奶油卡、裡面只有字體排印的 bento，就算頁面其他地方再好，讀起來也是無聊的 AI 預設。
* **每個 section 的行動裝置收合必須明寫。**每個多欄版面都要在同一個元件裡宣告 `< 768px` 的 fallback。不准有「應該沒問題，Tailwind 會處理」的假設。

### 4.8 圖片與視覺資產策略

Landing page 和作品集是**視覺產品**。只有文字加假截圖 div 的頁面就是垃圾（slop）。

**視覺資產的優先順序：**
1. **圖片生成工具優先。**只要環境裡有**任何**圖片生成工具（`generate_image`、MCP 圖片工具、IDE 內建生成、OpenAI 圖片工具等），就**必須**用它產生各 section 專屬的資產：hero 攝影、產品照、材質背景、氛圍圖。依 section 需求以正確長寬比生成。不要因為手刻 CSS 感覺比較快就跳過這步。
2. **真實網路圖片其次。**沒有生成工具時，用真實攝影來源。可接受的預設：
   * `https://picsum.photos/seed/{descriptive-seed}/{w}/{h}` 作為攝影 placeholder（seed 要描述該 section，例如 `marrow-cookware-kitchen`）
   * 需求提供時，用實際的 stock 或品牌 URL
   * 明確允許時用開放授權來源（Unsplash 直連 URL、Pexels）
3. **最後手段：告訴使用者。**兩者都不可行時，**不要**用手刻 SVG 插圖或 div 拼的「假截圖」填滿頁面。改留清楚標示的 placeholder 插槽（`<!-- TODO: hero product photo, 1600x1200 -->`），並在回覆最後說：*「這個頁面在以下位置需要真實圖片：〔位置清單〕。請生成或提供。」*

**就算是 minimalist 網站也需要真實圖片。**純文字頁面不是 minimalism，是未完成的作品。就算是 editorial 的 Linear 風網站，也至少需要 2-3 張真實圖片（hero、一張產品/生活照、一張輔助圖）。需求走克制路線就生成黑白 minimalist 攝影；不要因為轉盤值低就完全跳過圖片。

**社會證明要用真實公司 logo。**需求要求「Trusted by / Used by / Customers」logo 牆時，**不要**預設排一排純文字字標（styled 過的 `<span>Acme Co</span>`）。用真實 SVG logo：
* **來源：Simple Icons**（任何顏色都可用 `https://cdn.simpleicons.org/{slug}/ffffff`，或 `simple-icons` npm 套件）。涵蓋多數知名品牌。
* **替代：devicon** 用於技術棧 logo（`@svgr/cli` 或 CDN）。
* **品牌名是虛構的？那就連 SVG 標誌一起虛構。**生成一個簡單的 monogram（圓圈裡一個字母、雙字母連字、抽象字符），以行內 `<svg>` 渲染並配合頁面風格。虛構品牌名配純文字字標看起來很 generic。
* **一律**確保 logo 在亮暗兩種模式都能正常渲染（深底白、淺底黑、或單色主題變數）。
* **LOGO-ONLY 規則（強制）：**logo 牆 = 只有 logo，別無其他。**不要**在每個 logo 下方印產業 / 類別標籤（不要 `Vercel` 下面寫 `hosting`、`Stripe` 下面寫 `payments`、`Cloudflare` 下面寫 `infra`）。logo 本身就是可信度，標籤加不了使用者不知道的東西。可選：品牌名當 alt 文字給螢幕閱讀器、可選連到品牌網站。僅此而已。

**手刻插圖：**
* 來自函式庫的 SVG 圖示：可以（見第 3.C 節）。
* 手刻裝飾性 SVG（客製插圖、logo、標誌）：**強烈不建議**，絕不當預設。只有以下情況可接受：
  - 需求明確要求（「幫我畫一個 SVG logo」）
  - 是單一、簡單的幾何標誌（一個方形、一個圓形、display 字型的字標）
  - 你對輸出品質有信心

**Div 拼的假截圖是禁止的。**用 `<div>` 矩形拼出來的「手工產品預覽」——假任務清單、假 dashboard、假終端機視窗——就是 AI 特徵。需要展示產品時：
* 有真實截圖 URL 就用
* 用圖片工具生成一張
* 用真實元件預覽（頁面裡放一個真的迷你版 UI）
* 或乾脆跳過預覽，改用 editorial 攝影

**Hero 需要真實視覺。**文字 + 漸層色塊不是 hero——是 placeholder。

### 4.9 內容密度

Landing page 靠的是**第一印象**，不是全文精讀。狠心地砍。

* **每個 section 的預設內容形狀：**短標題（≤ 8 個詞）+ 短副段落（≤ 25 個詞）+ 一個視覺資產**或**一個 CTA。再多的東西都必須以該 section 的任務來合理化。
* **不准有資料傾倒式 section。**行銷頁上 20 列的出版品表格、30 列的獎項清單、巨型定價矩陣 = 用錯版面。改用：
  - 前 3-5 個亮點 +「View full list」連結
  - Marquee / carousel 呈現廣度
  - 資料本身就是產品的話，另開一頁
* **長清單需要的是不同的 UI 元件，不是更長的清單。**預設的 `<ul>` bullet / `divide-y` row 是偷懶的選擇。超過 5 個項目時，改用以下之一：
  - 兩欄分割搭配分組項目
  - 每項配圖 + 標籤的卡片 grid
  - 項目可分類就用 tabs / accordion
  - 水平 scroll-snap 膠囊
  - 廣度型清單（見證、logo、能力）用 carousel
  - 「很多但不需要個別注意的東西」用 marquee
  10 列規格表、每列下面一條 hairline，是**最糟的**預設。要嘛把列分成 2-3 塊、用稀疏的分隔線，要嘛改成一卡一規格的版面。
* **規格表特別注意（Marrow 鍋具模式）。**每列都加 `border-b` 的長產品規格表，是 AI 面對鍋具 / 硬體 / 服飾 / 職人商品需求的預設。禁止。具體替代方案：
  - **兩欄卡片 grid：**每個規格一張卡，含規格名、數值（大型 display 數字）、一行「為什麼重要」的內文。桌面兩欄、行動一欄。
  - **Scroll-snap 水平膠囊：**每個規格一顆膠囊，使用者可滑動瀏覽。
  - **分組成塊：**把 10 個規格分成 3 個邏輯群（例如「Materials」、「Cooking」、「Warranty」），每群配**一條**柔和分隔線和群標題。
  - **主打 vs 其餘：**3-4 個主打規格做成大型 display 磁磚，其餘收在「View full specifications」的 disclosure 裡。

* **文案自我稽核（COPY SELF-AUDIT，出貨前強制）：**宣告任何任務完成之前，重讀頁面上每一條可見字串（標題、副標、eyebrow、按鈕標籤、內文、圖說、alt 文字、footer 文字、錯誤訊息）。標記任何符合以下情況的字串：
  - **文法壞掉**（「free on its past」、「two plans but one is honest」、脫離語境的「to put it on the table」）
  - **指涉不明**（沒有前文的「we plan to stay that way」）
  - **聽起來像 AI 幻覺**（可愛但錯誤的雙關、不成立的強行比喻、「elegant nothing」式的空話）
  - **讀起來像 LLM 在裝深沉**（被動攻擊式的謙遜、假工匠標籤、故作詩意的 micro-meta）
  改寫每條被標記的字串。不確定某字串是否說得通時，換成平實的功能性句子。AI 生成的賣萌文案比無聊文案更糟。
* **假精確數字要被標記。**像 `92%`、`4.1×`、`48k`、`5.8 mm`、`13.4 lb` 這類數字必須：
  - 來自真實資料（需求、品牌準則、公開指標）——可以
  - 明確標示為 mock（`<!-- mock -->`、「example」、「sample data」）——可以
  - AI 自己發明的規格美學——禁止。不要偽造品牌沒有宣稱的工程精確度。
* **一頁一種文案語域（register）。**不要在同一個構圖裡混用技術 mono（「47 tasks · 0.6 ctx-switches/day」）、editorial 散文和行銷重拳，除非品牌聲音明確要求。

### 4.10 引言與見證（Testimonials）

* 引言本文**最多 3 行**。絕不 6 行。原始引言更長 → 剪短。landing page 的引言是片段，不是完整評論。
* 字級非常小時（例如 footer 式見證），行數上限可以稍微放寬。精神是：「一眼看完」。
* 引言文字裡**不用 em-dash** 當設計花招（長停頓、kinetic em-dash、em-dash 當 bullet）。見第 9.G 節——em-dash 全面禁止。
* 署名：姓名 + 職稱 +（可選）公司。絕不只有名字（「- Sarah」）。
* 引號：用真正的印刷引號（" "）或乾脆不用。不用直立的 ASCII 引號（"）。

### 4.11 頁面主題鎖（亮 / 暗模式一致性）

一頁只有**一個**主題。Section 不准反轉。

* 頁面是暗模式，**所有** section 都是暗模式。不准在暗色 section 之間夾一個亮色暖紙 section（反之亦然）。使用者不該在捲動途中覺得走進了另一個網站。
* 例外：需求明確要求「Color Block Story」或「捲動切換主題」的手法，**且**那是刻意的構圖（一次完整的主題切換配強力轉場，不是隨機交錯）時，每頁允許一次。
* 預設行為：在頁面層級選定亮、暗或自動（`prefers-color-scheme`）然後鎖住。同一主題家族內的 section 層級背景色調變化沒問題（`bg-zinc-950` 旁邊放 `bg-zinc-900`）；在 `bg-zinc-950` 的頁面中間翻成 `bg-amber-50` 就是壞掉。
* 使用內建主題機制的設計系統（Radix Themes、shadcn/ui 的 `<Theme>`）時，在 `layout.tsx` 或頁面根節點設定主題**一次**。不准讓個別 section 覆寫。

---

## 5. 情境感知的主動性

這些是工具，不是預設。設計判讀需要時才用。**沒有任何一項會自動觸發。**

* **Liquid Glass / Glassmorphism：**適合 premium consumer、Apple 系、奢華品牌或媒體疊層氛圍。不適合 dashboard、公部門或「無聊 B2B」。使用時要超越單純的 `backdrop-blur`：加 1px 內邊框（`border-white/10`）和細微內陰影（`shadow-[inset_0_1px_0_rgba(255,255,255,0.1)]`）做出實體邊緣折射感。在 `prefers-reduced-transparency` 下提供實色 fallback。
* **磁性微物理（Magnetic Micro-physics）：**當 `MOTION_INTENSITY > 5` 且需求讀起來是 premium / playful / agency 時使用。**只能**用 Motion 的 `useMotionValue` / `useTransform` 在 React 渲染週期之外實作。絕不用 `useState`。見第 3.B 節。
* **常駐微互動**（Pulse、Typewriter、Float、Shimmer、Carousel）：當 `MOTION_INTENSITY > 5` 且該 section 確實因動態受益（狀態指示、即時動態、AI 感）時使用。**不是每張卡片都需要無限循環。**資訊型 section 就讓它靜止。套用 Spring Physics（`type: "spring", stiffness: 100, damping: 20`）——不用線性 easing。
* **「宣稱有動態，就要看得到動態。」**若 `MOTION_INTENSITY > 4`，頁面必須真的會動：至少要有 hero 進場轉場、關鍵 section 的 scroll-reveal、CTA 的 hover 物理。宣稱 `MOTION_INTENSITY: 7` 卻是靜態頁面，就是壞掉。反過來說，在可用範圍內做不出能動的動態時，就把轉盤降到 3、出一個乾淨的靜態頁面。絕不半吊子地做出會壞的動態（被切斷的 ScrollTrigger、跳動的進場、缺 cleanup）。
* **動態必須有動機（強制）。**加任何動畫之前先問：「這個動畫在傳達什麼？」有效答案：層級（把注意力引到對的地方）、敘事（依敘事順序揭示內容）、回饋（回應使用者操作）、狀態轉換（顯示某物改變了）。無效答案：「看起來很酷」。因為有 GSAP 就到處用 GSAP 是外行。每個 ScrollTrigger、每個 marquee、每個 pinned section 都需要理由。一句話講不出理由，就砍掉那個動畫。
* **Marquee 每頁最多一個（強制）。**水平捲動文字 marquee（「logo 無限捲動」、「宣言橫著跑」、「kinetic 文字帶」）每頁最多用**一次**。同一頁出現兩個以上 marquee，讀起來就是偷懶填充。挑 marquee 真正服務內容的那一個 section；其他的用不同版面。
* **GSAP Sticky-Stack 模式（使用捲動堆疊時）。**「捲動時卡片堆疊」必須是真正的 sticky-stack，不是循序 reveal 清單。標準程式碼骨架見下方第 5.A 節。常見失敗：trigger 在捲動到一半時觸發，而不是釘在視窗頂端。修法：`start: "top top"`，不是 `start: "top center"` 或 `"top 80%"`。
* **GSAP 水平平移模式（使用水平滾動劫持時）。**標準骨架見下方第 5.B 節。常見失敗：section 還沒釘住動畫就開始，使用者看到半張 slide。同樣的修法：`start: "top top"`，釘住 wrapper，scrub 內層 track。

### 5.A Sticky-Stack——標準骨架

```tsx
"use client";
import { useRef, useEffect } from "react";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { useReducedMotion } from "motion/react";

gsap.registerPlugin(ScrollTrigger);

export function StickyStack({ cards }: { cards: React.ReactNode[] }) {
  const ref = useRef<HTMLDivElement>(null);
  const reduce = useReducedMotion();

  useEffect(() => {
    if (reduce || !ref.current) return;
    const ctx = gsap.context(() => {
      const cardEls = gsap.utils.toArray<HTMLElement>(".stack-card");
      cardEls.forEach((card, i) => {
        if (i === cardEls.length - 1) return;
        ScrollTrigger.create({
          trigger: card,
          start: "top top",                              // 釘在視窗頂端
          endTrigger: cardEls[cardEls.length - 1],
          end: "top top",
          pin: true,
          pinSpacing: false,
        });
        gsap.to(card, {
          scale: 0.92,
          opacity: 0.55,
          ease: "none",
          scrollTrigger: {
            trigger: cardEls[i + 1],
            start: "top bottom",
            end: "top top",
            scrub: true,
          },
        });
      });
    }, ref);
    return () => ctx.revert();
  }, [reduce]);

  return (
    <div ref={ref} className="relative">
      {cards.map((card, i) => (
        <div
          key={i}
          className="stack-card sticky top-0 min-h-[100dvh] flex items-center justify-center"
        >
          {card}
        </div>
      ))}
    </div>
  );
}
```

關鍵點：`start: "top top"`、`pin: true`、除最後一張外每張卡都被釘住，縮放/透明度的變形是由「下一張卡」的 scroll trigger 驅動（所以前一張卡在下一張抵達時縮小）。

### 5.B 水平平移（Horizontal-Pan）——標準骨架

```tsx
"use client";
import { useRef, useEffect } from "react";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { useReducedMotion } from "motion/react";

gsap.registerPlugin(ScrollTrigger);

export function HorizontalPan({ children }: { children: React.ReactNode }) {
  const wrap = useRef<HTMLDivElement>(null);
  const track = useRef<HTMLDivElement>(null);
  const reduce = useReducedMotion();

  useEffect(() => {
    if (reduce || !wrap.current || !track.current) return;
    const ctx = gsap.context(() => {
      const distance = track.current!.scrollWidth - window.innerWidth;
      gsap.to(track.current, {
        x: -distance,
        ease: "none",
        scrollTrigger: {
          trigger: wrap.current,
          start: "top top",                              // section 頂端碰到視窗頂端時開始釘住
          end: () => `+=${distance}`,                    // 捲動距離 = track 寬度減視窗寬度
          pin: true,
          scrub: 1,
          invalidateOnRefresh: true,
        },
      });
    }, wrap);
    return () => ctx.revert();
  }, [reduce]);

  return (
    <section ref={wrap} className="relative overflow-hidden">
      <div ref={track} className="flex h-[100dvh] items-center">
        {children}
      </div>
    </section>
  );
}
```

關鍵點：`start: "top top"`、`pin: true`、`end: "+=${distance}"`（捲動長度 = 所需的水平位移）、`scrub: 1`。wrapper 被釘住，內層 track 隨使用者垂直捲動而水平滑動。

### 5.C Scroll-Reveal Stagger——標準骨架（較輕量的替代）

單純的「項目進入視窗就出現」（不需釘住）時，優先用 Motion 的 `whileInView` 而不是 GSAP——更輕量、不需要 ScrollTrigger：

```tsx
"use client";
import { motion, useReducedMotion } from "motion/react";

export function RevealStagger({ items }: { items: string[] }) {
  const reduce = useReducedMotion();
  return (
    <ul className="grid gap-6">
      {items.map((item, i) => (
        <motion.li
          key={item}
          initial={reduce ? false : { opacity: 0, y: 24 }}
          whileInView={{ opacity: 1, y: 0 }}
          viewport={{ once: true, amount: 0.3 }}
          transition={{
            duration: 0.6,
            delay: i * 0.06,
            ease: [0.16, 1, 0.3, 1],
          }}
        >
          {item}
        </motion.li>
      ))}
    </ul>
  );
}
```

適用於：功能清單、見證 grid、logo 牆，任何只需要「捲動時進場」的東西。GSAP 留給真正的 pin/scrub 工作。

### 5.D 禁用的動畫模式

* **`window.addEventListener("scroll", ...)`** 禁止。它在每個捲動影格都執行、容易卡頓（jank）、無法批次處理。改用 Motion 的 `useScroll()`、GSAP 的 `ScrollTrigger`、IntersectionObserver，或 CSS `scroll-driven animations`（`animation-timeline: view()`）。
* **在 React state 裡用 `window.scrollY` 自算捲動進度。**同樣的理由。每個影格都重新渲染。
* **會碰 React state 的 `requestAnimationFrame` 迴圈。**改用 motion values（`useMotionValue` + `useTransform`）。
* **版面轉場（Layout Transitions）：**可見的狀態變化（清單重新排序、modal 展開、路由間共享元素）用 Motion 的 `layout` 和 `layoutId` props。不要「為了保險」把靜態內容包進 `layout` props——它會付出量測成本。
* **交錯編排（Staggered Orchestration）：**順序有意義的 reveal 時刻，用 `staggerChildren`（Motion）或 CSS 級聯（`animation-delay: calc(var(--index) * 100ms)`）。使用 `staggerChildren` 時，父層（`variants`）和子層**必須**在同一棵 Client Component 樹裡。

---

## 6. 效能與無障礙護欄

### 6.A 硬體加速
* 只對 `transform` 和 `opacity` 做動畫。絕不對 `top`、`left`、`width`、`height` 做動畫。
* `will-change: transform` 節制使用——只放在真的會動的元素上。

### 6.B 減少動態（Reduced Motion，強制）
* **任何 `MOTION_INTENSITY > 3` 的動態都必須遵守 `prefers-reduced-motion`。**沒得商量。
* Motion 裡：用 `useReducedMotion()` 包住並降級為靜態。
* CSS 裡：把動畫放進 `@media (prefers-reduced-motion: no-preference)`，或在 `@media (prefers-reduced-motion: reduce)` 下提供停用的覆寫區塊。
* 無限循環、parallax、滾動劫持、磁性物理，在 reduced motion 下**必須**塌縮為靜態 / 即時完成。

### 6.C 暗模式（任何消費者導向頁面都強制）
* **從一開始就為兩種模式設計。**沒有使用者明確指示，絕不出貨只有亮或只有暗的版本。
* 用 Tailwind `dark:` variant 或 CSS 變數做 tokens。一個專案選一種策略。
* **這裡不指定具體的暗模式顏色。**由需求決定。兩種模式都要維持視覺層級、品牌識別和 WCAG AA 對比（內文 AAA）。
* 尊重 `prefers-color-scheme: dark`。除非品牌堅持單一模式，預設跟隨系統偏好。

### 6.D Core Web Vitals 目標
* **LCP** < 2.5s。Hero 圖片必須用 `next/image priority` 或 preload。
* **INP** < 200ms。重活移出主執行緒。
* **CLS** < 0.1。為圖片、字型、embed 預留空間。
* 宣告頁面完成前先跑 Lighthouse。

### 6.E DOM 成本
* 顆粒 / 噪點濾鏡**只能**套在固定的、`pointer-events-none` 的偽元素上（例如 `fixed inset-0 z-[60] pointer-events-none`）。**絕不**套在會捲動的容器上——連續的 GPU 重繪會摧毀行動裝置的 FPS。
* 注意 bundle 大小。Motion 不算小，Three.js 很大。不在首屏（above the fold）的東西一律 lazy-load。

### 6.F Z-Index 節制
絕不亂灑 `z-50` 或 `z-10`。z-index 只用在系統性的圖層情境（sticky navbar、modal、overlay、顆粒層）。把 z-index 尺度記錄在專案常數檔裡。

---

## 7. 轉盤定義（技術參考）

### DESIGN_VARIANCE（等級 1-10）
* **1-3（可預期）：**對稱的 CSS Grid（12 欄、等 fr 單位）、相等的 padding、置中對齊。
* **4-7（偏移）：**`margin-top: -2rem` 的疊壓、變化的圖片長寬比（4:3 旁邊放 16:9）、置中資料上方配靠左標題。
* **8-10（非對稱）：**Masonry 版面、分數單位的 CSS Grid（`grid-template-columns: 2fr 1fr 1fr`）、大面積空白區（`padding-left: 20vw`）。
* **行動裝置覆寫：**等級 4-10 時，`md:` 以上的非對稱版面在 `< 768px` 視窗**必須**收合為嚴格單欄（`w-full`、`px-4`、`py-8`）。

### MOTION_INTENSITY（等級 1-10）
* **1-3（靜態）：**沒有自動動畫。只有 CSS `:hover` 和 `:active` 狀態。`prefers-reduced-motion` 本來就是預設模式。
* **4-7（流暢 CSS）：**`transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1)`。進場用 `animation-delay` 級聯。聚焦在 `transform` 和 `opacity`。
* **8-10（進階編排）：**複雜的捲動觸發 reveal、parallax、scroll-driven animation（CSS `animation-timeline` 或 GSAP ScrollTrigger）。用 Motion hooks。**絕不用 `window.addEventListener('scroll')`**——這是硬性禁令，不是「盡量不要」。允許的替代方案見第 5.D 節。

### VISUAL_DENSITY（等級 1-10）
* **1-3（美術館）：**大量留白。巨大的 section 間距（`py-32` 到 `py-48`）。昂貴、乾淨。
* **4-7（日常應用）：**標準 web app 間距（`py-16` 到 `py-24`）。
* **8-10（駕駛艙）：**緊湊 padding。不用卡片盒；1px 線條分隔資料。強制：所有數字用 `font-mono`。

---

## 8. 暗模式協定

預設雙模式。除非需求是模擬印刷的 editorial，否則絕不假設只有亮模式。

### 8.A Token 策略（選一個，貫徹到底）
* **Tailwind `dark:` variant**（utility-first 專案的預設）：每個顏色 utility 都配上暗模式變體（`bg-white dark:bg-zinc-950`、`text-gray-900 dark:text-gray-100`）。
* **CSS 變數**（用於 shadcn/ui、Radix Themes 或有主題機制的元件庫）：定義語意化 tokens（`--surface`、`--surface-elevated`、`--text-primary`、`--accent`），在 `[data-theme="dark"]` 或 `@media (prefers-color-scheme: dark)` 下替換值。

### 8.B 這裡不指定具體顏色
由需求和品牌決定。本 skill 只強制：
* **對比**——內文至少 WCAG AA，hero 文案以 AAA 為目標。
* **層級對等**——亮模式成立的視覺層級，在暗模式也要成立。CTA 在亮模式跳出來，在暗模式也要跳出來。
* **品牌忠實度**——主品牌色保持可辨識。不要把品牌去飽和到淹沒在暗模式裡。
* **不用純 `#000000`、不用純 `#ffffff`**——用 off-black（zinc-950、近黑暖灰）和 off-white。純值會扼殺深度。

### 8.C 預設模式
尊重 `prefers-color-scheme`，除非品牌堅持。若任一模式會喪失關鍵品牌表現，加一個手動切換。

### 8.D 完成前在兩種模式下測試
開發期間就用兩種模式開啟頁面。不要出貨一個你只在一種模式下看過的頁面。

---

## 9. AI 特徵（AI Tells：禁用模式）

除非需求明確要求，否則避開這些簽名式模式。

### 9.A 視覺與 CSS
* **預設不用霓虹 / 外光暈。**改用內邊框或細微的染色陰影。
* **不用純黑（`#000000`）。**用 off-black、zinc-950 或炭灰。
* **不用過飽和的強調色。**去飽和讓它融入中性色。
* **大標題不用過量的漸層文字。**
* **不用自訂滑鼠游標。**過時、對無障礙不友善、對效能不友善。

### 9.B 字體排印
* **避免把 Inter 當預設。**見第 4.1 節。存在覆寫路徑。
* **不要只會用超大 H1 吼人。**用字重 + 顏色控制層級，不是靠原始尺寸。
* **襯線限制：**襯線給 editorial / 奢華 / 出版。不給 dashboard。

### 9.C 版面與間距
* **數學上完美**的 padding 和 margin。不要有帶著尷尬縫隙的漂浮元素。
* **不用三欄等寬 feature card。**generic 的「三張一樣的卡片橫排」feature row 是禁止的。改用兩欄 zigzag、非對稱 grid、scroll-pinned 或水平捲動的替代方案。

### 9.D 內容與資料（「Jane Doe」效應）
* **不用 generic 名字。**「John Doe」、「Sarah Chan」、「Jack Su」→ 用有創意、真實感、符合地區的名字。
* **不用 generic 頭像。**不用 SVG「蛋形」或 Lucide user 圖示 → 用可信的照片 placeholder 或特定的造型處理。
* **不用假完美的數字。**避免 `99.99%`、`50%`、`1234567`。用有機、混亂的資料（`47.2%`、`+1 (312) 847-1928`）。
* **不用新創垃圾品牌名。**「Acme」、「Nexus」、「SmartFlow」、「Cloudly」→ 發明有情境、聽起來真實的高質感名字。
* **不用填充動詞。**「Elevate」、「Seamless」、「Unleash」、「Next-Gen」、「Revolutionize」→ 只用具體動詞。

### 9.E 外部資源與元件
* **不手刻 SVG 圖示。**用 Phosphor / HugeIcons / Radix / Tabler。Lucide 只在明確要求時使用。
* **手刻裝飾性 SVG 強烈不建議**當預設（見第 4.8 節）。
* **不用 div 拼的假截圖。**絕不用 `<div>` 矩形蓋一個假產品 UI 來模擬截圖。用真實圖片、生成圖片，或跳過預覽。
* **不用失效的 Unsplash 連結。**用 `https://picsum.photos/seed/{descriptive-string}/{w}/{h}`、生成的照片 placeholder 或實際資產。
* **shadcn/ui 客製：**允許，但**絕不**以預設狀態出貨。依專案美學客製圓角、顏色、陰影、字體排印。
* **產品級整潔度：**程式碼視覺上乾淨、令人印象深刻、一絲不苟地打磨。

### 9.F 實測驗出的特徵（直接禁止）

這些模式來自真實的 LLM 生成 landing page 測試。它們是模型想「看起來有設計感」時的預設簽名。除非需求明確要求某一項，否則一律視為硬性禁令。

**Hero 與頁面頂部**
* **Hero 裡不放版本標籤。**`V0.6`、`v2.0`、`BETA`、`INVITE-ONLY PREVIEW`、`EARLY ACCESS`、`ALPHA`——禁止作為預設 eyebrow。只有需求明確關於產品發表 / 預覽狀態時才可接受。
* **不用「Brand · No. 01」式的次級 eyebrow。**「Marrow · No. 01 · The 6-quart」這類 micro-meta 行。跳過。

**Section 編號與微標籤**
* **不用 section 編號 eyebrow。**`00 / INDEX`、`001 · Capabilities`、`002 · Featured commission`、`06 · how it works`、`05 · The honest table`——禁止。Eyebrow 應該用白話講主題，不是編號。
* **圖片或 bento 磁磚上不放 `01 / 4` 式分頁標示。**使用者會數數，不需要標籤。
* **不用 `Scroll · 001 Capabilities` 式捲動提示。**簡單箭頭或「Scroll」就夠；不加 section 編號前綴。
* **不把「Index of Work, 2018 - 2026」式範圍標籤**當 eyebrow。直接說這個 section 是什麼。

**分隔符與圓點**
* **間隔號（`·`）採配給制。**metadata 條每行最多 1 個。**不要**把它當萬用分隔符（「foo · bar · baz · qux · quux」）。需要分隔符家族時，優先用換行、hairline 或分欄。
* **不在每個清單/導覽/徽章上放裝飾性彩色狀態圓點。**「ONE Q4 SLOT OPEN」前的彩點、每個 nav 連結前、每列任務前的彩點——預設禁止。只有圓點傳達真實語意狀態（伺服器狀態、可用性旗標）且節制使用時才可接受。

**Em-dash 與排印花招**
* **Em-dash（`—`）不作為設計元素，也不出現在任何地方。**完整、不可協商的禁令見下方第 9.G 節。em-dash 字元在標題、eyebrow、膠囊、內文、引言、署名、圖說、按鈕文字和 alt 文字中一律禁止。用一般連字號（`-`）。
* **不把 `<br>` 斷行 + 斜體的標題**當預設「設計手法」。「for thirty\<br\>*years.*」這類切法。標題首先要讀起來自然，只有需求要求時才耍聰明。
* **不用垂直旋轉文字**（「INDEX OF WORK, 2018 - 2026」轉 90°）。agency 作品集的陳腔濫調。只有需求明確是 agency / Awwwards / 實驗性、且它服務真正的構圖目的時才用。
* **不用十字準星 / hairline 格線當裝飾。**只為了讓頁面「感覺有設計」而畫的垂直水平線——禁止。只有在組織真實內容時才用。

**假產品預覽**
* **Hero 裡不放 div 拼的假產品 UI**（styled div 蓋的假任務清單、假終端機、假 dashboard）。這是 LLM 設計特徵第一名。用真實截圖、生成圖片、真實元件預覽，或乾脆不放。
* **假截圖裡不放假版本 footer**（「v0.6.2-rc.1」、「last sync 4s ago · main」）。毫無貢獻，滿滿 AI 味。

**行銷文案特徵**
* **不用「Quietly in use at」/「Quietly trusted by」**式社會證明標頭。用自然語言：「Trusted by」、「Used at」、「Customers include」，或者 logo 自己會說話時乾脆不放標頭。
* **不用「From the field」/「Field notes」/「Currently on the bench」/「On our desks」/「Loose plates」式詩意標籤**放在引言、部落格或側欄 section 上。讀起來是表演型工匠味。用平實的功能標籤（「Testimonials」、「Latest writing」、「Now working on」）或不放標籤。
* **不用「We respect the French ones」式**假謙虛的同業致意內文。又賣萌又 AI。
* **不用天氣 / 地區條**（「LIS 14:23 · 18°C」）在 header/footer，除非需求明確關於某個地點 / 跨時區分布的工作室。
* **Eyebrow 下不放 micro-meta 句子。**像 *「Each of these is a feature we ship today, not a roadmap promise. The list will stay short on purpose.」* 這種掛在 section 標題下的句子是雜訊。Eyebrow + 標題 + 內文就夠了。
* **不用 generic 步驟標籤。**「Stage 1 / Stage 2 / Stage 3」、「Step 1 / Step 2 / Step 3」、「Phase 01 / Phase 02 / Phase 03」、「Pass One / Pass Two / Pass Three」。禁止。實際的步驟內容就是標籤。必須呈現進程時，直接用動詞-名詞（「Install」、「Configure」、「Ship」），不是「Stage 1: Install」。

**膠囊、標籤與版本戳**
* **不在圖片上疊膠囊/標籤/tag。**不要在照片上疊 `<span>` 加 `Brand · 02`、`PLATE · BRAND`、`Field notes - journal` 這類 tag。要嘛讓圖片自己說話，要嘛在圖片正下方（圖片之外）加圖說。
* **不把攝影署名圖說當裝飾。**stock/picsum 圖片下的 `Field study no. 12 · Ines Caetano`、`Plate 03 · House archive`、`Frame XII · 35mm` 這類字串很做作。只有真的有攝影師為真實照片掛名（且經同意）時才允許攝影署名。否則：跳過圖說，或用一行功能性圖說（「The 6-quart, in Sage.」）。
* **行銷頁不放版本 footer。**footer 裡的 `v1.4.2`、`Build 0048`、`last sync 4s ago · main` 是 CLI / 開發工具的配件，不是 landing page 內容。行銷 / landing / 作品集頁面禁止。
* **不用「Reservation 412 of 800」式即時庫存計數器**當裝飾。只有需求明確是有真實資料的限量 waitlist 時才行。

**裝飾文字條**
* **Hero 底部不放裝飾文字條。**`BRAND. MOTION. SPATIAL.`、`TYPE / FORM / MOTION`、`DESIGN · BUILD · SHIP`、`ESTD. 2018 · LISBON · BRAND. MOTION. SPATIAL.` 這類橫貫 hero 底部的小型 mono 大寫條，是 agency 作品集陳腔濫調。預設禁止。只有當這條承載真實可導覽的連結（sticky 底部導覽）或真實狀態資訊（cookie 橫幅、docs 網站的 build 資訊）時才可接受。
* **Section 標題不放右上角漂浮小字。**模式：section 有巨大的靠左標題；同一個 section 標頭的右上角飄著一小段說明文字，和其他東西都對不齊。那個漂浮物就是特徵。要嘛把小字直接放標題下方，要嘛做乾淨的兩欄標頭（左：標題，右：對齊的內文），但不要一小段角落文字。

**清單、分隔線與計分**
* **長清單 / 規格表不要每列都 `border-t` + `border-b`。**選一種（列與列之間下邊框，或群組上方上邊框）並稀疏使用。10 列規格表每列下面一條 hairline 是最偷懶的版面——替代 UI 元件見第 4.9 節。
* **不用有填色背景軌道的計分/進度條**當比較視覺。需要呈現「X / Y」比較時，優先用數字 + 小圖示，或**沒有**背景軌道的迷你行內長條。大塊 `bg-zinc-200` 軌道上蓋一段填色，是 landing page 上的 dashboard UI 雜訊。

**地區、時間、捲動提示**
* **地區 / 城市名 / 時間 / 天氣條對 99% 的需求都是禁止的。**hero 裡的「Lisbon, working with founders」、footer 裡的「1200-690 Lisbon, Portugal」、nav 裡的「Lisbon 14:23 · 18°C」。這些是 agency 作品集裝飾特徵。只有以下情況允許：需求明確描述一個跨時區分布、時區攸關業務的工作室，或旅遊導向的品牌，或真實的實體場館。footer 提一次聯絡地址沒問題；氛圍式地區條不行。
* **捲動提示禁止。**`Scroll`、`↓ scroll`、`Scroll to explore`、`Scroll to walk through it`、動畫滑鼠滾輪圖示。使用者還沒捲動時，他正在看 hero。他知道什麼是捲動。視窗底部不需要標籤。
* **預設零顆裝飾性狀態圓點。**nav 項目前、清單列前、徽章前、狀態標籤前的彩色圓點都是特徵。只有傳達真實語意狀態（真實伺服器狀態的 live 指示、真實可用性旗標）時才可接受，且每個頁面 section 最多一顆。

### 9.G EM-DASH 禁令（違反率最高的單一特徵）

**Em-dash（`—`）全面禁止。**它是 LLM 的簽名式文體拐杖，也是實測中視覺特徵第一名。沒有「有限度使用」的餘地，沒有「自然語言頻率」的餘地，沒有「內文裡可以」的餘地。都沒有。

* **標題裡禁止。**用句號或逗號。
* **Eyebrow / 標籤 / 膠囊 / 按鈕文字 / 圖說 / nav 項目裡禁止。**改用換行、分欄或 hairline。
* **內文裡禁止。**重組句子：拆成兩句加句號、或逗號、或括號、或冒號。
* **引言署名裡禁止。**用帶空格的一般連字號（` - `）或換行 + 較細字重的名字。
* **當分隔符用的 en-dash（`–`）也禁止。**日期範圍（`2018-2026`）用連字號。數字範圍（`€40-80k`）用連字號。

頁面上唯一允許的 dash 字元是：
* 一般連字號 `-`（複合詞、範圍、標記中的分隔線）
* 數學裡的負號（`-5°C`）

只要輸出中有任何一個使用者看得到的 `—` 或 `–`，該輸出就沒通過 Pre-Flight 檢查，必須重寫。

這條規則不可協商。過去用「節制使用」的措辭時，agent 一直無視 em-dash 限制。這裡的措辭是二元的：零個 em-dash。

---

## 10. 參考詞彙（Agent 該認識的模式名稱）

這是詞彙表，不是函式庫。Agent 應該**認識**這些模式名稱，才能用它們溝通、帶著它們思考設計、並在設計判讀需要時伸手取用。**實作與程式碼草圖放在 Block Library（第 12 節），會逐步補上。**

### Hero 範式
* **Asymmetric Split Hero**——文字一側、資產一側，大量留白。
* **Editorial Manifesto Hero**——大字級、無資產，近乎海報。
* **Video / Media Mask Hero**——文字作為遮罩鏤空在影片背景上。
* **Kinetic-Type Hero**——動態字體排印作為主要視覺。
* **Curtain-Reveal Hero**——捲動時 hero 像布幕一樣分開。
* **Scroll-Pinned Hero**——hero 釘住不動，內容在後方捲動。

### 導覽與選單
* **Mac OS Dock Magnification**——邊緣導覽，圖示在 hover 時流暢縮放。
* **Magnetic Button**——被游標吸過去。
* **Gooey Menu**——子項目像黏稠液體般分離。
* **Dynamic Island**——用於狀態 / 通知的變形膠囊。
* **Contextual Radial Menu**——在點擊處展開的環形選單。
* **Floating Speed Dial**——FAB 彈出成弧形的次要動作。
* **Mega Menu Reveal**——全螢幕下拉，內容交錯淡入。

### 版面與 Grid
* **Bento Grid**——非對稱磁磚分組（Apple 控制中心）。
* **Masonry Layout**——交錯 grid，無固定列高。
* **Chroma Grid**——邊框 / 磁磚帶著細微流動的漸層。
* **Split-Screen Scroll**——兩半往相反方向滑動。
* **Sticky-Stack Sections**——捲動時釘住並堆疊的 section。

### 卡片與容器
* **Parallax Tilt Card**——追蹤滑鼠座標的 3D 傾斜。
* **Spotlight Border Card**——邊框在游標下方發亮。
* **Glassmorphism Panel**——帶內部折射的毛玻璃。
* **Holographic Foil Card**——hover 時虹彩流轉。
* **Tinder Swipe Stack**——實體卡疊，滑走即消。
* **Morphing Modal**——按鈕自己展開成對話框。

### 捲動動畫
* **Sticky Scroll Stack**——卡片黏住並實際堆疊。
* **Horizontal Scroll Hijack**——垂直捲動 → 水平平移。
* **Locomotive / Sequence Scroll**——影片 / 3D 序列綁定捲軸。
* **Zoom Parallax**——中央背景圖隨捲動放大。
* **Scroll Progress Path**——SVG 線條沿捲動繪製。
* **Liquid Swipe Transition**——像黏稠液體的頁面轉場。

### 藝廊與媒體
* **Dome Gallery**——3D 全景藝廊。
* **Coverflow Carousel**——邊緣傾斜的 3D carousel。
* **Drag-to-Pan Grid**——無邊界可拖曳畫布。
* **Accordion Image Slider**——窄條在 hover 時展開。
* **Hover Image Trail**——滑鼠留下彈出的圖片軌跡。
* **Glitch Effect Image**——hover 時 RGB 通道錯位。

### 字體排印與文字
* **Kinetic Marquee**——隨捲動反向的無盡文字帶。
* **Text Mask Reveal**——巨型文字作為透向影片的窗。
* **Text Scramble Effect**——載入 / hover 時的 Matrix 式解碼。
* **Circular Text Path**——文字沿旋轉圓圈彎曲。
* **Gradient Stroke Animation**——描邊文字上流動的漸層。
* **Kinetic Typography Grid**——字母閃避游標。

### 微互動與效果
* **Particle Explosion Button**——CTA 在成功時碎成粒子。
* **Liquid Pull-to-Refresh**——重新載入指示像脫落的水滴。
* **Skeleton Shimmer**——placeholder 上掠過的光影。
* **Directional Hover-Aware Button**——填色從游標進入的那一側灌入。
* **Ripple Click Effect**——從點擊座標散開的波紋。
* **Animated SVG Line Drawing**——向量即時把自己畫出來。
* **Mesh Gradient Background**——有機的熔岩燈色塊。
* **Lens Blur Depth**——背景 UI 模糊以聚焦前景動作。

### 動畫函式庫選擇
* **Motion（`motion/react`）**——UI / Bento / 狀態變化動態的預設。
* **GSAP + ScrollTrigger**——整頁 scrolltelling 和滾動劫持用。隔離在專屬的葉節點元件，配 `useEffect` cleanup。
* **Three.js / WebGL**——canvas 背景和 3D 場景用。同樣的隔離規則。
* **絕不在同一棵元件樹混用 GSAP / Three.js 和 Motion。**它們會搶同一批影格。

---

## 11. 改版協定（REDESIGN PROTOCOL）

本 skill 同時處理**全新開發（greenfield）和改版**。誤判模式是壞改版產出的最大單一來源。

### 11.A 偵測模式（第一個動作）
* **Greenfield**——沒有既有網站，或已核准全面翻新。轉盤基準值照第 1 節。
* **改版——保留**——現代化但不破壞品牌。先稽核、抽取品牌 tokens、逐步演化。
* **改版——翻新**——在既有內容上換新的視覺語言。視覺當 greenfield 處理；保留內容和 IA。

模糊時，問**一次**：*「這次改版要保留既有品牌，還是視覺上從零開始？」*

### 11.B 動手前先稽核
提出改動之前，先記錄現況：
* **品牌 tokens**——主色 / 強調色、字型堆疊、logo 處理方式、圓角。
* **資訊架構（IA）**——頁面樹、主導覽、關鍵轉換路徑。
* **內容區塊**——有什麼、什麼在發揮作用、什麼是填充。
* **要保留的模式**——招牌互動、有辨識度的 hero、文案聲音。
* **要淘汰的模式**——AI 垃圾特徵、壞掉的版面、失效連結、generic 圖庫照、效能陷阱。
* **既有網站的轉盤判讀**——推斷現況的 `DESIGN_VARIANCE` / `MOTION_INTENSITY` / `VISUAL_DENSITY`。那是你的起點，不是基準值。
* **SEO 基線**——目前有排名的頁面、meta 標題、結構化資料、OG 卡。**SEO 遷移是改版第一大風險。**

### 11.C 保留規則
* **沒被要求就不改資訊架構。**保持頁面 slug、錨點 ID、主導覽標籤穩定，為了 SEO 也為了肌肉記憶。
* **套用第 4.2 節之前先抽取品牌色。**本來就是紫色的品牌繼續紫——套用紫色守則的覆寫條款。
* **沒被要求重寫就保留文案聲音。**視覺現代化 ≠ 內容重寫。
* **尊重既有的無障礙成果。**不倒退 focus 狀態、alt 文字、鍵盤導覽、對比。
* **尊重既有的分析事件。**不重新命名下游追蹤所依賴的按鈕、表單欄位、section ID。

### 11.D 現代化槓桿（優先順序）
按順序套用——需求滿足了就停：
1. **字體排印刷新**——每單位風險換到最大視覺提升。
2. **間距與節奏**——加大 section padding、修正垂直節奏。
3. **色彩重新校準**——去飽和、統一中性色、保留品牌強調色。
4. **動態層**——為既有元件加上符合 `MOTION_INTENSITY` 的微互動。
5. **Hero 與關鍵 section 重組**——用第 10 節詞彙重構漏斗頂端。
6. **整塊置換**——只在既有區塊無藥可救時。

### 11.E 決策樹：定向演化 vs 全面改版
* IA、內容、SEO 都健全 → **定向演化**（槓桿 1-4）。約 40% 的風險換到約 70% 的價值。
* 視覺債是結構性的（IA 壞掉、沒有設計系統、行動版壞掉）→ **全面改版**，嚴格保留內容。
* 品牌本身在變 → **greenfield**。

### 11.F 絕不默默改動的東西
沒有使用者明確核准，絕不修改：
* URL 結構 / 路由 slug。
* 主導覽標籤。
* 表單欄位名稱或順序（會弄壞分析 + 自動填入）。
* 品牌 logo 或字標。
* 既有的法律 / 同意 / cookie 文案。

---

## 12. THE BLOCK LIBRARY（契約——實作會逐步補進來）

參考詞彙（第 10 節）為模式命名；Block Library 用真實的 props、真實的動態規格、真實的程式碼草圖來實作它們。

**狀態：**schema 已在此定義。Block 會逐步加入。不要不照這個 schema 就擅自新增 block。

### 12.A 檔案位置
```
skills/taste-skill/blocks/
  hero/
    asymmetric-split.md
    editorial-manifesto.md
    kinetic-type.md
    ...
  feature/
    bento-grid.md
    sticky-scroll-stack.md
    zig-zag.md
    ...
  social-proof/
  pricing/
  cta/
  footer/
  navigation/
  portfolio/
  transition/
```

### 12.B 必要的 Frontmatter
```yaml
---
name: asymmetric-split-hero
category: hero
dial_compatibility:
  variance: [6, 10]
  motion: [3, 10]
  density: [2, 5]
when_to_use: "Landing pages with one strong asset and one strong message. Default hero for SaaS, agency, premium consumer."
not_for: "Editorial / manifesto launches where the message IS the design."
stack: ["react", "next", "tailwind", "motion"]
---
```

### 12.C 必要的本文章節
1. **視覺草圖**——版面的簡短 ASCII 圖或描述。
2. **Props API**——元件的介面。
3. **程式碼草圖**——最小可運作實作（預設 Server Component，動態放 Client island）。
4. **行動裝置 fallback**——`< 768px` 的明確收合規則。
5. **動態變體**——每個 `MOTION_INTENSITY` 區間（1-3、4-7、8-10）各一個變體。Reduced-motion fallback 明寫。
6. **暗模式筆記**——此 block 專屬的 token 策略。
7. **反模式**——此 block 常見的走鐘方式。
8. **參考資料**——正式上線的真實範例連結。

### 12.D Block-Library 紀律
* 一檔一 block。不准多 block 檔案。
* 每個 block 必須能獨立運作（丟進頁面就能渲染）。
* 每個 block 必須通過 Pre-Flight 檢查（第 14 節）。
* 相依於第 2.A 節某設計系統的 block，放在 `blocks/<category>/<name>--<system>.md`（例如 `feature/bento-grid--material.md`）。

---

## 13. 不在範圍內

本 skill **不**適用於：
* Dashboard / 高密度產品 UI / 管理後台（用第 2.A 節的 Fluent、Carbon、Atlassian 或 Polaris）。
* 資料表格（用 TanStack Table 或 AG Grid）。
* 多步驟表單 / 精靈（用表單專屬模式；本 skill 幫不上忙）。
* 程式碼編輯器（用 Monaco / CodeMirror 及其官方外觀客製）。
* 原生行動裝置（直接用 Apple HIG / Material）。
* 即時協作 UI（presence、游標、OT 感知——是另一類問題）。

如果需求屬於上述任一項，**明白說出來**，指向正確的工具，並且只把本 skill 的行銷頁 / 關於頁 / landing page 部分套在真正適用的表面上。

---

## 14. 最終 PRE-FLIGHT 檢查

輸出程式碼之前跑完這個矩陣。這是最後一道濾網。

**這不是可選的。每一格都要跑。任何一格不過，輸出就不算完成。**

- [ ] **需求推斷**已宣告（第 0.B 節的一行判讀）？
- [ ] **轉盤值**明確、且是從需求推理出來的，而不是默默用基準值？
- [ ] **設計系統**適用時已從第 2 節選定，或美學已誠實標示？
- [ ] **改版模式**已偵測且稽核已做（適用時，第 11 節）？
- [ ] **頁面上零個 em-dash（`—`）。**標題、eyebrow、膠囊、內文、引言、署名、圖說、按鈕、alt 文字。零個。（第 9.G 節——不可協商。）
- [ ] **頁面主題鎖**：整頁只有一個主題（亮、暗或自動）。沒有 section 在頁中翻成反轉模式（第 4.11 節）？
- [ ] **色彩一致性鎖**：一個強調色在所有 section 用法一致（第 4.2 節）？
- [ ] **形狀一致性鎖**：一套圓角系統一致套用（第 4.4 節）？
- [ ] **按鈕對比檢查**：每個 CTA 文字在其背景上可讀（沒有白配白，WCAG AA 4.5:1）？
- [ ] **CTA 按鈕換行**：桌面版沒有 CTA 標籤換成 2 行以上？
- [ ] **表單對比檢查**：表單輸入框、placeholder、focus ring、標籤全部對 section 背景通過 WCAG AA？
- [ ] **襯線紀律**：若用了襯線字型，它不是 Fraunces 或 Instrument_Serif（或者是，但有明確的品牌理由）？和你上一個專案用的襯線不同？
- [ ] **Premium-consumer 調色盤檢查**：若需求是 premium-consumer（鍋具 / wellness / 職人 / 奢華），調色盤不是 AI 預設的米白+黃銅+牛血紅+深咖啡家族？和你上一個 premium-consumer 專案的家族不同？
- [ ] **Italic 下伸部淨空**：每個含 `y g j p q` 的 italic 單字至少 `leading-[1.1]` + `pb-1` 預留？
- [ ] **Hero 塞進視窗**：標題 ≤ 2 行、副文案 ≤ 20 個詞且 ≤ 4 行、CTA 不捲動可見、字級有搭配圖片規劃？
- [ ] **Hero 頂部 padding**：桌面版最多 `pt-24`，hero 內容沒有飄到視窗一半？
- [ ] **Hero 堆疊紀律**：hero 最多 4 個文字元素（eyebrow 或品牌列、標題、副文案、CTA）？CTA 下方沒有小 tagline、hero 裡沒有信任微條？
- [ ] **EYEBROW 計數（機械式）**：統計所有元件中 section 標題上方 `uppercase tracking` 微標籤的出現次數。次數 ≤ ceil(sectionCount / 3)？Hero 算 1 個。
- [ ] **分割式標頭禁令**：沒有「左大標題 + 右小段說明」的 section 標頭模式（改為垂直堆疊）？
- [ ] **Zigzag 交錯上限**：沒有 3 個以上連續 section 用同一種圖文分割版面？
- [ ] **無重複 CTA 意圖**：沒有兩個相同意圖的 CTA（頁面上同時有「Get in touch」+「Let's talk」= Fail）？
- [ ] **Logo 牆 = 只有 logo**：logo 下方沒印產業 / 類別標籤？
- [ ] **Bento 背景多樣性**：至少 2-3 個 bento 格有真正的視覺變化（圖片、漸層、圖樣），不是全部白底白字卡？
- [ ] **「Used by / Trusted by」logo 牆**位於 hero 底下、不在 hero 裡，用**真實** SVG logo（Simple Icons / devicon）或生成的 SVG 標誌，**不是**純文字字標？
- [ ] **文案自我稽核**：每條可見字串都重讀過，沒有出貨文法壞掉或 AI 幻覺的句子（「free on its past」那類）？
- [ ] **動態有動機**：每個動畫都能用一句話說明理由（層級 / 敘事 / 回饋 / 狀態轉換），沒有為秀而秀的 GSAP？
- [ ] **Marquee 每頁最多一個**：同一頁沒有兩個水平 marquee？
- [ ] **導覽列桌面版單行**、高度 ≤ 80px？
- [ ] **Section 版面重複**檢查：沒有兩個 section 共用同一版面家族（8 個 section 至少 4 種家族）？
- [ ] **Bento 有節奏且格數精確**（N 個項目 → N 格，中間或結尾沒有空格）？
- [ ] **長清單用了正確的 UI 元件**（> 5 個項目不用預設 `<ul>` + `divide-y`——見第 4.9 節替代方案）？
- [ ] **用了真實圖片**（生成工具優先，其次 Picsum seed，再來明確的 placeholder 插槽）——沒有 div 拼的假截圖、沒有手刻裝飾 SVG、沒有純文字 minimalism？
- [ ] **圖片上沒疊膠囊/標籤**（沒有 `Plate · Brand`、沒有 `Field notes - journal`）？
- [ ] **沒把攝影署名圖說當裝飾**（`Field study no. 12 · Ines Caetano`）？
- [ ] **行銷頁沒有版本 footer**（`v1.4.2`、`Build 0048`）？
- [ ] **Eyebrow 下沒有 micro-meta 句子**（「Each of these is a feature we ship today...」）？
- [ ] **Hero 底部沒有裝飾文字條**（`BRAND. MOTION. SPATIAL.`）？
- [ ] **Section 標題沒有右上角漂浮小字**？
- [ ] **沒有帶填色背景軌道的計分/進度條**當比較視覺？
- [ ] **沒有地區 / 城市名 / 時間 / 天氣條**，除非需求真的是跨地分布或以地點為核心？
- [ ] **沒有捲動提示**（`Scroll`、`↓ scroll`、`Scroll to explore`）？
- [ ] **Hero 裡沒有版本標籤**（V0.6、BETA、INVITE-ONLY），除非需求就是產品發表？
- [ ] **沒有 section 編號 eyebrow**（`00 / INDEX`、`001 · Capabilities`、`06 · how it works`）？
- [ ] **沒有裝飾性圓點**（預設零顆，只給真實語意狀態）？
- [ ] **長清單 / 規格表沒有每列都 `border-t` + `border-b`**？
- [ ] **內容密度**合理：沒有 20 列資料表、沒有無理由的假精確規格、副段落預設 ≤ 25 個詞？
- [ ] **引言 ≤ 3 行**本文，署名乾淨（無 em-dash）？
- [ ] **宣稱的動態 = 看得到的動態**：若 `MOTION_INTENSITY > 4`，頁面真的會動，不是只有宣稱？
- [ ] **GSAP sticky-stack / 水平平移**依第 5.A / 5.B 節標準骨架實作（`start: "top top"`、`pin: true`、正確的 scrub）？
- [ ] **沒有 `window.addEventListener('scroll')`**——只用 Motion `useScroll()` / ScrollTrigger / IntersectionObserver / CSS scroll-driven animations？
- [ ] **Reduced motion**：所有 `MOTION_INTENSITY > 3` 的東西都包好了？
- [ ] **暗模式** tokens 已定義且兩種模式都測過？
- [ ] **行動裝置收合**明寫（`w-full`、`px-4`、`max-w-7xl mx-auto`）於高 variance 版面？
- [ ] **視窗穩定性**：`min-h-[100dvh]`，絕不 `h-screen`？
- [ ] **`useEffect` 動畫**有嚴格的 cleanup 函式？
- [ ] **空 / loading / 錯誤**狀態都有提供？
- [ ] **能用間距就省掉卡片**？
- [ ] **圖示**只來自允許的函式庫（Phosphor / HugeIcons / Radix / Tabler），沒有手刻 SVG path？
- [ ] **動態**隔離在頂端有 `'use client'` 的 client 葉節點元件、已 memoize？
- [ ] **沒有第 9 節的 AI 特徵**（Inter 當預設、AI 紫、三張等寬卡、Jane Doe、Acme、「Quietly in use at」）？
- [ ] **Core Web Vitals** 合理可達（LCP < 2.5s、INP < 200ms、CLS < 0.1）？
- [ ] **一個專案一個設計系統**（沒有 Material + shadcn 混用）？

只要有一格無法誠實打勾，頁面就不算完成。修好再交付。

---

# 附錄——有真實來源背書的參考素材

以下各節是收錄進來（vendored）的參考內容。它們為第 2 節點名的每個設計系統提供真實的安裝指令、真實的官方文件連結、以及真實可用的起手 snippet。用它們把決策錨定在生產現實上，而不是訓練資料的虛構。

## 附錄 A——各設計系統的安裝指令

```bash
# Material Web (Material 3)
npm install @material/web

# Fluent UI React (v9)
npm install @fluentui/react-components

# Fluent UI Web Components（不綁框架）
npm install @fluentui/web-components @fluentui/tokens

# IBM Carbon
npm install @carbon/react @carbon/styles

# Radix Themes
npm install @radix-ui/themes

# shadcn/ui（開放程式碼、元件歸你所有）
npx shadcn@latest init
npx shadcn@latest add button card badge separator input

# Primer CSS（GitHub 產品/開發工具 UI）
npm install --save @primer/css

# Primer Brand（GitHub 行銷 UI）
npm install @primer/react-brand

# GOV.UK Frontend
npm install govuk-frontend

# USWDS (US Web Design System)
npm install uswds

# Atlassian Design System (Atlaskit)
yarn add @atlaskit/css-reset @atlaskit/tokens @atlaskit/button @atlaskit/badge @atlaskit/section-message @atlaskit/card

# Bootstrap 5.3
npm install bootstrap

# Shopify Polaris Web Components（僅限 Shopify 應用）
# 把這段加進你的應用 HTML head：
#   <meta name="shopify-api-key" content="%SHOPIFY_API_KEY%" />
#   <script src="https://cdn.shopify.com/shopifycloud/polaris.js"></script>
```

## 附錄 B——標準來源（重新發明之前先讀這些）

### Material Web
- https://github.com/material-components/material-web
- https://material-web.dev/theming/material-theming/
- https://m3.material.io/develop/web

### Fluent UI
- https://fluent2.microsoft.design/get-started/develop
- https://fluent2.microsoft.design/components/web/react/
- https://github.com/microsoft/fluentui
- https://learn.microsoft.com/en-us/fluent-ui/web-components/

### Carbon
- https://carbondesignsystem.com/
- https://github.com/carbon-design-system/carbon
- https://carbondesignsystem.com/developing/react-tutorial/overview/
- https://carbondesignsystem.com/developing/web-components-tutorial/overview/

### Shopify Polaris
- https://shopify.dev/docs/api/app-home/web-components
- https://github.com/Shopify/polaris-react
- https://polaris-react.shopify.com/components

### Atlassian
- https://atlassian.design/get-started/develop
- https://atlassian.design/components/button/examples
- https://atlaskit.atlassian.com/packages/design-system/button/example/disabled
- https://atlassian.design/tokens/design-tokens

### Primer
- https://primer.style/
- https://github.com/primer/css
- https://github.com/primer/brand

### GOV.UK
- https://design-system.service.gov.uk/components/button/
- https://design-system.service.gov.uk/styles/layout/
- https://github.com/alphagov/govuk-frontend

### USWDS
- https://designsystem.digital.gov/documentation/developers/
- https://designsystem.digital.gov/components/button/
- https://designsystem.digital.gov/components/card/
- https://github.com/uswds/uswds

### Bootstrap
- https://getbootstrap.com/docs/5.3/layout/grid/
- https://getbootstrap.com/docs/5.3/components/card/

### Tailwind
- https://tailwindcss.com/docs/dark-mode
- https://tailwindcss.com/blog/tailwindcss-v4

### Radix
- https://www.radix-ui.com/themes/docs/components/theme
- https://www.radix-ui.com/themes/docs/components/card
- https://github.com/radix-ui/themes

### shadcn/ui
- https://ui.shadcn.com/docs
- https://ui.shadcn.com/docs/components/card
- https://github.com/shadcn-ui/ui

### 原生 CSS / W3C 標準
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/backdrop-filter
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/prefers-color-scheme
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/prefers-reduced-motion
- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout
- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Scroll-driven_animations
- https://drafts.csswg.org/scroll-animations-1/

### Apple Liquid Glass（僅限 Apple 平台）
- https://developer.apple.com/design/human-interface-guidelines/materials
- https://developer.apple.com/documentation/TechnologyOverviews/liquid-glass
- https://developer.apple.com/documentation/TechnologyOverviews/adopting-liquid-glass
- https://developer.apple.com/documentation/SwiftUI/Material

---

## 附錄 C——Apple Liquid Glass：誠實的 Web 近似

**不要**把隨便撿來的 CSS snippet 當成官方的 Apple Liquid Glass。

### 什麼是官方的
Apple 在其 Human Interface Guidelines 與 Developer Documentation 中為 **Apple 平台**記載了 Liquid Glass。它是用於 Apple 平台 UI 的動態材質。Apple 的原生實作屬於 Apple 平台 API 和系統元件，**不是公開的 web CSS 套件**。

相關官方文件：
- Apple Human Interface Guidelines → Materials
- Apple Developer Documentation → Liquid Glass
- Apple Developer Documentation → Adopting Liquid Glass
- SwiftUI → Material

### 什麼不是官方的
Apple 沒有給一般網站用的 `liquid-glass.css`。

Web 近似可以使用：
- `backdrop-filter`
- 透明背景
- 多層邊框
- highlight 疊層
- 漸層
- 動態
- 高對比 fallback

但那是 **web glassmorphism / 毛玻璃近似**，不是官方的 Apple Liquid Glass。在註解裡照實標示。

### 較安全的 web 近似骨架

```css
.liquid-glass-web-approx {
  position: relative;
  isolation: isolate;
  overflow: hidden;
  border-radius: 999px;
  border: 1px solid rgb(255 255 255 / .32);
  background:
    linear-gradient(135deg, rgb(255 255 255 / .30), rgb(255 255 255 / .08)),
    rgb(255 255 255 / .12);
  backdrop-filter: blur(24px) saturate(180%) contrast(1.05);
  -webkit-backdrop-filter: blur(24px) saturate(180%) contrast(1.05);
  box-shadow:
    inset 0 1px 0 rgb(255 255 255 / .48),
    inset 0 -1px 0 rgb(255 255 255 / .12),
    0 18px 60px rgb(0 0 0 / .18);
}

.liquid-glass-web-approx::before {
  content: "";
  position: absolute;
  inset: 0;
  z-index: -1;
  border-radius: inherit;
  background:
    radial-gradient(circle at 20% 0%, rgb(255 255 255 / .55), transparent 34%),
    linear-gradient(90deg, rgb(255 255 255 / .18), transparent 42%, rgb(255 255 255 / .14));
  pointer-events: none;
}

.liquid-glass-web-approx::after {
  content: "";
  position: absolute;
  inset: 1px;
  border-radius: inherit;
  border: 1px solid rgb(255 255 255 / .14);
  pointer-events: none;
}

@media (prefers-color-scheme: dark) {
  .liquid-glass-web-approx {
    border-color: rgb(255 255 255 / .18);
    background:
      linear-gradient(135deg, rgb(255 255 255 / .16), rgb(255 255 255 / .04)),
      rgb(15 23 42 / .42);
    box-shadow:
      inset 0 1px 0 rgb(255 255 255 / .22),
      0 18px 60px rgb(0 0 0 / .42);
  }
}

@media (prefers-reduced-transparency: reduce) {
  .liquid-glass-web-approx {
    background: rgb(255 255 255 / .96);
    backdrop-filter: none;
    -webkit-backdrop-filter: none;
  }
}
```

**重要：**`prefers-reduced-transparency` 的瀏覽器支援參差不齊；要測試。就算沒有 blur 也一律提供足夠的對比。

---

**附錄結束。**上面的安裝指令是現實的錨點。Apple Liquid Glass 骨架是有標示的近似，不是 Apple 發行的套件。各設計系統的標準文件請查該系統的官方文件（連結在第 2 節與附錄 B）。
