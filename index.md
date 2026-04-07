# Research Wiki — 全站目錄

> 最後更新：2026-04-07 | 版本：v0.2（首次 Ingest）

---

## 快速導覽

| 區塊 | 說明 | 連結 |
|------|------|------|
| 核心概念 | 研究中的重要概念定義與脈絡 | [concepts/](wiki/concepts/) |
| 論文引用 | 相關論文摘要與分析 | [references/](wiki/references/) |
| 研究方法 | 操作流程與方法說明 | [howto/](wiki/howto/) |
| 演化史 | 研究問題與方法的演化時間軸 | [evolution/](wiki/evolution/) |
| Q&A 紀錄 | 歷次問答紀錄 | [logs/](wiki/logs/) |
| 產出物 | 自動生成的簡報與衍生檔案 | [artifacts/](wiki/artifacts/) |
| 操作日誌 | 完整時間軸紀錄 | [log.md](log.md) |

---

## 研究主題一覽

> 研究主軸：以 Promotion-Efficacy Fit 干預取代 Fear Appeal，減少健康自陳資料誤報；並以 RAM slider 鼠標追蹤訓練 ML 分類器偵測誤報行為。

---

## 核心概念（Concepts）

| 概念 | 說明 | 類別 |
|------|------|------|
| [Promotion-Efficacy Fit](wiki/concepts/promotion-efficacy-fit.md) | 核心干預構念：Promotion-focus × Self-efficacy × RAM slider | 核心構念 |
| [Regulatory Focus Theory](wiki/concepts/regulatory-focus-theory.md) | Promotion vs. Prevention 動機取向；Regulatory Fit 機制 | 核心理論 |
| [Protection Motivation Theory](wiki/concepts/protection-motivation-theory.md) | PMT / Fear Appeal 的理論基礎與歷史演化 | 對比理論 |
| [RAM Slider Bar](wiki/concepts/ram-slider.md) | 核心 UI 工具：以肢體互動成本抑制誇大回報 | 核心工具 |
| [Mouse Movement Metrics](wiki/concepts/mouse-movement-metrics.md) | RT、MD、Correction、Gap 等指標定義與採樣方法 | 測量 |
| [Signal Transform Strategies](wiki/concepts/signal-transform-strategies.md) | DFT / STFT / EMD 比較及對鼠標移動的適用性 | 方法論 |
| [Honesty-Humility](wiki/concepts/honesty-humility.md) | HEXACO 誠實-謙遜維度（控制變項） | 控制變項 |
| [Self-Construal](wiki/concepts/self-construal.md) | Singelis vs. Vignoles 七維量表（控制變項） | 控制變項 |
| [Self-Concept Clarity](wiki/concepts/self-concept-clarity.md) | SCC vs. Self-Consistency 選擇理由與量表 | 控制變項 |
| [Self-Regulation](wiki/concepts/self-regulation.md) | SDT 動機連續譜；與 RegFocus 和 Fear Appeal 的關係 | 控制變項 |
| [Protobuf vs. MsgPack](wiki/concepts/protobuf-vs-msgpack.md) | 資料序列化方案比較（DEV 議題） | DEV |

---

## 論文引用（References）

| 論文 | 作者 | 年份 | 核心貢獻 |
|------|------|------|---------|
| [Keller (2006)](wiki/references/keller-2006.md) | Keller, P. A. | 2006 | Promotion × Self-efficacy 提高防曬意圖；Regulatory Fit 實驗 |
| [Förster et al. (2003)](wiki/references/forster-2003.md) | Förster, Higgins, Bianco | 2003 | 促進焦點犧牲精確度換速度；RAM + 無速度壓力可緩解 |
| [Weinmann et al. (2022)](wiki/references/weinmann-2022.md) | Weinmann et al. | 2022 | 鼠標偏差與速度可偵測欺詐意圖；RAM 理論基礎 |
| [Khan et al. (2024)](wiki/references/khan-2024.md) | Khan et al. | 2024 | 鼠標動態生物識別完整 survey；特徵候選清單 |
| [Cooper et al. (2023)](wiki/references/cooper-2023.md) | Cooper et al. | 2023 | 誠實行為前因系統整理；研究空缺確認 |
| [Ho et al. (2025)](wiki/references/ho-2025.md) | Ho et al. | 2025 | 提供監督選擇 → 85% 選擇被監督且誠實；自主性工具 |

---

## 研究方法（How-To）

| 方法 | 說明 |
|------|------|
| [Mouse Tracking Pipeline](wiki/howto/mouse-tracking-pipeline.md) | 從前端捕捉到 S3 儲存到特徵工程的完整管線 |

---

## 演化史（Evolution）

- [研究問題演化史](wiki/evolution/research-question-evolution.md)：從 Mindfulness → RFT × Efficacy → Promotion-Efficacy Fit
- [研究方法演化史](wiki/evolution/research-method-evolution.md)：從問卷設計到鼠標追蹤 + ML 分類

---

## 最近更新

| 日期 | 操作 | 摘要 |
|------|------|------|
| 2026-04-07 | Ingest | 首次 Ingest：Sharon Research Meeting-2026-04-07.pdf，建立完整 Wiki 知識庫 |
| 2026-04-07 | 初始化 | 建立完整資料夾結構與初始檔案 |

---

## 系統說明

本 Wiki 遵循 **兩層分離 + 閉環管線** 架構：

- **L0 原始層**（`raw/`）：所有素材的進入點，永遠不修改
- **L1 知識層**（`wiki/`）：LLM 自動生成與維護的結構化知識庫

操作規則詳見 [CLAUDE.md](CLAUDE.md)。
