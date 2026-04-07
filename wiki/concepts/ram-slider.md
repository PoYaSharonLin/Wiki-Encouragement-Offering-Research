---
title: RAM Slider Bar（反應激活模型滑桿）
created: 2026-04-07
updated: 2026-04-07
tags: [artifact, RAM, slider, mouse-tracking, response-cost]
source: raw/google-slides/Sharon Research Meeting-2026-04-07.pdf
---

# RAM Slider Bar（反應激活模型滑桿）

## 定義

基於 Response Activation Model（RAM）設計的 UI 干預工具，透過增加不誠實回報的「肢體互動成本」來抑制誇大行為。

## 設計理論（RAM 理論）

引自 Yu (2023)，基於 Welsh & Elliott (2004) 與 Weinmann et al. (2021)：

> "When there is intention to misreport, individuals might hesitate between multiple potential target locations, resulting in deviations from the simplest and swiftest possible movement."

因此，若增加與目標不誠實位置之間的肢體互動成本，使用者在實際操作前就會感知到努力程度，進而在行動前直面其不誠實意圖。

## 三種起始位置設計

| 起始位置 | 設計意圖 | 理論說明 |
|----------|----------|----------|
| 最左（最不健康） | 實驗組（增加互動成本） | 要報高分需要更多移動，RAM 效應最強 |
| 中間 | 對照組 | 無偏向 |
| 最右（最健康） | 反向對照 | 使用者不需「克服」不誠實意圖 |

## 依變項（Mouse-Tracking Metrics）

| 指標 | 定義 | 誠實 vs 不誠實 |
|------|------|---------------|
| Diet Score | 報告的健康分數 | 誠實 → 偏低（更接近真實） |
| Response Speed | 答題速度 | Ruby 的研究：快 → 更誠實 |
| Response Correction | 滑桿位置變更次數 | 多 → 更多衝突 → 可能不誠實 |
| Response Gap | max(Score_t-1 − Score_t) | 大 → 可能蓄意調高分數 |

## 與 Ruby 研究的對比

| 項目 | Ruby | Sharon |
|------|------|--------|
| RAM 起始位置 | 最不健康端 | 最不健康端（同） |
| 訊息干預 | Fear Appeal | Promotion-focus × Self-efficacy |
| 主要追蹤指標 | Speed | Correction + Gap + ML classification |

## 與鼠標追蹤的整合

RAM slider 蒐集的滑桿軌跡（x 位置 × 時間戳）與 Weinmann (2022) 的鼠標追蹤研究直接相關：
- 更多偏移（deviation from shortest path）→ 認知衝突（cognitive dissonance/load）
- 速度較慢 → Cognitive Load Theory

## See Also
- [[wiki/concepts/mouse-movement-metrics.md]]
- [[wiki/concepts/promotion-efficacy-fit.md]]
- [[wiki/references/weinmann-2022.md]]
