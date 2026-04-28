---
sidebar_label: 'Libraries for Local Distribution'
format: md
---

# Libraries for Local Distribution

> Runtime binaries along with a set of required libraries
> needed to deploy the application.

With local distribution, each C or C++ application/installer has its own copies of OpenVINO Runtime binaries.
However, OpenVINO has a scalable plugin-based architecture, which means that some components
can be loaded in runtime only when they are really needed. This guide helps you understand
what minimal set of libraries is required to deploy the application.

Local distribution is also suitable for OpenVINO binaries built from source using
[Build instructions](https://github.com/openvinotoolkit/openvino/wiki#how-to-build),
but this guide assumes that OpenVINO Runtime is built dynamically.
For [Static OpenVINO Runtime](https://github.com/openvinotoolkit/openvino/blob/master/docs/dev/static_libaries.md),
select the required OpenVINO capabilities at the CMake configuration stage using
[CMake Options for Custom Compilation](https://github.com/openvinotoolkit/openvino/blob/master/docs/dev/cmake_options_for_custom_compilation.md),
then build and link the OpenVINO components to the final application.

Note

The steps below are independent of the operating system and refer to the library file name
without any prefixes (like `lib` on Unix systems) or suffixes (like `.dll` on Windows OS).
Do not put `.lib` files on Windows OS to the distribution because such files are needed
only at a linker stage.

## Library Requirements for C++ and C Languages

Regardless of the programming language of an application, the `openvino` library must always
be included in its final distribution. This core library manages all inference and frontend plugins.
The `openvino` library depends on the TBB libraries which are used by OpenVINO Runtime
to optimally saturate devices with computations.

If your application is in C language, you need to additionally include the `openvino_c` library.

## Libraries for Pluggable Components

The picture below presents dependencies between the OpenVINO Runtime core and pluggable libraries:

![image](img/deployment_full.svg)

### Libraries for Compute Devices

For each inference device, OpenVINO Runtime has its own plugin library:

  - `openvino_intel_cpu_plugin` for \[Intel® CPU devices\](../running-inference/inference-devices-and-modes/cpu-device.md)
  - `openvino_intel_gpu_plugin` for \[Intel® GPU devices\](../running-inference/inference-devices-and-modes/gpu-device.md)
  - `openvino_intel_npu_plugin` for \[Intel® NPU devices\](../running-inference/inference-devices-and-modes/npu-device.md)
  - `openvino_arm_cpu_plugin` for \[ARM CPU devices\](../running-inference/inference-devices-and-modes/cpu-device.md)

Depending on which devices are used in the app, the corresponding libraries should be included in the distribution package.

As shown in the picture above, some plugin libraries may have OS-specific dependencies
which are either backend libraries or additional supports files with firmware, etc.
Refer to the table below for details:

Windows

```html
<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 26%" />
<col style="width: 57%" />
</colgroup>
<thead>
<tr class="header">
<th>Device</th>
<th>Dependency</th>
<th>Location</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>CPU</p>
</blockquote></td>
<td><blockquote>
<p>—</p>
</blockquote></td>
<td><blockquote>
<p>—</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>GPU</p>
</blockquote></td>
<td><div class="line-block">OpenCL.dll<br />
cache.json<br />
</div></td>
<td><div class="line-block"><code>C:\Windows\System32\opencl.dll</code><br />
<code>.\runtime\bin\intel64\Release\cache.json</code> or<br />
<code>.\runtime\bin\intel64\Debug\cache.json</code></div></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>NPU</p>
</blockquote></td>
<td><blockquote>
<p>—</p>
</blockquote></td>
<td><blockquote>
<p>—</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>Arm® CPU</p>
</blockquote></td>
<td><blockquote>
<p>—</p>
</blockquote></td>
<td><blockquote>
<p>—</p>
</blockquote></td>
</tr>
</tbody>
</table>
```

Linux arm64

```html
<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 26%" />
<col style="width: 57%" />
</colgroup>
<thead>
<tr class="header">
<th>Device</th>
<th>Dependency</th>
<th>Location</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>Arm® CPU</p>
</blockquote></td>
<td><blockquote>
<p>—</p>
</blockquote></td>
<td><blockquote>
<p>—</p>
</blockquote></td>
</tr>
</tbody>
</table>
```

Linux x86\_64

```html
<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 26%" />
<col style="width: 57%" />
</colgroup>
<thead>
<tr class="header">
<th>Device</th>
<th>Dependency</th>
<th>Location</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>CPU</p>
</blockquote></td>
<td><blockquote>
<p>—</p>
</blockquote></td>
<td><blockquote>
<p>—</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>GPU</p>
</blockquote></td>
<td><div class="line-block">libOpenCL.so<br />
cache.json</div></td>
<td><div class="line-block"><code>/usr/lib/x86_64-linux-gnu/libOpenCL.so.1</code><br />
<code>./runtime/lib/intel64/cache.json</code></div></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>NPU</p>
</blockquote></td>
<td><blockquote>
<p>—</p>
</blockquote></td>
<td><blockquote>
<p>—</p>
</blockquote></td>
</tr>
</tbody>
</table>
```

macOS arm64

```html
<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 26%" />
<col style="width: 57%" />
</colgroup>
<thead>
<tr class="header">
<th>Device</th>
<th>Dependency</th>
<th>Location</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>Arm® CPU</p>
</blockquote></td>
<td><blockquote>
<p>—</p>
</blockquote></td>
<td><blockquote>
<p>—</p>
</blockquote></td>
</tr>
</tbody>
</table>
```

macOS x86\_64

```html
<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 26%" />
<col style="width: 57%" />
</colgroup>
<thead>
<tr class="header">
<th>Device</th>
<th>Dependency</th>
<th>Location</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>CPU</p>
</blockquote></td>
<td><blockquote>
<p>—</p>
</blockquote></td>
<td><blockquote>
<p>—</p>
</blockquote></td>
</tr>
</tbody>
</table>
```

### Libraries for Execution Modes

The `HETERO`, `BATCH`, and `AUTO` execution modes can also be used by the application explicitly or implicitly.
Use the following recommendation scheme to decide whether to add the appropriate libraries to the distribution package:

  - If \[AUTO\](../running-inference/inference-devices-and-modes/auto-device-selection.md) is used
    explicitly in the application or `ov::Core::compile_model` is used without specifying a device, put `openvino_auto_plugin` to the distribution.
    
    Note
    
    Automatic Device Selection relies on \[inference device plugins\](../running-inference/inference-devices-and-modes.md).
    If you are not sure which inference devices are available on the target system, put all inference plugin libraries in the distribution.
    If ov::device::priorities is used for AUTO to specify a limited device list, grab the corresponding device plugins only.

  - If \[HETERO\](../running-inference/inference-devices-and-modes/hetero-execution.md) is either
    used explicitly or `ov::hint::performance_mode` is used with GPU, put `openvino_hetero_plugin` in the distribution.

  - If \[BATCH\](../running-inference/inference-devices-and-modes/automatic-batching.md) is either
    used explicitly or `ov::hint::performance_mode` is used with GPU, put `openvino_batch_plugin` in the distribution.

### Frontend Libraries for Reading Models

OpenVINO Runtime uses frontend libraries dynamically to read models in different formats:

  - `openvino_ir_frontend` is used to read OpenVINO IR.
  - `openvino_tensorflow_frontend` is used to read the TensorFlow file format.
  - `openvino_tensorflow_lite_frontend` is used to read the TensorFlow Lite file format.
  - `openvino_onnx_frontend` is used to read the ONNX file format.
  - `openvino_paddle_frontend` is used to read the Paddle file format.
  - `openvino_pytorch_frontend` is used to convert PyTorch model via `openvino.convert_model` API.

Depending on the model format types that are used in the application in `ov::Core::read_model`, select the appropriate libraries.

Note

To optimize the size of the final distribution package, it is recommended to convert models
to OpenVINO IR by using \[model conversion API\](../model-preparation.md). This way you
do not have to keep TensorFlow, TensorFlow Lite, ONNX, PaddlePaddle, and other frontend
libraries in the distribution package.

## Examples

CPU + OpenVINO IR in C application

In this example, the application is written in C, performs inference on CPU, and reads models stored in the OpenVINO IR format.

The following libraries are used: `openvino_c`, `openvino`, `openvino_intel_cpu_plugin`, and `openvino_ir_frontend`.

  - The `openvino_c` library is a main dependency of the application. The app links against this library.
  - The `openvino` library is used as a private dependency for `openvino_c` and is also used in the deployment.
  - `openvino_intel_cpu_plugin` is used for inference.
  - `openvino_ir_frontend` is used to read source models.

Auto-Device Selection between GPU and CPU

In this example, the application is written in C++, performs inference
with the \[Automatic Device Selection\](../running-inference/inference-devices-and-modes/auto-device-selection.md)
mode, limiting device list to GPU and CPU, and reads models
\[created using C++ code\](../running-inference/model-representation.md).

The following libraries are used: `openvino`, `openvino_auto_plugin`, `openvino_intel_gpu_plugin`, and `openvino_intel_cpu_plugin`.

  - The `openvino` library is a main dependency of the application. The app links against this library.
  - `openvino_auto_plugin` is used to enable Automatic Device Selection.
  - `openvino_intel_gpu_plugin` and `openvino_intel_cpu_plugin` are used for inference. AUTO
    selects between CPU and GPU devices according to their physical existence on the deployed machine.
  - No frontend library is needed because `ov::Model` is created in code.
