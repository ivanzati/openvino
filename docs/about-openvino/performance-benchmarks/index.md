---
sidebar_label: 'Performance Benchmarks'
format: md
---

# Performance Benchmarks

> toolkit, that may help you decide what hardware to use or how
> to plan the workload.

This page presents benchmark results for the
[Intel® Distribution of OpenVINO™ toolkit](https://software.intel.com/content/www/us/en/develop/tools/openvino-toolkit.html)
and \[OpenVINO Model Server\](../../model-server/ovms\_what\_is\_openvino\_model\_server.md), for
a representative selection of public neural networks and Intel® devices. The results may help
you decide which hardware to use in your applications or plan AI workload for the hardware
you have already implemented in your solutions. Click the buttons below to see the chosen
benchmark data.

Note

Benchmark data for the OpenVINO inference engine is available only on the
[Model Hub page](https://www.intel.com/content/www/us/en/developer/tools/openvino-toolkit/model-hub.html).

1 1 2 2

[https://www.intel.com/content/www/us/en/developer/tools/openvino-toolkit/model-hub.html](https://www.intel.com/content/www/us/en/developer/tools/openvino-toolkit/model-hub.html)

`bar_chart;1.4em` OpenVINO Benchmark Graphs (general)

\#

`bar_chart;1.4em` OVMS Benchmark Graphs (general)

./performance-benchmarks/generative-ai-performance.html

`table_view;1.4em` LLM performance for AI PC

\#

`bar_chart;1.4em` OVMS for GenAI

**Key performance indicators and workload parameters**

Throughput

For Vision and NLP Models this measures the number of inferences delivered within a latency threshold
(for example, number of Frames Per Second - FPS).
For GenAI (or Large Language Models) this measures the token rate after the first token aka. 2nd token
throughput rate which is presented as tokens/sec. Please click on the "Workload Parameters" tab to
learn more about input/output token lengths, etc.

Latency

For Vision and NLP models this measures the synchronous execution of inference requests and
is reported in milliseconds. Each inference request (for example: preprocess, infer,
postprocess) is allowed to complete before the next one starts. This performance metric is
relevant in usage scenarios where a single image input needs to be acted upon as soon as
possible. An example would be the healthcare sector where medical personnel only request
analysis of a single ultra sound scanning image or in real-time or near real-time applications
such as an industrial robot's response to actions in its environment or obstacle avoidance
for autonomous vehicles.
For Transformer models like Stable-Diffusion this measures the time it takes to convert the prompt
or input text into a finished image. It is presented in seconds.

Workload Parameters

The workload parameters affect the performance results of the different models we use for
benchmarking. Image processing models have different image size definitions and the
Natural Language Processing models have different max token list lengths. All these can
be found in detail in the \[FAQ section\](performance-benchmarks/performance-benchmarks-faq.md).
All models are executed using a batch size of 1. Below are the parameters for the GenAI
models we display.

  - Input tokens: 1024,
  - Output tokens: 128,
  - number of beams: 1

For text to image:

  - iteration steps: 20,
  - image size (HxW): 256 x 256,
  - input token length: 1024 (the tokens for GenAI models are in English).

**Platforms, Configurations, Methodology**

To see the methodology used to obtain the numbers and learn how to test performance yourself,
see the guide on \[getting performance numbers\](performance-benchmarks/getting-performance-numbers.md).

For a listing of all platforms and configurations used for testing, refer to the following:

1 1 2 2

../\_static/download/benchmarking\_OV\_platform\_list.pdf

`download;1.5em` Click for Hardware Platforms \[PDF\]

../\_static/download/benchmarking\_OV\_system\_info\_detailed.xlsx

`download;1.5em` Click for Configuration Details \[XLSX\]

**Disclaimers**

  - System configurations used for Intel® Distribution of OpenVINO™ toolkit performance results
    are based on release 2026.0, as of February 25, 2026.
  - OpenVINO Model Server performance results are based on release 2025.3, as of September 3rd, 2025.

The results may not reflect all publicly available updates. Intel technologies' features and
benefits depend on system configuration and may require enabled hardware, software, or service
activation. Learn more at intel.com, the OEM, or retailer.

See configuration disclosure for details. No product can be absolutely secure.
Performance varies by use, configuration and other factors. Learn more at
[www.intel.com/PerformanceIndex](https://www.intel.com/PerformanceIndex).
Intel optimizations, for Intel compilers or other products, may not optimize to the same degree
for non-Intel products.

Results may vary. For more information, see
\[F.A.Q.\](./performance-benchmarks/performance-benchmarks-faq.md)
See \[Legal Information\](./additional-resources/terms-of-use.md).
