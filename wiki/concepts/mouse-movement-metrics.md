---
title: Mouse Movement Metrics
created: 2026-04-07
updated: 2026-04-13
tags: [mouse-tracking, metrics, feature-engineering, signal-processing]
source: raw/google-slides/Sharon Research Meeting-2026-04-07.pdf
---

# Mouse Movement Metrics

## Data Format

The most basic raw data format is `[x, y, timestamp_ms]`. The frontend stores data grouped by event type:

```json
{ "type": "MM", "events": [[x, y, "mousemove", ts], ...] }
{ "type": "PC", "events": [[x, y, "mousedown", ts], ...] }
```

## Four Sampling Methods (Determines DB Schema)

| Method | Description | Signal Processing Fit | Citation |
|--------|-------------|----------------------|----------|
| Every motion downsampled (e.g., 5 pts) | Window of fixed point count per motion segment | STFT | Khan et al. (2024) |
| Motions sampled at given frequency 👾 | Fixed interval, e.g., every 16.67 ms (60 FPS) | DFT | Khan et al. (2024); Mazza et al. (2020) |
| Fixed pixel ranges in digital signing 🖌️ | Record when cursor moves >N px, common in e-signature | 2D-DFT | Nossam et al. (2024) |
| Event type changes | Segment by mousemove / mousedown / click transitions | EMD | Khan (2024); Weinmann et al. (2022) |

**Decision (2026-03-31)**: The sampling method determines the database schema and available feature engineering tools.

## Key Metric Categories from the Literature

### Distance
- Traveled Distance / Curve length (Khan et al., 2024)
- Deviation Distance (Khan et al., 2024; Weinmann et al., 2022)
- Straightness / Efficiency = straight-line / actual path (Khan et al., 2024; Weinmann et al., 2022)

### Time
- Reaction Time (RT): Time from question appearance to click response (Khan et al., 2024; Mazza et al., 2020)
- Maximum Deviation Time (MD-time): Time to reach maximum deviation (Khan et al., 2024)

### Velocity
- Movement Speed (Weinmann et al., 2022)
- Velocity / Horizontal / Vertical velocity (Khan et al., 2024)
- Tangential / Angular velocity (Khan et al., 2024)

### Acceleration
- Horizontal / Vertical acceleration (Khan et al., 2024)
- Tangential Jerk (rate of change of acceleration) (Khan et al., 2024)

### Curvature
- Curvature / Rate of Curvature (Khan et al., 2024; Mazza et al., 2020)
- Curvature Velocity (Khan et al., 2024)

### Shapes
- Angle of Movement / Total Angles / Bending Energy (Khan et al., 2024)
- Self-Intersection (Khan et al., 2024)
- Jitter (Khan et al., 2024; Mazza et al., 2020)
- Central Moments (Khan et al., 2024)

## Key Metrics Adopted in This Research (Decision 2026-03-31)

- **Reaction Time (RT)**: Time from question appearance to completion of slider interaction
- **Maximum Deviation (MD)**: Maximum perpendicular distance between the actual trajectory and the ideal straight line
- RT and MD serve as ground truth to validate the effectiveness of the "encouragement offering" feature

**Exploratory analysis**: Use EMD (Empirical Mode Decomposition) to analyze the relationship between features such as curvature and jitter, and response time

## Limitations of Signal Transform Strategies (Conclusion 2026-04-07)

Although signal transforms (DFT, STFT, EMD) can decompose mouse movements into different frequency components:
- **Lack of meaningful frequency mapping**: What behavioral meaning does a 10 Hz movement have? How does it differ from 5 Hz?
- Unlike EEG (where alpha/beta/gamma bands have known behavioral correlates) or communication signals, mouse movement lacks pre-defined frequency meanings

Potential uses of Signal Transform remain:
1. As a precursor to feature engineering (exploring signal potential)
2. As comparative research (between-group differences in high-frequency vs. low-frequency features)

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
