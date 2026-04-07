---
title: Mouse Tracking Data Pipeline（鼠標追蹤資料管線）
created: 2026-04-07
updated: 2026-04-07
---

# Mouse Tracking Data Pipeline（鼠標追蹤資料管線）

## 適用場景

蒐集 RAM slider 行為資料，從前端事件捕捉到 S3 儲存到後端特徵工程。

## 步驟

### Step 1：前端資料捕捉（tracker.js）

以 MsgPack 編碼，格式為 mixed-type array：
```javascript
// 單一事件格式
[x(int), y(int), movement_type(str), ts_ms(timestamp)]

// Trajectory 物件
{ "type": "MM", "events": [[x, y, "mousemove", ts], ...] }
{ "type": "PC", "events": [[x, y, "mousedown", ts], [x, y, "mouseup", ts], [x, y, "click", ts]] }
```

序列化：Frontend 使用 MsgPack 將 trajectory 物件編碼為 binary blob

### Step 2：後端接收 + S3 上傳（behavior.rb）

```
Frontend → PUT /upload → Backend → PUT S3（不解碼，直接轉傳）
```

> 後端不解碼，直接以 binary blob 形式上傳至 S3，降低延遲與複雜度

### Step 3：採樣方法選擇（決定 DB Schema）

| 方法 | 觸發條件 | 適配分析 |
|------|----------|---------|
| Window size | 每 N 個事件 | STFT |
| Fixed timing | 每 16.67ms（60 FPS） | DFT |
| Fixed pixel range | 位移 > 3px | 2D-DFT |
| Event type changes | mousemove/mousedown/click 切換 | EMD |

**現況（2026-03-31）**：採樣方法尚待決定（見 Open Questions）

### Step 4：特徵工程

基本指標（只需 x, y, timestamp）：
- **Reaction Time (RT)**：問題出現 → 第一次互動的時間差
- **Maximum Deviation (MD)**：實際軌跡與理想直線的最大垂直距離
- Speed：總移動距離 / 總時間
- Straightness：直線距離 / 實際路徑距離

進階指標（來自 Khan et al., 2024）：
- Jitter（抖動）、Curvature、Angular velocity 等

探索性分析：使用 EMD 分解信號，分析特徵貢獻

### Step 5：RAM Slider 特有指標

- **Response Correction**：滑桿改變方向的次數（多 = 更多衝突）
- **Response Gap**：max|Score_t-1 − Score_t|（大 = 可能蓄意調整）

## 注意事項

1. 採樣方法決定後，才能最終確定 DB Schema
2. Protobuf 比 MsgPack 在 S3 上傳時間約快 50%（但需要維護 .proto schema）
3. Signal Transform 策略目前僅有探索性價值，缺乏頻率-行為的明確映射

## Open Questions（2026-03-31 狀態）

- ❓ 哪種採樣方法？（固定時間 / 固定像素範圍 / 事件類型切換）
- ❓ RT 和 MD 作為 ground truth 是否足夠？

## See Also
- [[wiki/concepts/mouse-movement-metrics.md]]
- [[wiki/concepts/signal-transform-strategies.md]]
- [[wiki/concepts/protobuf-vs-msgpack.md]]
- [[wiki/references/weinmann-2022.md]]
