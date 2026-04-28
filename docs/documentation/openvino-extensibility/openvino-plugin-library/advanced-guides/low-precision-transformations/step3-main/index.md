---
sidebar_label: 'Step 3. Main Transformations'
format: md
---

# Step 3. Main Transformations

> precision transformations that handle decomposition and
> dequantization operations.

Main transformations are the majority of low precision transformations. Transformations operate with dequantization operations. Main transformations include:

  - \[AddTransformation\](step3-main/arithmetic/add.md)
  - \[AvgPoolTransformation\](step3-main/pooling/avg-pool.md)
  - \[BatchToSpaceTransformation\](step3-main/shape/batch-to-space.md)
  - \[ClampTransformation\](step3-main/activation/clamp.md)
  - \[ConcatTransformation\](step3-main/movement/concat.md)
  - \[ConvolutionTransformation\](step3-main/convolution/convolution.md)
  - \[ConvolutionBackpropDataTransformation\](step3-main/convolution/convolution-backprop-data.md)
  - \[DepthToSpaceTransformation\](step3-main/movement/depth-to-space.md)
  - \[FakeQuantizeDecompositionTransformation\](step4-cleanup/fake-quantize-decomposition.md)
  - \[FakeQuantizeTransformation\](step3-main/quantization/fake-quantize.md)
  - \[InterpolateTransformation\](step3-main/image/interpolate.md)
  - \[GroupConvolutionTransformation\](step3-main/convolution/group-convolution.md)
  - \[GatherTransformation\](step3-main/movement/gather.md)
  - \[MatMulTransformation\](step3-main/matrix/mat-mul.md)
  - \[MaxPoolTransformation\](step3-main/pooling/max-pool.md)
  - \[MultiplyPartialTransformation\](step3-main/arithmetic/multiply-partial.md)
  - \[MultiplyTransformation\](step3-main/arithmetic/multiply.md)
  - \[MVNTransformation\](step3-main/normalization/mvn.md)
  - \[NormalizeL2Transformation\](step3-main/normalization/normalize-l2.md)
  - \[PadTransformation\](step3-main/movement/pad.md)
  - \[PReluTransformation\](step3-main/activation/prelu.md)
  - \[ReduceMaxTransformation\](step3-main/reduction/reduce-max.md)
  - \[ReduceMeanTransformation\](step3-main/reduction/reduce-mean.md)
  - \[ReduceMinTransformation\](step3-main/reduction/reduce-min.md)
  - \[ReduceSumTransformation\](step3-main/reduction/reduce-sum.md)
  - \[ReluTransformation\](step3-main/activation/relu.md)
  - \[ReshapeTransformation\](step3-main/shape/reshape.md)
  - \[SpaceToBatchTransformation\](step3-main/shape/space-to-batch.md)
  - \[SqueezeTransformation\](step3-main/shape/squeeze.md)
  - \[ShuffleChannelsTransformation\](step3-main/movement/shuffle-channels.md)
  - \[SplitTransformation\](step3-main/movement/split.md)
  - \[StridedSliceTransformation\](step3-main/movement/strided-slice.md)
  - \[TransposeTransformation\](step3-main/movement/transpose.md)
  - \[UnsqueezeTransformation\](step3-main/shape/unsqueeze.md)
  - \[VariadicSplitTransformation\](step3-main/movement/variadic-split.md)

Let's explore some main transformations on the example model. Original model:

![Original model](img/step3_original.svg)

Result model after main transformations:

![Transformed model](img/step3_transformed.svg)

Changes in the example model after main transformation:

  - All `FakeQuantize` operations (`fakeQuantize1`, `fakeQuantize2` and `fakeQuantize3`) were decomposed:
      - original `FakeQuantize` operations were replaced with new operations with other output intervals and output port precision,
      - dequantization operations.
  - Dequantization operations were moved via precision preserved (`concat1` and `concat2`) and quantized (`convolution2`) operations.

Note

The left branch (branch \#1) does not require per-tensor quantization. As a result, the `fakeQuantize1` output interval is \[0, 255\]. But quantized `convolution2` requires per-tensor quantization on the right branch (branch \#2). Then all connected `FakeQuantize` interval operations (`fakeQuantize1` and `fakeQuantize2`) are aligned to have per-tensor quantization after the concatenation (`concat2`) operation.
