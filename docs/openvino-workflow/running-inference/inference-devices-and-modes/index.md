---
sidebar_label: 'Inference Devices and Modes'
format: md
---

# Inference Devices and Modes

The OpenVINO™ Runtime offers several inference modes to optimize hardware usage.
You can run inference on a single device or use automated modes that manage multiple devices:

**single-device inference**  
   This mode runs all inference on one selected device. The OpenVINO Runtime includes built-in plugins that support the following devices:  
   \[CPU\](inference-devices-and-modes/cpu-device.md)  
   \[GPU\](inference-devices-and-modes/gpu-device.md)  
   \[NPU\](inference-devices-and-modes/npu-device.md)

**automated inference modes**  
   These modes automate device selection and workload distribution, potentially increasing performance and portability:  
   \[Automatic Device Selection (AUTO)\](inference-devices-and-modes/auto-device-selection.md)  
   \[Heterogeneous Execution (HETERO)\](inference-devices-and-modes/hetero-execution.md) across different device types  
   \[Automatic Batching Execution (Auto-batching)\](inference-devices-and-modes/automatic-batching.md): automatically groups inference requests to improve throughput

Learn how to configure devices in the \[Query device properties\](inference-devices-and-modes/query-device-properties.md) article.

## Enumerating Available Devices

The OpenVINO Runtime API provides methods to list available devices and their details.
When there are multiple instances of a device, they get specific names like GPU.0 for iGPU.
Here is an example of the output with device names, including two GPUs:

``` sh
./hello_query_device
Available devices:
    Device: CPU
...
    Device: GPU.0
...
    Device: GPU.1
```

See the \[Hello Query Device Sample\](../../get-started/learn-openvino/openvino-samples/hello-query-device.md)
for more details.

Below is an example showing how to list available devices and use them with multi-device mode:

C++

docs/articles\_en/assets/snippets/MULTI2.cpp

If you have two GPU devices, you can specify them explicitly as “MULTI:GPU.1,GPU.0”.
Here is how to list and use all available GPU devices:

C++

docs/articles\_en/assets/snippets/MULTI3.cpp

## Additional Resources

  - [OpenVINO™ Runtime API Tutorial](https://github.com/openvinotoolkit/openvino_notebooks/tree/latest/notebooks/openvino-api)
  - [AUTO Device Tutorial](https://github.com/openvinotoolkit/openvino_notebooks/tree/latest/notebooks/auto-device)
  - [GPU Device Tutorial](https://github.com/openvinotoolkit/openvino_notebooks/tree/latest/notebooks/gpu-device)
  - [NPU Device Tutorial](https://github.com/openvinotoolkit/openvino_notebooks/tree/latest/notebooks/hello-npu)
