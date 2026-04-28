---
sidebar_label: 'Step 1. Prerequisites Transformations'
format: md
---

# Step 1. Prerequisites Transformations

> prepare a model before applying other low precision transformations.

Prerequisites transformations are optional. The transformations prepare a model before running other low precision transformations. The transformations do not operate with dequantization operations or update precisions. Prerequisites transformations include:

  - \[PullReshapeThroughDequantization\](step1-prerequisites/lin-op-sequence-fusion.md)
  - \[PullTransposeThroughDequantization\](step1-prerequisites/pull-transpose-through-dequantization.md)
  - \[LinOpSequenceFusion\](step1-prerequisites/lin-op-sequence-fusion.md)
  - \[ConvertSubtractConstant\](step1-prerequisites/convert-subtract-constant.md)
