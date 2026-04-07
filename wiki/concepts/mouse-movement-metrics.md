---
title: Mouse Movement Metrics（鼠標移動指標）
created: 2026-04-07
updated: 2026-04-07
tags: [mouse-tracking, metrics, feature-engineering, signal-processing]
source: raw/google-slides/Sharon Research Meeting-2026-04-07.pdf
---

# Mouse Movement Metrics（鼠標移動指標）

## 資料格式

最基本的原始資料格式為 `[x, y, timestamp_ms]`。前端以 event type 分組儲存：

```json
{ "type": "MM", "events": [[x, y, "mousemove", ts], ...] }
{ "type": "PC", "events": [[x, y, "mousedown", ts], ...] }
```

## 四種採樣方法（決定 DB Schema）

| 方法 | 說明 | 適配 Signal Processing |
|------|------|----------------------|
| Window size（固定點數） | 每 N 點一組 | STFT |
| Fixed timing（固定時間） | 如每 16.67ms（60 FPS） | DFT |
| Fixed pixel range（固定像素距離） | 如每 >3px 記錄一次 | 2D-DFT |
| Event type changes（事件類型切換） | mousemove/mousedown/click | EMD |

**決策（2026-03-31）**：採樣方法決定資料庫 schema 與可用的特徵工程工具。

## 文獻中的主要指標分類

### Distance（距離）
- Traveled Distance / Curve length
- Deviation Distance
- Straightness / Efficiency = straight-line / actual path

### Time（時間）
- Reaction Time (RT)：問題出現到點擊回應的時間
- Maximum Deviation Time (MD-time)：到達最大偏移的時間

### Velocity（速度）
- Velocity / Horizontal / Vertical velocity
- Tangential / Angular velocity

### Acceleration（加速度）
- Horizontal / Vertical acceleration
- Tangential Jerk（加加速度）

### Curvature（曲率）
- Curvature / Rate of Curvature
- Curvature Velocity

### Shapes（形狀特徵）
- Angle of Movement / Total Angles / Bending Energy
- Self-Intersection
- Jitter（抖動）
- Central Moments

來源：Khan et al. (2024) — Mouse dynamics behavioral biometrics survey

## 本研究採用的關鍵指標（2026-03-31 決策）

- **Reaction Time (RT)**：問題出現到滑桿互動完成的時間
- **Maximum Deviation (MD)**：實際軌跡與理想直線的最大垂直距離
- 以 RT 和 MD 作為 ground truth 驗證「鼓勵提供」功能的有效性

**探索性分析**：使用 EMD（Empirical Mode Decomposition）分析曲率、抖動等特徵與反應時間的關係

## Signal Transform Strategies 的限制（2026-04-07 結論）

雖然信號轉換（DFT、STFT、EMD）能將鼠標移動分解為不同頻率成分，但：
- **缺乏有意義的頻率映射**：10 Hz 的移動代表什麼行為意義？如何與 5 Hz 區別？
- 對比 EEG（alpha/beta/gamma 頻帶有已知對應）或通訊信號，鼠標移動缺乏事先定義的頻率含義

Signal Transform 的潛在用途仍在於：
1. 作為特徵工程的前驅（探索信號潛力）
2. 作為比較性研究（高頻 vs 低頻特徵的組間差異）

## See Also
- [[wiki/concepts/signal-transform-strategies.md]]
- [[wiki/concepts/ram-slider.md]]
- [[wiki/references/weinmann-2022.md]]
- [[wiki/references/khan-2024.md]]
