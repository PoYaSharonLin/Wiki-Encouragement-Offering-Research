---
title: Protobuf vs. MsgPack（資料序列化方案比較）
created: 2026-04-07
updated: 2026-04-07
tags: [dev, serialization, protobuf, msgpack, S3, data-storage]
source: raw/google-slides/Sharon Research Meeting-2026-04-07.pdf
---

# Protobuf vs. MsgPack（資料序列化方案比較）

## 概述（2026-04-07 DEV 議題）

為了儲存追蹤文件（tracking documents）到 S3，評估兩種二進制序列化方案。

## 比較表

| 特性 | Protobuf（法律合約） | MsgPack（握手協議） |
|------|--------------------|--------------------|
| Schema 要求 | 必要（.proto 定義） | 不需要（自描述格式） |
| 資料大小 | 通常更小 | 比 Protobuf 略大 |
| 序列化速度 | 極佳（schema 驅動優化） | 非常快，但 Protobuf 略勝 |
| 常見使用場景 | Microservices、gRPC API | Web API、實時訊息、遊戲 |
| 檔案開銷 | Schema 檔案 + 生成代碼 | 零額外開銷 |
| 調試工具 | 豐富（protoc、grpcurl、Wireshark plugins） | 較少工具 |

## 決策邏輯

```
是否有其他服務需要遵從 schema？
  ├─ 是 → Protobuf
  └─ 否 → 新代碼是否需要處理舊 schema 的資料？
              ├─ 是 → MsgPack
              └─ 否 → for others to clone and customize → MsgPack
                       for others to fork and develop → Protobuf
```

## S3 上傳時間比較

| 場景 | MsgPack 大小 | Protobuf 大小 | 差異（@ 1 Mbps） |
|------|-------------|--------------|-----------------|
| 5 min 典型 | ~200 KB | ~100 KB | ~1.6 sec |
| 10 min 典型 | ~400 KB | ~200 KB | ~1.6 sec |
| 10 min 重度使用 | ~1 MB | ~500 KB | ~2 sec |

## 當前實現

- **Frontend** (`tracker.js`)：使用 MsgPack 將 `{ type, events }` trajectory 物件編碼為 binary blob
- **Backend** (`behavior.rb`)：接收 blob 並直接 PUT 到 S3，不解碼
- 資料格式：`[x(int), y(int), movement_type(str), ts_ms(timestamp)]` 的 mixed-type array

## See Also
- [[wiki/howto/mouse-tracking-pipeline.md]]
- [[wiki/concepts/mouse-movement-metrics.md]]
