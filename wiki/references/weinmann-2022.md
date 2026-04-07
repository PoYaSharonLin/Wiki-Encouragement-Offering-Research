---
title: The Path of the Righteous: Using Trace Data to Understand Fraud Decisions in Real Time
authors: [Weinmann, M., Valacich, J. S., Schneider, C., Jenkins, J. L., Hibbeln, M.]
year: 2022
venue: MIS Quarterly, 46(4), 2317-2336
created: 2026-04-07
tags: [mouse-tracking, fraud-detection, RAM, cognitive-load, MIS]
---

# The Path of the Righteous: Using Trace Data to Understand Fraud Decisions in Real Time

## 核心貢獻

驗證鼠標移動軌跡能反映欺詐意圖，具體發現：
- **Hypothesis 1**（Study 1）：在詐欺回應中，鼠標移動偏離最短路徑（RAM + Cognitive Dissonance）
- **Hypothesis 2**（Study 1）：在詐欺回應中，鼠標移動速度較慢（Cognitive Load Theory）
- Study 2 複製 Study 1，但在不同詐欺程度（extent）上測試

## 與本研究的關聯

RAM slider 的**理論基礎**：
- 詐欺意圖 → 認知衝突（Cognitive Dissonance）→ 更多移動偏差
- 詐欺意圖 → 認知負荷（Cognitive Load）→ 速度變慢
- Ruby 的研究（2023）直接以此為基礎
- Weinmann (2025) ICIS Short Paper 進一步以 tensor embeddings 豐富鼠標指標

## 關鍵方法（資料蒐集管線）

1. **Raw Data Capture**：x/y 坐標 + 毫秒時間戳
2. **Pixel Standardization**：標準化至 8×6 格（x×8/screen_width, y×6/screen_height）
3. **Measurement**：speed + average submovement deviation ratio = (actual distance) / (Euclidean distance)

## See Also
- [[wiki/concepts/ram-slider.md]]
- [[wiki/concepts/mouse-movement-metrics.md]]
