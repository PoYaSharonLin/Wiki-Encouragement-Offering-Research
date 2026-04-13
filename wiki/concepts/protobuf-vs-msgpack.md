---
title: Protobuf vs. MsgPack
created: 2026-04-07
updated: 2026-04-13
tags: [dev, serialization, protobuf, msgpack, S3, data-storage]
source: raw/google-slides/Sharon Research Meeting-2026-04-07.pdf
---

# Protobuf vs. MsgPack

## Overview (2026-04-07 Dev Topic)

Evaluation of two binary serialization formats for storing tracking documents to S3.

## Comparison Table

| Feature | Protobuf ("legal contract") | MsgPack ("handshake protocol") |
|---------|-----------------------------|---------------------------------|
| Schema requirement | Required (.proto definition) | Not required (self-describing format) |
| Data size | Generally smaller | Slightly larger than Protobuf |
| Serialization speed | Excellent (schema-driven optimization) | Very fast, but slightly slower than Protobuf |
| Common use cases | Microservices, gRPC API | Web APIs, real-time messaging, games |
| File overhead | Schema file + generated code | Zero overhead |
| Debugging tools | Rich (protoc, grpcurl, Wireshark plugins) | Fewer tools available |

## Decision Logic

```
Do other services need to conform to the schema?
  ├─ Yes → Protobuf
  └─ No → Does new code need to handle data from old schemas?
              ├─ Yes → MsgPack
              └─ No → for others to clone and customize → MsgPack
                       for others to fork and develop → Protobuf
```

## S3 Upload Time Comparison

| Scenario | MsgPack size | Protobuf size | Difference (@ 1 Mbps) |
|----------|-------------|---------------|-----------------------|
| 5 min typical | ~200 KB | ~100 KB | ~1.6 sec |
| 10 min typical | ~400 KB | ~200 KB | ~1.6 sec |
| 10 min heavy use | ~1 MB | ~500 KB | ~2 sec |

## Current Implementation

- **Frontend** (`tracker.js`): Uses MsgPack to encode `{ type, events }` trajectory objects as a binary blob
- **Backend** (`behavior.rb`): Receives the blob and PUTs it directly to S3 without decoding
- Data format: mixed-type array of `[x(int), y(int), movement_type(str), ts_ms(timestamp)]`

## See Also
- [[wiki/howto/mouse-tracking-pipeline.md]]
- [[wiki/concepts/mouse-movement-metrics.md]]
