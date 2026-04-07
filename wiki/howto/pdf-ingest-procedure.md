---
title: PDF Ingest 操作程序
created: 2026-04-07
updated: 2026-04-07
---

# PDF Ingest 操作程序

## 適用場景

收到任何 PDF 素材（研究會議簡報、論文、報告）時，如何系統性地將它轉化為 Wiki 知識頁面。

---

## 步驟

### Step 0：確認檔案位置

PDF 必須放在 `raw/` 下（或子資料夾如 `raw/google-slides/`）。
**不要修改原始 PDF，只讀取。**

---

### Step 1：讀取 PDF（分批次）

PDF 每次最多讀 20 頁。用 `Read` tool 加 `pages` 參數分批讀取：

```
Read(file_path="raw/.../file.pdf", pages="1-20")
Read(file_path="raw/.../file.pdf", pages="21-40")
...
```

> **策略**：先讀前 20 頁建立整體框架（研究問題、核心概念），再讀後續頁面補充細節。
> 大型 PDF（100+ 頁）可先讀首尾各一批，掌握脈絡後再決定哪些段落值得深入。

---

### Step 2：建立概念清單（Compile 前規劃）

讀取後，先在心裡（或草稿）列出：
- 哪些**核心概念**需要建 `wiki/concepts/` 頁面？
- 哪些**論文引用**需要建 `wiki/references/` 頁面？
- 哪些**操作流程**需要建 `wiki/howto/` 頁面？

避免重複：先用 `Glob` 確認是否已有對應頁面，若有則更新而非新建。

```
Glob(pattern="wiki/concepts/*.md")
Glob(pattern="wiki/references/*.md")
```

---

### Step 3：Compile — 生成 Wiki 頁面

依序建立或更新頁面，遵循 CLAUDE.md 的 Schema 規範：

- **concepts/**：每個核心概念一個 `.md`，含定義、研究角色、演化脈絡
- **references/**：每篇論文一個 `.md`，含核心貢獻、與本研究關聯、關鍵方法
- **howto/**：操作流程頁面

每頁都要加 `source:` 指向原始 PDF 路徑。

---

### Step 4：Link — 建立雙向連結

每個新頁面的 `See Also` 區塊都要連結相關頁面。
同時回到**已存在的相關頁面**，在它們的 `See Also` 補上指向新頁面的連結。

---

### Step 5：更新 Evolution 檔案

在以下兩個檔案末尾追加本次更新（**只追加，不覆蓋**）：

- `wiki/evolution/research-question-evolution.md`
- `wiki/evolution/research-method-evolution.md`

格式見 CLAUDE.md 第四節。

---

### Step 6：更新 index.md

在 `index.md` 的對應表格中新增所有新頁面的條目。
更新頁首的「最後更新」日期與「最近更新」區塊。

---

### Step 7：寫入 log.md

在 `log.md` 末尾追加操作紀錄：

```markdown
## YYYY-MM-DD HH:MM — Ingest

- **操作**：Ingest
- **來源**：`raw/.../file.pdf`
- **產出**：列出所有新增/更新的頁面
- **摘要**：一句話說明核心變化
```

---

### Step 8：移動原始檔（選擇性）

若該 PDF 已完整處理，將其移至 `raw/done/`：

```bash
mv "raw/.../file.pdf" "raw/done/file.pdf"
```

> 若 PDF 屬於持續更新的系列（如每週會議），可留在原位等下次版本進來再移。

---

## 注意事項

1. **大型 PDF**（如 225 頁簡報）：不需要讀完每一頁。策略是：
   - 讀前 20 頁確立研究問題與核心構念
   - 根據目錄或標題判斷哪些段落有新內容
   - 重點讀有實質方法或新概念的段落
   - 跳過純圖表或重複說明的頁面

2. **論文 PDF vs. 簡報 PDF**：
   - 論文：直接建 `wiki/references/` 頁面，用 `Add paper` 指令流程
   - 簡報（Google Slides 匯出）：先 Compile concepts/howto，再從中抽出論文引用

3. **不要猜測**：所有 wiki 內容必須有來源可追溯，不確定的內容用「（待確認）」標記

4. **並行建立**：多個獨立頁面可以用並行 Write tool calls 加速，但 index.md 和 log.md 要最後統一更新

---

## See Also

- [[wiki/howto/mouse-tracking-pipeline.md]]
- [CLAUDE.md](../../CLAUDE.md) — 完整指令集與 Schema 規範
