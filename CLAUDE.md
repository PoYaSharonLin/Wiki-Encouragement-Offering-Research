# CLAUDE.md — 系統提示詞與 Schema 規則

> 本檔案是 LLM 操作此 Wiki 的完整行為規範。每次執行任何指令前，LLM 必須先讀取此檔案。

---

## 一、核心理念

### 來源一：Karpathy LLM Wiki 設計原則
- **原始層永不修改**：`raw/` 裡的所有檔案只能讀取，不能編輯或刪除
- **知識層持續演進**：`wiki/` 是唯一允許 LLM 寫入與更新的地方
- **每次操作必留紀錄**：任何 ingest、update、query 都必須在 `log.md` 寫入時間軸條目
- **index.md 是門面**：每次更新後必須同步更新 `index.md` 的相關區塊
- **內部連結優先**：wiki 內部頁面應儘量以 `[[相對路徑]]` 或 `[文字](相對路徑)` 互相連結

### 來源二：gigaai.studio 兩層分離 + 閉環管線
- **L0 原始層**（`raw/`）：所有外部素材的進入點，只分類不修改
- **L1 知識層**（`wiki/`）：LLM 主動轉化與建立關係的結構化知識庫
- **閉環原則**：知識庫越用越強化，每次 Query 的有價值發現都要回寫進 wiki

---

## 二、五階段管線（每次 Ingest 都要完整執行）

```
Capture → Compile → Link → Query → Feedback → Lint
```

| 階段 | 動作 | 輸出位置 |
|------|------|----------|
| **Capture** | 素材放入 `raw/`，不分類不修改 | `raw/` |
| **Compile** | LLM 分析素材，生成結構化頁面 | `wiki/concepts/`、`wiki/references/`、`wiki/howto/` |
| **Link** | 建立雙向連結，更新相關頁面的 See Also | 各 wiki 頁面 |
| **Query** | 交叉檢索所有筆記回答問題 | `wiki/logs/YYYY-MM-DD-qna-XXX.md` |
| **Feedback** | 有價值的發現回寫入 wiki | 相關 wiki 頁面 |
| **Lint** | 檢查斷連結、矛盾、過時內容 | `log.md` 的 Lint 區段 |

---

## 三、資料夾結構與用途

```
RESEARCH-WIKI/
├── raw/
│   ├── done/              ← 處理完的原始檔移到這裡（只移動，不修改）
│   └── google-slides/     ← 每周匯出的 Google Slides（YYYY-MM-DD-vX.md）
├── wiki/
│   ├── concepts/          ← 核心概念頁面（每個概念一個 .md 檔）
│   ├── references/        ← 論文與引用頁面（每篇論文一個 .md 檔）
│   ├── howto/             ← 研究方法與操作流程
│   ├── evolution/         ← 演化史專區（固定兩個核心檔案）
│   ├── logs/              ← Q&A 紀錄、操作紀錄
│   └── artifacts/         ← 自動產出的簡報、摘要等衍生檔案
├── index.md               ← 全站目錄 + 快速導覽（必須隨時保持最新）
├── log.md                 ← 時間軸操作紀錄（只能追加，不能刪除）
└── CLAUDE.md              ← 本檔案
```

---

## 四、檔案 Schema 規範

### concepts/ 頁面格式
```markdown
---
title: 概念名稱
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [tag1, tag2]
source: raw/google-slides/XXXX.md
---

# 概念名稱

## 定義
...

## 在研究中的角色
...

## 演化脈絡
（此概念如何隨研究推進而變化）

## See Also
- [[相關概念]]
- [[相關論文]]
```

### references/ 頁面格式
```markdown
---
title: 論文標題
authors: [作者1, 作者2]
year: YYYY
venue: 會議/期刊名稱
created: YYYY-MM-DD
tags: [tag1, tag2]
---

# 論文標題

## 核心貢獻
...

## 與本研究的關聯
...

## 關鍵方法
...

## See Also
- [[相關概念]]
- [[相關方法]]
```

### howto/ 頁面格式
```markdown
---
title: 方法名稱
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# 方法名稱

## 適用場景
...

## 步驟
1. ...
2. ...

## 注意事項
...

## See Also
```

### evolution/ 更新格式（追加在檔案末尾）
```markdown
### YYYY-MM-DD 更新
- 研究問題從「...」→「...」
- 研究方法從「...」→「...」
- 新增概念：[[概念名]]
- 來源：[raw/google-slides/XXXX.md](../raw/google-slides/XXXX.md)
```

---

## 五、指令集

### `Ingest new source: <路徑>`
執行完整五階段管線：
1. 讀取 `raw/` 下的指定檔案
2. Compile：生成或更新 `wiki/concepts/`、`wiki/references/`、`wiki/howto/`
3. Link：更新相關頁面的雙向連結
4. 更新 `wiki/evolution/research-question-evolution.md`
5. 更新 `wiki/evolution/research-method-evolution.md`
6. 更新 `index.md`
7. 在 `log.md` 寫入紀錄
8. 將處理完的原始檔移到 `raw/done/`（如果適合的話）

### `Query: <問題>`
1. 交叉檢索 `wiki/` 所有相關頁面
2. 生成回答
3. 將問答存入 `wiki/logs/YYYY-MM-DD-qna-001.md`
4. 若回答中有新洞見，回寫至相關 wiki 頁面

### `Add paper: <路徑或 DOI>`
1. 在 `wiki/references/` 建立論文頁面
2. 自動分類 tag
3. 連結到相關 concepts/howto 頁面
4. 更新 `index.md` 的 References 區塊
5. 在 `log.md` 寫入紀錄

### `Lint`
1. 掃描所有 wiki 內部連結，回報斷連結
2. 檢查 evolution/ 時間軸是否有缺口
3. 檢查 index.md 是否與實際檔案同步
4. 在 `log.md` 寫入 Lint 報告

---

## 六、log.md 寫入格式

每次操作必須在 `log.md` 追加：

```markdown
## YYYY-MM-DD HH:MM — <操作類型>

- **操作**：Ingest / Query / Add paper / Lint
- **來源**：`raw/...`（如有）
- **產出**：列出新增或更新的 wiki 頁面
- **摘要**：一句話說明本次操作的核心變化
```

---

## 七、LLM 行為守則

1. **不猜測，要引用**：所有 wiki 內容必須有來源連結指向 `raw/` 的原始素材
2. **不重複，要連結**：相同概念只建一個頁面，其他地方用連結
3. **追加不覆蓋**：`log.md` 和 `evolution/` 只能追加，不能刪除舊內容
4. **更新必同步**：每次更新 wiki 頁面後，必須同步更新 `index.md` 和 `log.md`
5. **保持簡潔**：每個頁面聚焦單一主題，避免一頁包山包海
