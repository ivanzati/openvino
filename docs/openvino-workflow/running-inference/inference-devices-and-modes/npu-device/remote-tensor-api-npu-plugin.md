---
sidebar_label: 'Remote Tensor API of NPU Plugin'
format: md
---

# Remote Tensor API of NPU Plugin

> interoperability with existing native APIs, such as
> NT handle, or DMA-BUF System Heap, and provides mechanisms
> for mapping files into memory for efficient data access.

The NPU plugin supports memory sharing between OpenVINO and native APIs such as OpenCL, Vulkan, or DirectX 12.
It implements the `ov::RemoteContext` and `ov::RemoteTensor` interfaces, providing mechanisms for efficient memory sharing.
On Windows, the plugin exports an NT handle; on Linux, it uses a DMA-BUF System Heap. You can share this memory by
passing the pointer as the `shared_buffer` member to the `remote_tensor(..., shared_buffer)` create function.
Another option is to import memory by mapping a file into memory or by using a CPU virtual address allocation. These methods
help avoid memory copy overhead when plugging OpenVINO inference into an existing NPU pipeline.

Supported scenario by the Remote Tensor API:

  - The NPU plugin context and memory objects can be constructed from low-level device, display, or memory handles and used to create the OpenVINO™ `ov::CompiledModel` or `ov::Tensor` objects.

Class and function declarations for the API are defined in the following file: `src/inference/include/openvino/runtime/intel_npu/level_zero/level_zero.hpp`

The most common way to enable the interaction of your application with the Remote Tensor API is to use user-side utility classes
and functions that consume or produce native handles directly.

## Context Sharing Between Application and NPU Plugin

NPU plugin classes that implement the `ov::RemoteContext` interface are responsible for context sharing.
Obtaining a context object is the first step in sharing pipeline objects.
The context object of the NPU plugin directly wraps Level Zero context, setting a scope for sharing the
`ov::RemoteTensor` objects. The `ov::RemoteContext` object is retrieved from the NPU plugin.

Once you have obtained the context, you can use it to create the `ov::RemoteTensor` objects.

### Getting RemoteContext from the Plugin

To request the current default context of the plugin, use one of the following methods:

Get context from Core

docs/articles\_en/assets/snippets/npu\_remote\_objects\_creation.cpp

Get context from compiled model

docs/articles\_en/assets/snippets/npu\_remote\_objects\_creation.cpp

## Memory Sharing Between Application and NPU Plugin

The classes that implement the `ov::RemoteTensor` interface are the wrappers for native API
memory handles, which can be obtained from them at any time.

To create a shared tensor from a native memory handle or a file, use dedicated `create_tensor`, `create_l0_host_tensor`, or `create_host_tensor`
methods of the `ov::RemoteContext` sub-classes.
`ov::intel_npu::level_zero::LevelZero` has multiple overloads methods which enable wrapping pre-allocated native handles with the `ov::RemoteTensor`
object or requesting plugin to allocate specific device memory.
For more details, see the code snippets below:

Native Handle and File Mapping

.. tab-item:: File Mapping

docs/articles\_en/assets/snippets/npu\_remote\_objects\_creation.cpp

Import CPU virtual address allocation

  - sync  
    import-cpu-va

docs/articles\_en/assets/snippets/npu\_remote\_objects\_creation.cpp

NT handle

  - sync  
    nt-handle

docs/articles\_en/assets/snippets/npu\_remote\_objects\_creation.cpp

DMA-BUF System Heap file descriptor

  - sync  
    dma-buf

docs/articles\_en/assets/snippets/npu\_remote\_objects\_creation.cpp

Allocate device memory

Remote Tensor - Level Zero host memory

docs/articles\_en/assets/snippets/npu\_remote\_objects\_creation.cpp

Tensor - Level Zero host memory

docs/articles\_en/assets/snippets/npu\_remote\_objects\_creation.cpp

### Limitations

The NPU plugin does not support methods for direct allocation of native handles.

Warning

**CPU Virtual Address Allocation Requirements**
When using CPU virtual address allocations, you **must** comply with the following requirements to prevent memory corruption and crashes:

**1. Memory Alignment (Mandatory)**
Both the allocation pointer and its size must be aligned to the standard page size (4KB). Non-aligned allocations will be rejected.

**2. Allocation Lifetime (Critical)**
The allocation must remain valid **until ALL** of the following have occurred:
\* All inference requests using this remote tensor have completed execution, **AND**
\* All inference requests using this remote tensor have been destroyed, **AND**
\* The remote tensor has been destroyed

Failure to maintain the allocation for the entire lifecycle will result in undefined behavior and potential crashes.

## Low-Level Methods for RemoteContext and RemoteTensor Creation

The high-level wrappers mentioned above bring a direct dependency on native APIs to your program.
If you want to avoid the dependency, you still can directly use the `ov::Core::create_context()`,
`ov::RemoteContext::create_tensor()`, and `ov::RemoteContext::get_params()` methods.
On this level, native handles are re-interpreted as void pointers and all arguments are passed
using `ov::AnyMap` containers that are filled with the `std::string, ov::Any` pairs.
Two types of map entries are possible: a descriptor and a container.
The descriptor sets the expected structure and possible parameter values of the map.

For possible low-level properties and their description, refer to the header file:
[remote\_properties.hpp](https://github.com/openvinotoolkit/openvino/blob/master/src/inference/include/openvino/runtime/intel_npu/remote_properties.hpp).

## Additional Resources

  - [ov::Core](https://docs.openvino.ai/2026/api/c_cpp_api/classov_1_1_core.html)
  - [ov::RemoteTensor](https://docs.openvino.ai/2026/api/c_cpp_api/classov_1_1_remote_tensor.html)
