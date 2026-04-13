---
title: Signal Transform Strategies
created: 2026-04-07
updated: 2026-04-13
tags: [signal-processing, DFT, STFT, EMD, feature-engineering, 2026-04-07]
source: raw/google-slides/Sharon Research Meeting-2026-04-07.pdf
---

# Signal Transform Strategies

## Overview

This page summarizes the systematic comparison of different signal transform methods discussed in the 2026-04-07 research meeting, focusing on their applicability to mouse movement analysis.

## Comparison of Four Main Methods

| Method | Temporal Info | Assumption | Output | Typical Input | Use Cases |
|--------|---------------|------------|--------|---------------|-----------|
| **DFT** (Discrete Fourier Transform) | ❌ | Stationary | Frequency components | `[value(float)]` uniformly sampled | Audio pitch, EEG baseline bands |
| **STFT** (Short-Time Fourier Transform) | ✅ (limited) | Locally stationary | Time-frequency spectrogram | `[ts_ms, value(float)]` | Speech recognition, EEG alpha/beta/gamma |
| **2D-DFT** | ❌ (no time axis) | Stationary | Frequency image | `image[h][w]` 2D matrix | Image compression (JPEG), texture recognition |
| **EMD** (Empirical Mode Decomposition) | ✅ (adaptive) | None (handles nonlinearity) | IMFs (Intrinsic Mode Functions) | `[ts_ms, value(float)]` non-stationary series | Biomedical signals (EEG/ECG), financial time series, fault detection |

## Applicability to Mouse Movement Analysis

### Conclusion (2026-04-07)

> Although signal transforms can successfully decompose mouse movement signals, there is a lack of clear frequency-behavior mapping.
> For example: what behavior does a 10 Hz movement represent? How does it differ behaviorally from 5 Hz?

**Contrast**: In EEG research, alpha (8–13 Hz) = relaxation, beta (14–30 Hz) = active thinking — clear definitions exist. Mouse movement currently has no such correspondence.

### Potential Research Uses

**Use 1: Precursor to Feature Engineering**
- Even without interpreting individual frequencies, signal transforms can demonstrate the potential that extractable signals exist in mouse movement data
- Can serve as important features for predictive models

**Use 2: Comparative Research**
- If the "encouragement offering" group consistently shows lower power in high-frequency bands → empirical link: users in the "encouragement offering" condition tend to make fewer micro-corrections

## Sampling Methods and Matching Signal Transforms

The sampling method for mouse movement directly determines the applicable signal transform tools:

| Sampling Method | Matching Tool |
|-----------------|---------------|
| Window size (fixed point count per segment) | STFT |
| Fixed timing (fixed-interval sampling) | DFT |
| Fixed pixel range (fixed pixel distance) | 2D-DFT |
| Event type changes (transition-based segmentation) | EMD |

## Recommended Usage (This Research)

- Use EMD as exploratory data analysis (EDA) to examine whether features such as curvature and jitter contribute more strongly to long response times
- Not the primary hypothesis-testing tool, but a supplementary analysis

## See Also
- [[wiki/concepts/mouse-movement-metrics.md]]
- [[wiki/howto/mouse-tracking-pipeline.md]]
