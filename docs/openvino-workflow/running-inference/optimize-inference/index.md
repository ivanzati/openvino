---
sidebar_label: 'Optimize Inference'
format: md
---

# Optimize Inference

> optimizations that can be done independently. Inference
> speed depends on latency and throughput.

Runtime, or deployment optimization focuses on tuning inference and execution parameters. Unlike
model-level optimization, it is highly specific to the hardware you use and the goal you want
to achieve. You need to plan whether to prioritize accuracy or performance,
\[throughput\](optimize-inference/optimizing-throughput.md) or \[latency\](optimize-inference/optimizing-latency.md),
or aim at the golden mean. You should also predict how scalable your application needs to be
and how exactly it is going to work with the inference component. This way, you will be able
to achieve the best results for your product.

Note

For more information on this topic, see the following articles:

  - \[Inference Devices and Modes\](inference-devices-and-modes.md)
  - *Inputs Pre-processing with the OpenVINO*
  - *Async API*
  - *The 'get\_tensor' Idiom*
  - For variably-sized inputs, consider \[dynamic shapes\](model-input-output/dynamic-shapes.md)

## Performance-Portable Inference

To make configuration easier and performance optimization more portable, OpenVINO offers the
\[Performance Hints\](optimize-inference/high-level-performance-hints.md) feature. It comprises
two high-level “presets” focused on latency **(default)** or throughput.

Although inference with OpenVINO Runtime can be configured with a multitude
of low-level performance settings, it is not recommended, as:

  - It requires deep understanding of device architecture and the inference engine.
  - It may not translate well to other device-model combinations. For example:
      - CPU and GPU deduce their optimal number of streams differently.
      - Different devices of the same type, favor different execution configurations.
      - Different models favor different parameter configurations (e.g., compute vs memory-bandwidth,
        inference precision, and possible model quantization).
      - Execution “scheduling” impacts performance strongly and is highly device specific. GPU-oriented
        optimizations \[do not always map well to the CPU\](optimize-inference/optimizing-low-level-implementation.md).

## Additional Resources

  - *Using Async API and running multiple inference requests in parallel to leverage throughput*.
  - \[The throughput approach implementation details for specific devices\](optimize-inference/optimizing-low-level-implementation.md)
  - \[Details on throughput\](optimize-inference/optimizing-throughput.md)
  - \[Details on latency\](optimize-inference/optimizing-latency.md)
  - \[API examples and details\](optimize-inference/high-level-performance-hints.md)
