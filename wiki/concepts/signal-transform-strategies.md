---
title: Signal Transform Strategies（信號轉換策略）
created: 2026-04-07
updated: 2026-04-07
tags: [signal-processing, DFT, STFT, EMD, feature-engineering, 2026-04-07]
source: raw/google-slides/Sharon Research Meeting-2026-04-07.pdf
---

# Signal Transform Strategies（信號轉換策略）

## 概述

本頁面整理 2026-04-07 研究會議中對不同信號轉換方法的系統性比較，討論其在鼠標移動分析中的適用性。

## 四種主要方法比較

| 方法 | 時間資訊 | 假設 | 輸出 | 典型輸入 | 適用場景 |
|------|----------|------|------|---------|---------|
| **DFT**（離散傅立葉變換） | ❌ | Stationary | 頻率成分 | `[value(float)]` 均勻採樣 | 音頻音調、EEG 基線頻帶 |
| **STFT**（短時傅立葉變換） | ✅（有限） | Locally stationary | 時頻圖 | `[ts_ms, value(float)]` | 語音識別、EEG alpha/beta/gamma |
| **2D-DFT** | ❌（無時間軸） | Stationary | 頻率圖像 | `image[h][w]` 二維矩陣 | 圖像壓縮（JPEG）、紋理識別 |
| **EMD**（經驗模態分解） | ✅（自適應） | None（可處理非線性） | IMFs（本徵模態函數） | `[ts_ms, value(float)]` 非平穩序列 | 生醫信號（EEG/ECG）、金融時序、故障偵測 |

## 對鼠標移動分析的適用性

### 結論（2026-04-07）

> 雖然信號轉換可以成功分解鼠標移動信號，但缺乏清晰的頻率-行為映射。
> 例如：10 Hz 的移動代表什麼行為？與 5 Hz 有何行為差異？

**對比**：在 EEG 研究中，alpha（8-13 Hz）= 放鬆、beta（14-30 Hz）= 主動思考，有明確定義。鼠標移動目前無此對應。

### 潛在研究用途

**用途 1：特徵工程的前驅**
- 即使無法解釋個別頻率，信號轉換可以展示「鼠標移動中存在可提取的信號」的潛力
- 可成為預測模型的重要特徵

**用途 2：比較性研究**
- 若「鼓勵提供」組在高頻帶的功率持續較低 → 有實證連結：在「鼓勵提供」條件下，使用者傾向減少微幅修正（micro-corrections）

## 採樣方法與適配的 Signal Transform

鼠標移動的採樣方法直接決定可使用的信號轉換工具：

| 採樣方法 | 適配工具 |
|----------|---------|
| Window size（固定窗口點數） | STFT |
| Fixed timing（固定時間採樣） | DFT |
| Fixed pixel range（固定像素距離） | 2D-DFT |
| Event type changes（事件類型切換） | EMD |

## 建議用法（本研究）

- 採用 EMD 作為探索性資料分析（EDA），檢查曲率、抖動等特徵是否對長反應時間有較強貢獻
- 非主要假設驗證工具，而是補充性分析

## See Also
- [[wiki/concepts/mouse-movement-metrics.md]]
- [[wiki/howto/mouse-tracking-pipeline.md]]
