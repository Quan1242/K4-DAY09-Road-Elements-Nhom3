# Ontology + CVAT setup

Bảng ontology là **source of truth** cho schema CVAT: `03_cvat_labels.json` phải khớp từng dòng ở đây. Thay mọi
placeholder mới là xong (gate G2).

## Ontology table

| Name | Geometry | Type (class / attribute) | Allowed values | Default | Mutable? | Rationale |
|---|---|---|---|---|---|---|
| `traffic_light` | Rectangle | Class | — | — | — | One instance for each separate signal head housing. |
| `state` | — | Attribute | `undefined`, `red`, `yellow`, `green`, `unknown`, `off` | `undefined` | Yes | Captures the visible signal state; `unknown` and `off` have different meanings. |
| `relevance` | — | Attribute | `undefined`, `ego_lane`, `other_lane`, `ambiguous` | `undefined` | Yes | Captures which vehicle lane the head controls; ambiguity must not be guessed away. |
| `shape` | — | Attribute | `undefined`, `circle`, `arrow`, `other` | `undefined` | No | Captures the visible signal form; it is separate from signal state and lane relevance. |

## Class hay attribute

`traffic_light` is the class because every annotated instance is a vehicle traffic signal with the same box rule. `state`, `relevance`, and `shape` are attributes because they describe the same signal head. All three default to `undefined`; annotators must set an observed value before export. Leaving the default unchanged is a quality defect, not a negative label.

## CVAT

- **Phiên bản CVAT** (`make cvat-status`): TODO
- **Tên task calibration** (có version guideline, ví dụ `team07-calib-v1`): TODO
- **Guide của task đã dán `02_guideline.md`?** TODO (có / chưa)
- **Nhóm dùng Track hay Shape, vì sao:** Shape — mỗi ảnh là quan sát độc lập và guideline không nội suy state qua frame.

## Setup test

Một thành viên **chưa tham gia setup** mở task và trả lời: label gì, dùng tool nào, gán attribute nào, khi nào
escalate. Ghi lại ai test và chỗ họ vấp:

TODO
