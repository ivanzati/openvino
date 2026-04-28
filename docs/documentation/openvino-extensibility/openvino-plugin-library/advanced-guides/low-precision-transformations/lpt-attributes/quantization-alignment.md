---
sidebar_label: 'QuantizationAlignment Attribute'
format: md
---

# QuantizationAlignment Attribute

`ov::QuantizationAlignmentAttribute` class represents the `QuantizationAlignment` attribute.

The attribute defines a subgraph with the same quantization alignment. `FakeQuantize` operations are not included. The attribute is used by quantization operations.

| Property name | Values          |
| ------------- | --------------- |
| Required      | Yes             |
| Defined       | Operation       |
| Properties    | value (boolean) |
