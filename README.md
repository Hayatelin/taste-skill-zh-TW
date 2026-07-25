# Taste Skill 繁體中文版（taste-skill-zh-TW）

> 專治 AI 做出來的介面一看就知道是 AI 做的。

這是 [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) 的**繁體中文在地化版本**。原專案是 2026 年最受注目的 agent skill 之一，用一整套設計規則取代 AI 的預設審美，讓產出的介面不再是那種千篇一律的樣板長相。

簡體中文版請看 [taste-skill-zh-CN](https://github.com/Hayatelin/taste-skill-zh-CN)。

---

## 這東西解決什麼問題

叫 AI 做一個 landing page，你大概能猜到會拿到什麼：置中的大標題、底下三個並排的圖示卡片、漸層背景、圓角按鈕、Inter 字體、一堆留白但沒有節奏。技術上沒有錯，但一眼就看得出是 AI 生的。

原因是 LLM 沒有在讀你的需求，它跳過理解直接輸出訓練資料裡最平均的那個樣子。

taste-skill 的做法是在動手之前先強制 agent 讀懂情境——這是什麼類型的頁面、給誰看、有沒有既有品牌資產、使用者用了哪些形容詞——然後才從對應的設計方向裡取用規則。每一條規則都是**情境觸發**的，不會無差別套用。

這跟 [Humanizer](https://github.com/kevintsai1202/Humanizer-zh-TW) 去除文字的 AI 味是同一件事的兩面：一個管文字，一個管介面。

---

## 內容

**核心設計 skill**

| Skill | 用途 |
|---|---|
| `taste-skill` | 主 skill。landing page、作品集、改版的完整設計規範（1200 行） |
| `taste-skill-v1` | 前一代版本，規則較精簡，適合輕量使用 |
| `redesign-skill` | 改版專用：先稽核現況，再決定保留什麼、翻掉什麼 |
| `output-skill` | 產出格式規範 |

**風格模組**

| Skill | 風格 |
|---|---|
| `minimalist-skill` | 極簡 |
| `brutalist-skill` | 粗獷主義（工業印刷／終端機感） |
| `soft-skill` | 柔性高質感視覺 |
| `gpt-tasteskill` | 給 ChatGPT 用的精簡版 |

**圖像生成與轉譯**

| Skill | 用途 |
|---|---|
| `imagegen-frontend-web` | 產生網頁設計參考圖（給 ChatGPT Images 等生成器） |
| `imagegen-frontend-mobile` | 產生行動裝置介面參考圖 |
| `image-to-code-skill` | 把設計圖忠實轉成可執行的前端程式碼 |
| `brandkit` | 品牌識別套件生成 |
| `stitch-skill` | 搭配 Google Stitch 的設計轉程式流程 |

典型用法是串起來的：先用 `imagegen-*` 產出參考圖板，再用 `image-to-code-skill` 交給 Codex／Cursor／Claude Code 實作。

---

## 安裝

### Claude Code

**方式一：plugin marketplace（最快）**

```
/plugin marketplace add Hayatelin/taste-skill-zh-TW
/plugin install taste-skill-zh-TW@taste-skill-zh-TW
```

**方式二：手動複製**

```bash
git clone https://github.com/Hayatelin/taste-skill-zh-TW.git
cp -r taste-skill-zh-TW/skills/* ~/.claude/skills/
```

Windows PowerShell：

```powershell
git clone https://github.com/Hayatelin/taste-skill-zh-TW.git
Copy-Item -Recurse taste-skill-zh-TW\skills\* $env:USERPROFILE\.claude\skills\
```

只裝主 skill 就夠用的話：

```bash
cp -r taste-skill-zh-TW/skills/taste-skill ~/.claude/skills/
```

### 其他 agent

| 工具 | 放置位置 |
|---|---|
| Cursor | `.cursor/skills/` |
| Codex | `~/.codex/skills/` |
| Gemini CLI | `~/.gemini/skills/` |
| Windsurf | `.windsurf/skills/` |
| GitHub Copilot | `.github/skills/` |

---

## 怎麼用

直接描述你要做的東西就好，skill 會自己判斷該不該啟動：

```
幫我做一個 SaaS 的 landing page，B2B，客戶是採購主管，要沉穩但不無聊
把這個作品集改版，保留現有的 logo 跟主色
用 brutalist 風格做一個活動頁
```

給 skill 越多情境線索（頁面類型、受眾、風格參考、既有品牌資產），產出的落差越大。它的第一件事就是輸出一行「設計判讀」告訴你它怎麼理解你的需求——不對就當場糾正它，比做完再改省事。

---

## 關於這個繁中版

**完整翻譯，沒有摘要。** 15 個檔案、約 6800 行，包含 1206 行的主 skill，全部逐節對照翻譯，章節、表格、程式碼區塊數與原文一致。

**設計術語保留英文或中英並列。** hero、CTA、above the fold、bento grid、full-bleed、negative space、type scale 這些詞，台灣設計圈本來就講英文，翻成中文反而看不懂。首次出現時以「中文（英文）」呈現，之後保留英文。

**風格名稱不翻。** brutalist、glassmorphism、minimalist、Neo-Grotesque 是專有風格名，翻掉就查不到資料了。

**技術值一律不動。** CSS 屬性與值、hex 色碼、字體名稱、Tailwind class、JSX 片段、套件名、URL，全部保持原樣。

**「禁用清單」保留英文原文。** 原專案有幾份清單列出禁止使用的 AI 味文案（"Elevate your"、"Seamless"、"Scroll to explore" 等）與填充語——這些是要讓 agent 比對英文原字的，翻成中文就失效了，因此原樣保留並附中文說明。

---

## 授權與致謝

MIT License。

- 原專案：[Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) © 2026 Leonxlnx — 所有設計方法論的內容都是他的功勞
- 繁體中文在地化：© 2026 [VictorLin](https://github.com/Hayatelin)

覺得好用的話，請優先去給[原專案](https://github.com/Leonxlnx/taste-skill)一顆星。

翻譯有問題、用詞不順、或發現漏譯，歡迎開 issue 或送 PR。
