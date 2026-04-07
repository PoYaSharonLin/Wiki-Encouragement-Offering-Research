# 操作日誌（時間軸）

> 本檔案只能追加，不能刪除或修改舊條目。

---

## 2026-04-07 — 初始化

- **操作**：系統初始化
- **來源**：無（首次建立）
- **產出**：
  - 建立完整資料夾結構（`raw/`、`wiki/` 及所有子資料夾）
  - `CLAUDE.md`（系統提示詞與 Schema 規則）
  - `index.md`（全站目錄）
  - `log.md`（本檔案）
  - `wiki/evolution/research-question-evolution.md`（研究問題演化史模板）
  - `wiki/evolution/research-method-evolution.md`（研究方法演化史模板）
- **摘要**：Wiki 系統初始化完成，等待第一次 Ingest。

---

## 2026-04-07 14:00 — Ingest

- **操作**：Ingest
- **來源**：`raw/google-slides/Sharon Research Meeting-2026-04-07.pdf`（225 slides，涵蓋 2025-10-28 至 2026-04-07 研究會議紀錄）
- **產出**：
  - 新增 concepts（11 頁）：
    - `wiki/concepts/promotion-efficacy-fit.md`
    - `wiki/concepts/regulatory-focus-theory.md`
    - `wiki/concepts/protection-motivation-theory.md`
    - `wiki/concepts/ram-slider.md`
    - `wiki/concepts/mouse-movement-metrics.md`
    - `wiki/concepts/signal-transform-strategies.md`
    - `wiki/concepts/honesty-humility.md`
    - `wiki/concepts/self-construal.md`
    - `wiki/concepts/self-concept-clarity.md`
    - `wiki/concepts/self-regulation.md`
    - `wiki/concepts/protobuf-vs-msgpack.md`
  - 新增 references（6 頁）：
    - `wiki/references/keller-2006.md`
    - `wiki/references/forster-2003.md`
    - `wiki/references/weinmann-2022.md`
    - `wiki/references/khan-2024.md`
    - `wiki/references/cooper-2023.md`
    - `wiki/references/ho-2025.md`
  - 新增 howto（1 頁）：
    - `wiki/howto/mouse-tracking-pipeline.md`
  - 更新：`wiki/evolution/research-question-evolution.md`
  - 更新：`wiki/evolution/research-method-evolution.md`
  - 更新：`index.md`（v0.1 → v0.2）
- **摘要**：首次完整 Ingest。研究核心為以 Promotion-Efficacy Fit 干預（RFT × Self-efficacy × RAM slider）取代 Fear Appeal 減少健康誤報，同時以 MsgPack + S3 建構鼠標追蹤資料管線；2026-04-07 新增對 Signal Transform 策略的系統整理，結論為鼠標移動缺乏明確頻率-行為映射。
