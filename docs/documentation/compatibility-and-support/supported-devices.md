---
sidebar_label: 'Supported Devices'
format: md
---

# Supported Devices

> of deep learning models.

The OpenVINO™ runtime enables you to use the following devices to run your
deep learning models:
\[CPU\](../../openvino-workflow/running-inference/inference-devices-and-modes/cpu-device.md),
\[GPU\](../../openvino-workflow/running-inference/inference-devices-and-modes/gpu-device.md),
\[NPU\](../../openvino-workflow/running-inference/inference-devices-and-modes/npu-device.md).

For their usage guides, see \[Devices and Modes\](../../openvino-workflow/running-inference/inference-devices-and-modes.md).  
For a detailed list of devices, see \[System Requirements\](../../about-openvino/release-notes-openvino/system-requirements.md).

Beside running inference with a specific device,
OpenVINO offers the option of running automated inference with the following inference modes:

\[Automatic Device Selection\](../../openvino-workflow/running-inference/inference-devices-and-modes/auto-device-selection.md):  
automatically selects the best device available for the given task. It offers many additional options and optimizations, including inference on multiple devices at the same time.

\[Heterogeneous Inference\](../../openvino-workflow/running-inference/inference-devices-and-modes/hetero-execution.md):  
enables splitting inference among several devices automatically, for example, if one device doesn't support certain operations.

\[Automatic Batching\](../../openvino-workflow/running-inference/inference-devices-and-modes/automatic-batching.md):  
automatically groups inference requests to improve device utilization.

## Feature Support and API Coverage

```html
<table>
<thead>
<tr class="header">
<th>Supported Feature</th>
<th>CPU</th>
<th>GPU</th>
<th>NPU</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>[Automatic Device Selection](../../openvino-workflow/running-inference/inference-devices-and-modes/auto-device-selection.md) Ye [Heterogeneous execution](../../openvino-workflow/running-inference/inference-devices-and-modes/hetero-execution.md) Ye [Automatic batching](../../openvino-workflow/running-inference/inference-devices-and-modes/automatic-batching.md) No [Multi-stream execution](../../openvino-workflow/running-inference/optimize-inference/optimizing-throughput.md) Ye</p>
</blockquote></td>
<td><p>s Ye s Ye Ye s Ye</p></td>
<td><p>s Pa s No s No s No</p></td>
<td><p>rtial</p></td>
</tr>
<tr class="even">
<td><blockquote>
<p>[Model caching](../../openvino-workflow/running-inference/optimize-inference/optimizing-latency/model-caching-overview.md) Ye [Dynamic shapes](../../openvino-workflow/running-inference/model-input-output/dynamic-shapes.md) Ye [Preprocessing acceleration](../../openvino-workflow/running-inference/optimize-inference/optimize-preprocessing.md) Ye</p>
</blockquote></td>
<td><p>s Pa s Pa s Ye</p></td>
<td><p>rtial Ye rtial No s No</p></td>
<td><p>s</p></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>[Stateful models](../../openvino-workflow/running-inference/inference-request/stateful-models.md) Ye [Extensibility](../../documentation/openvino-extensibility.md) Ye</p>
</blockquote></td>
<td><p>s Ye s Ye</p></td>
<td><p>s Ye s No</p></td>
<td><p>s</p></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 15%" />
<col style="width: 24%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>API Coverage:</strong></th>
<th>plugin</th>
<th>infer_request</th>
<th>compiled_model</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>CPU</td>
<td>98.31 %</td>
<td>100.0 %</td>
<td>90.7 %</td>
</tr>
<tr class="even">
<td>CPU_ARM</td>
<td>80.0 %</td>
<td>100.0 %</td>
<td>89.74 %</td>
</tr>
<tr class="odd">
<td>GPU</td>
<td>91.53 %</td>
<td>100.0 %</td>
<td>100.0 %</td>
</tr>
<tr class="even">
<td>dGPU</td>
<td>89.83 %</td>
<td>100.0 %</td>
<td>100.0 %</td>
</tr>
<tr class="odd">
<td>NPU</td>
<td>18.64 %</td>
<td>0.0 %</td>
<td>9.3 %</td>
</tr>
<tr class="even">
<td>AUTO</td>
<td>93.88 %</td>
<td>100.0 %</td>
<td>100.0 %</td>
</tr>
<tr class="odd">
<td>BATCH</td>
<td>86.05 %</td>
<td>100.0 %</td>
<td>86.05 %</td>
</tr>
<tr class="even">
<td>HETERO</td>
<td>61.22 %</td>
<td>99.24 %</td>
<td>86.05 %</td>
</tr>
<tr class="odd">
<td></td>
<td><div class="line-block">Percentage<br />
as of Open</div></td>
<td><blockquote>
<p>of API supported b</p>
</blockquote>
VINO 2024.5, 20 Nov</td>
<td>y the device, . 2024.</td>
</tr>
</tbody>
</table>
```

For setting up a relevant configuration, refer to the
\[Integrate with Customer Application\](../../openvino-workflow/running-inference.md)
topic (step 3 "Configure input and output").

Device support across OpenVINO 2024.6 distributions

| Device | Archives | PyPI | APT/YUM/ZYPPER | Conda | Homebrew | vcpkg | Conan | npm |
| ------ | -------- | ---- | -------------- | ----- | -------- | ----- | ----- | --- |
| CPU    | V        | V    | V              | V     | V        | V     | V     | V   |
| GPU    | V        | V    | V              | V     | V        | V     | V     | V   |
| NPU    | V\*      | V\*  | V\*            | n/a   | n/a      | n/a   | n/a   | V\* |

\* **Of the Linux systems, versions 22.04 and 24.04 include drivers for NPU.**  
 \**For Windows, CPU inference on ARM64 is not supported.*\*
