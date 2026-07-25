---
name: stitch-design-taste
description: 給 Google Stitch 用的語意化設計系統 skill。產出對 agent 友善的 DESIGN.md 檔案，強制執行高階、反平庸的 UI 標準——嚴格的字體排印、校準過的色彩、不對稱版面、恆常運作的微動態，以及硬體加速的效能。
---

# Stitch Design Taste — 語意化設計系統 Skill

## 概觀
這個 skill 會產出針對 Google Stitch 畫面生成最佳化的 `DESIGN.md` 檔案。它把經過實戰驗證的反平庸（anti-slop）前端工程指令，翻譯成 Stitch 原生的語意化設計語言——用描述性的自然語言規則搭配精確的數值，讓 Stitch 的 AI agent 能夠解讀並產出高階、不平庸的介面。

產出的 `DESIGN.md` 是提示 Stitch 生成新畫面時的 **single source of truth**，確保新畫面符合一套經過策展、高自主性的設計語言。Stitch 透過 **「視覺描述（Visual Descriptions）」**，搭配具體的色彩值、字體規格與元件行為來理解設計。

## 前置條件
- 可透過 [labs.google/stitch](https://labs.google/stitch) 存取 Google Stitch
- 選用：Stitch MCP Server，用於與 Cursor、Antigravity 或 Gemini CLI 做程式化整合

## 目標
產出一份 `DESIGN.md`，編碼以下內容：
1. **視覺氛圍** — 調性、密度與設計哲學
2. **色彩校準** — 中性色、強調色，以及附上 hex 碼的禁用手法
3. **字體架構** — 字體堆疊、級距層級與反面手法
4. **元件行為** — 按鈕、卡片、輸入欄位及其互動狀態
5. **版面原則** — 網格系統、間距哲學、響應式策略
6. **動態哲學** — 動畫引擎規格、彈簧物理、恆常運作的微互動
7. **反面手法** — 明確列出被禁用的 AI 設計老套路

## 分析與綜整指引

### 1. 定義氛圍
評估目標專案的意圖。從品味光譜中挑選具畫面感的形容詞：
- **密度（Density）：** 「Art Gallery Airy」（1–3）→「Daily App Balanced」（4–7）→「Cockpit Dense」（8–10）
- **變異度（Variance）：** 「Predictable Symmetric」（1–3）→「Offset Asymmetric」（4–7）→「Artsy Chaotic」（8–10）
- **動態（Motion）：** 「Static Restrained」（1–3）→「Fluid CSS」（4–7）→「Cinematic Choreography」（8–10）

預設基準：Variance 8、Motion 6、Density 4。依使用者描述的 vibe 動態調整。

### 2. 對應色盤
每個顏色都要提供：**描述性名稱** ＋ **Hex 碼** ＋ **功能角色**。

**強制限制：**
- 最多 1 個強調色。飽和度低於 80%
- 「AI 紫／藍霓虹」美學嚴格禁用——不要紫色按鈕光暈，不要霓虹漸層
- 使用絕對中性的基底（Zinc／Slate），搭配高對比的單一強調色
- 整份輸出固定一套色盤——不要在暖灰與冷灰之間搖擺
- 絕對不要用純黑（`#000000`）——請用 Off-Black、Zinc-950 或 Charcoal

### 3. 建立字體排印規則
- **Display／標題：** 收緊字距、控制級距。不要用吼的。層級靠字重與顏色建立，而不是只靠字級放到很大
- **Body：** 放鬆的行高，每行最多 65 個字元
- **字體選擇：** 高階／創意情境禁用 `Inter`。強制使用有獨特個性的字體：`Geist`、`Outfit`、`Cabinet Grotesk` 或 `Satoshi`
- **襯線禁令：** 通用襯線字體（`Times New Roman`、`Georgia`、`Garamond`、`Palatino`）一律禁用。編輯風／創意情境若需要襯線，只能用有辨識度的現代襯線：`Fraunces`、`Gambarino`、`Editorial New` 或 `Instrument Serif`。儀表板與軟體 UI 中永遠禁用襯線
- **儀表板限制：** 只使用無襯線配對（`Geist` ＋ `Geist Mono`，或 `Satoshi` ＋ `JetBrains Mono`）
- **高密度覆寫：** 當密度超過 7，所有數字都必須使用等寬字體

### 4. 定義 Hero 區塊
Hero 是第一印象，必須有創意、有衝擊力，絕不能平庸：
- **行內圖像字體排印（Inline Image Typography）：** 把小張、貼合情境的照片或視覺元素直接嵌在標題的字詞或字母之間。圖片以字高行內排列、帶圓角，充當視覺標點。這是招牌創意手法
- **不重疊：** 文字絕對不能疊在圖片或其他文字之上。每個元素都佔據自己乾淨的空間區域
- **不要填充文字：** 「Scroll to explore」、「Swipe down」、捲動箭頭圖示、跳動的 chevron 一律禁用。內容本身就該把使用者吸進去
- **不對稱結構：** 變異度超過 4 時，置中的 Hero 版面一律禁用
- **CTA 克制：** 最多一個主要 CTA。不要放次要的「Learn more」連結

### 5. 描述元件樣式
針對每一類元件，描述形狀、顏色、陰影深度與互動行為：
- **按鈕：** active 狀態要有實體按壓般的回饋。不要霓虹外光暈。不要自訂滑鼠游標
- **卡片：** 只在「高度差能傳達層級」時才使用。陰影要調成呼應背景色相。高密度版面請用 border-top 分隔線或負空間取代卡片
- **輸入／表單：** 標籤在輸入框上方，輔助文字選用，錯誤訊息在下方。使用標準的間距
- **載入狀態：** 骨架載入（skeletal loader）要符合版面尺寸——不要通用的圓形轉圈
- **空狀態：** 用編排過的構圖說明如何填入資料
- **錯誤狀態：** 清楚的行內錯誤回報

### 6. 定義版面原則
- 元素不得重疊——每個元素都佔據自己清楚的空間區域。不要用絕對定位堆疊內容
- 變異度超過 4 時，置中的 Hero 區塊一律禁用——強制使用分割畫面、靠左對齊，或不對稱留白
- 通用的「三張等寬卡片橫排」功能列一律禁用——請用兩欄 Zig-Zag、不對稱網格或水平捲動
- CSS Grid 優先於 Flexbox 運算——絕對不要用 `calc()` 百分比的偏方
- 用 max-width 限制版面寬度（例如 1400px 置中）
- 滿版高度區塊必須用 `min-h-[100dvh]`——絕對不要用 `h-screen`（iOS Safari 會出現災難性跳動）

### 7. 定義響應式規則
每個設計都必須在所有 viewport 上正常運作：
- **Mobile-First 收合（< 768px）：** 所有多欄版面都收合成單欄。沒有例外
- **不得水平捲動：** 行動裝置上出現水平溢出屬於重大失敗
- **字級縮放：** 標題透過 `clamp()` 縮放。內文最小 `1rem`／`14px`
- **觸控目標：** 所有互動元素的點擊區至少 `44px`
- **圖片行為：** 行內字體排印圖片（夾在字詞之間的照片）在行動裝置上改為堆疊到標題下方
- **導覽：** 桌機水平導覽在行動裝置上收合成乾淨的選單
- **間距：** 垂直區塊間距等比縮小（`clamp(3rem, 8vw, 6rem)`）

### 8. 編碼動態哲學
- **預設彈簧物理：** `stiffness: 100, damping: 20`——高階、有重量的手感。不要線性緩動
- **恆常微互動：** 每個 active 的元件都應該有無限循環狀態（Pulse、Typewriter、Float、Shimmer）
- **交錯編排：** 清單絕不瞬間全部掛載——用串連延遲做出瀑布式揭露
- **效能：** 只透過 `transform` 與 `opacity` 做動畫。絕對不要動 `top`、`left`、`width`、`height`。顆粒／噪點濾鏡只能掛在固定的偽元素上

### 9. 列出反面手法（AI 破綻）
把這些以明確的「NEVER DO」規則寫進 DESIGN.md：
- 任何地方都不要 emoji
- 不要用 `Inter` 字體
- 不要用通用襯線字體（`Times New Roman`、`Georgia`、`Garamond`）——需要時只用有辨識度的現代襯線
- 不要用純黑（`#000000`）
- 不要霓虹／外光暈陰影
- 不要過飽和的強調色
- 大標題不要用過多的漸層文字
- 不要自訂滑鼠游標
- 元素不得重疊——永遠保持乾淨的空間分隔
- 不要三欄等寬卡片版面
- 不要罐頭姓名（「John Doe」、「Acme」、「Nexus」）
- 不要假的整數（`99.99%`、`50%`）
- 不要 AI 文案老套字眼（「Elevate」、「Seamless」、「Unleash」、「Next-Gen」）
- 不要填充式 UI 文字：「Scroll to explore」、「Swipe down」、捲動箭頭、跳動的 chevron
- 不要用會失效的 Unsplash 連結——請用 `picsum.photos` 或 SVG 頭像
- 不要置中的 Hero 區塊（高變異度專案）

## 輸出格式（DESIGN.md 結構）

```markdown
# Design System: [Project Title]

## 1. Visual Theme & Atmosphere
（描述調性、密度、變異度與動態強度，要有畫面感。
範例：「A restrained, gallery-airy interface with confident asymmetric layouts
and fluid spring-physics motion. The atmosphere is clinical yet warm — like a
well-lit architecture studio.」）

## 2. Color Palette & Roles
- **Canvas White** (#F9FAFB) — 主要背景表面
- **Pure Surface** (#FFFFFF) — 卡片與容器填色
- **Charcoal Ink** (#18181B) — 主要文字，Zinc-950 的深度
- **Muted Steel** (#71717A) — 次要文字、說明、metadata
- **Whisper Border** (rgba(226,232,240,0.5)) — 卡片邊框、1px 結構線
- **[Accent Name]** (#XXXXXX) — 單一強調色，用於 CTA、active 狀態、focus ring
（最多 1 個強調色。飽和度 < 80%。不要紫色／霓虹。）

## 3. Typography Rules
- **Display：** [Font Name] — 收緊字距、控制級距、以字重驅動層級
- **Body：** [Font Name] — 放鬆行高、65ch 最大寬度、中性的次要色
- **Mono：** [Font Name] — 用於程式碼、metadata、時間戳、高密度數字
- **禁用：** Inter、高階情境下的通用系統字體。儀表板中禁用襯線字體。

## 4. Component Stylings
* **按鈕：** 扁平、無外光暈。active 時有 -1px 位移的實體感。主要按鈕用強調色填滿，次要用 ghost／outline。
* **卡片：** 大方的圓角（2.5rem）。擴散式的微弱陰影。只在高度差服務於層級時使用。高密度情境：改用 border-top 分隔線。
* **輸入欄位：** 標籤在上、錯誤在下。focus ring 使用強調色。不要浮動標籤。
* **載入器：** 骨架微光（skeletal shimmer），完全貼合版面尺寸。不要圓形轉圈。
* **空狀態：** 編排過的插畫式構圖——不要只寫「No data」。

## 5. Layout Principles
（網格優先的響應式架構。Hero 區塊使用不對稱分割。
768px 以下嚴格收合為單欄。以 max-width 限制寬度。
不要 flexbox 百分比運算。內部 padding 大方。）

## 6. Motion & Interaction
（所有互動元素採用彈簧物理。交錯串連的揭露。
active 的儀表板元件有恆常微循環。只用硬體加速的
transform。CPU 吃重的動畫隔離在獨立的 Client Component 中。）

## 7. Anti-Patterns (Banned)
（明確列出禁用手法：不要 emoji、不要 Inter、不要純黑、
不要霓虹光暈、不要三欄等寬網格、不要 AI 文案老套字眼、
不要罐頭佔位姓名、不要失效的圖片連結。）
```

## 最佳實務
- **要有描述性：** 寫「Deep Charcoal Ink (#18181B)」，不要只寫「深色文字」
- **要講功能：** 說明每個元素是做什麼用的
- **要一致：** 整份文件使用相同的術語
- **要精確：** 在括號中附上確切的 hex 碼、rem 值與像素值
- **要有立場：** 這不是中立的範本——它強制執行一套特定的高階美學

## 成功要訣
1. 從氛圍開始——先理解 vibe，再細究 token
2. 找出模式——辨識一致的間距、尺寸與樣式
3. 用語意思考——依用途命名顏色，不要只依外觀
4. 考慮層級——記錄視覺重量如何傳達重要性
5. 把禁令寫進去——反面手法和正面規則一樣重要

## 常見陷阱
- 使用未經轉譯的技術術語（寫「rounded-xl」而不是「大方的圓角」）
- 省略 hex 碼，或只給描述性名稱
- 忘記寫設計元素的功能角色
- 氛圍描述太籠統
- 忽略反面手法清單——正是這些東西讓產出顯得高階
- 退回到通用的「安全」設計，而不是貫徹這套策展過的美學
