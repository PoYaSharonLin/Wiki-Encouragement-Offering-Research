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

| Method | Description | Signal Processing Fit | Citation |
|--------|-------------|----------------------|----------|
| Every motion downsampled (e.g., 5 pts) | Window of fixed point count per motion segment | STFT | Khan et al. (2024) |
| Motions sampled at given frequency 👾 | Fixed interval, e.g., every 16.67 ms (60 FPS) | DFT | Khan et al. (2024); Mazza et al. (2020) |
| Fixed pixel ranges in digital signing 🖌️ | Record when cursor moves >N px, common in e-signature | 2D-DFT | Nossam et al. (2024) |
| Event type changes | Segment by mousemove / mousedown / click transitions | EMD | Khan (2024); Weinmann et al. (2022) |

**決策（2026-03-31）**：採樣方法決定資料庫 schema 與可用的特徵工程工具。

## 文獻中的主要指標分類

### Distance（距離）
- Traveled Distance / Curve length (Khan et al., 2024)
- Deviation Distance (Khan et al., 2024; Weinmann et al., 2022)
- Straightness / Efficiency = straight-line / actual path (Khan et al., 2024; Weinmann et al., 2022)

### Time（時間）
- Reaction Time (RT)：問題出現到點擊回應的時間 (Khan et al., 2024; Mazza et al., 2020)
- Maximum Deviation Time (MD-time)：到達最大偏移的時間 (Khan et al., 2024)

### Velocity（速度）
- Movement Speed (Weinmann et al., 2022)
- Velocity / Horizontal / Vertical velocity (Khan et al., 2024)
- Tangential / Angular velocity (Khan et al., 2024)

### Acceleration（加速度）
- Horizontal / Vertical acceleration (Khan et al., 2024)
- Tangential Jerk（加加速度）(Khan et al., 2024)

### Curvature（曲率）
- Curvature / Rate of Curvature (Khan et al., 2024; Mazza et al., 2020)
- Curvature Velocity (Khan et al., 2024)

### Shapes（形狀特徵）
- Angle of Movement / Total Angles / Bending Energy (Khan et al., 2024)
- Self-Intersection (Khan et al., 2024)
- Jitter（抖動）(Khan et al., 2024; Mazza et al., 2020)
- Central Moments (Khan et al., 2024)

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

---

## References

Khan, S., Devlen, C., Manno, M., & Hou, D. (2024). Mouse dynamics behavioral biometrics: A survey. *ACM Computing Surveys*, *56*(6), 1–33.

Mazza, C., Monaro, M., Burla, F., Colasanti, M., Orrù, G., Ferracuti, S., & Roma, P. (2020). Use of mouse-tracking software to detect faking-good behavior on personality questionnaires: an explorative study. *Scientific Reports*, *10*(1), 4835.

Weinmann, M., Valacich, J. S., Schneider, C., Jenkins, J. L., & Hibbeln, M. (2022). The path of the righteous: Using trace data to understand fraud decisions in real time. *MIS Quarterly*, *46*(4), 2317–2336.

Nossam, S. C., Katakam, R. A., Pulastya, G., & Jayan, S. (2024). Signature forgery detection and verification using deep learning techniques. In *2024 15th International Conference on Computing Communication and Networking Technologies (ICCCNT)* (pp. 1–6). IEEE.
