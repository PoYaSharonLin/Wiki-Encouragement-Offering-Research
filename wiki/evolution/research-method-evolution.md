# 研究方法演化史

> 本檔案自動維護，每次 Ingest 後追加新條目。只能追加，不能修改舊條目。

---

## 使用說明

每個條目記錄一次重大的研究方法轉變。格式如下：

```
### YYYY-MM-DD 更新
- 研究方法從「...」→「...」
- 轉變原因：...
- 新增/棄用工具或框架：...
- 來源：[raw/google-slides/XXXX.md](../../raw/google-slides/XXXX.md)
```

---

## 時間軸

### 2026-04-07 — 初始化
- 研究方法：（待第一次 Ingest 後填入）
- 來源：系統初始化，尚無原始素材

---

### 2026-04-07 更新（首次 Ingest）

研究方法從 2025-10 至今的演化：

- **2025-10/11**：確立核心方法論 = 問卷實驗 + RAM slider bar + 鼠標追蹤；確認以 Ruby (2023) 作為對比基準
- **2025-12-02**：研究設計從 2×2 確定為 **3×2 Between-Subjects**（3 種訊息框架 × 2 種 slider 設計）
- **2026-02-24**：確立三個控制變項（Honesty-Humility、Treatment Self-Regulation、Self-Construal）
- **2026-03-10**：開始精簡各控制變項量表（每個 facet 選 2 題）
- **2026-03-17**：確定使用 Vignoles et al. (2016) 七維量表衡量 Self-Construal（非 Singelis 1994）
- **2026-03-24**：確定使用 SCC（非 Self-Consistency）；Vue.js + Ruby 問卷平台建立
- **2026-03-31**：確定以 RT 和 MD 作為 ground truth 指標；探索 EMD 信號分解
- **2026-04-07**：
  - DEV：評估 Protobuf vs MsgPack 序列化方案（當前使用 MsgPack）
  - RESEARCH：系統整理 DFT/STFT/2D-DFT/EMD 信號轉換策略；結論：缺乏頻率-行為映射，限制直接使用

- 新增方法：[[wiki/concepts/signal-transform-strategies.md]]、[[wiki/concepts/protobuf-vs-msgpack.md]]
- 來源：[raw/google-slides/Sharon Research Meeting-2026-04-07.pdf](../../raw/google-slides/Sharon Research Meeting-2026-04-07.pdf)

---

## 摘要視圖

> 以下為所有研究方法的精簡列表，便於快速瀏覽演化脈絡。

| 日期 | 研究方法（簡述） | 關鍵工具/框架 | 來源 |
|------|----------------|--------------|------|
| 2025-10/11 | 問卷實驗 + RAM slider + 鼠標追蹤 | Vue.js/Ruby 平台 | 初始構思 |
| 2025-12 | 3×2 Between-Subjects Design 確立 | RAM slider | Thesis Proposal |
| 2026-02 | 三控制變項確立 | HEXACO + SDT + Self-Construal | 2026-02 |
| 2026-03 | 量表精簡 + RT/MD ground truth | SCC + EMD | 2026-03 |
| 2026-04 | Protobuf/MsgPack 評估；信號轉換策略整理 | MsgPack + S3 + DFT/EMD | 2026-04-07 Ingest |
