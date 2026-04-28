---
sidebar_label: 'Remote Tensor API of GPU Plugin'
format: md
---

# Remote Tensor API of GPU Plugin

> interoperability with existing native APIs, such as OpenCL,
> Microsoft DirectX, or VAAPI.

The GPU plugin implementation of the `ov::RemoteContext` and `ov::RemoteTensor` interfaces supports GPU
pipeline developers who need video memory sharing and interoperability with existing native APIs,
such as OpenCL, Microsoft DirectX, or VAAPI.

The `ov::RemoteContext` and `ov::RemoteTensor` interface implementation targets the need for memory sharing and
interoperability with existing native APIs, such as OpenCL, Microsoft DirectX, and VAAPI.
They allow you to avoid any memory copy overhead when plugging OpenVINO™ inference
into an existing GPU pipeline. They also enable OpenCL kernels to participate in the pipeline to become
native buffer consumers or producers of the OpenVINO™ inference.

There are two interoperability scenarios supported by the Remote Tensor API:

  - The GPU plugin context and memory objects can be constructed from low-level device, display, or memory handles and used to create the OpenVINO™ `ov::CompiledModel` or `ov::Tensor` objects.
  - The OpenCL context or buffer handles can be obtained from existing GPU plugin objects, and used in OpenCL processing on the application side.

Class and function declarations for the API are defined in the following files:

  - Windows -- `openvino/runtime/intel_gpu/ocl/ocl.hpp` and `openvino/runtime/intel_gpu/ocl/dx.hpp`
  - Linux -- `openvino/runtime/intel_gpu/ocl/ocl.hpp` and `openvino/runtime/intel_gpu/ocl/va.hpp`

The most common way to enable the interaction of your application with the Remote Tensor API is to use user-side utility classes
and functions that consume or produce native handles directly.

## Context Sharing Between Application and GPU Plugin

GPU plugin classes that implement the `ov::RemoteContext` interface are responsible for context sharing.
Obtaining a context object is the first step in sharing pipeline objects.
The context object of the GPU plugin directly wraps OpenCL context, setting a scope for sharing the
`ov::CompiledModel` and `ov::RemoteTensor` objects. The `ov::RemoteContext` object can be either created on top of
an existing handle from a native API or retrieved from the GPU plugin.

Once you have obtained the context, you can use it to compile a new `ov::CompiledModel` or create `ov::RemoteTensor`
objects. For network compilation, use a dedicated flavor of `ov::Core::compile_model()`, which accepts the context as an additional parameter.

### Creation of RemoteContext from Native Handle

To create the `ov::RemoteContext` object for user context, explicitly provide the context to the plugin using constructor for one
of `ov::RemoteContext` derived classes.

Windows/C++

Create from cl\_context

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation.cpp

Create from cl\_queue

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation.cpp

Create from ID3D11Device

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation.cpp

Windows/C

Create from cl\_context

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation\_c.cpp

Create from cl\_queue

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation\_c.cpp

Create from ID3D11Device

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation\_c.cpp

Linux/C++

Create from cl\_context

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation.cpp

Create from cl\_queue

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation.cpp

Create from VADisplay

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation.cpp

Linux/C

Create from cl\_context

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation\_c.cpp

Create from cl\_queue

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation\_c.cpp

Create from VADisplay

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation\_c.cpp

### Getting RemoteContext from the Plugin

If you do not provide any user context, the plugin uses its default internal context.
The plugin attempts to use the same internal context object as long as plugin options are kept the same.
Therefore, all `ov::CompiledModel` objects created during this time share the same context.
Once the plugin options have been changed, the internal context is replaced by the new one.

To request the current default context of the plugin, use one of the following methods:

C++

Get context from Core

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation.cpp

Get context from compiled model

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation.cpp

C

Get context from Core

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation\_c.cpp

Get context from compiled model

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation\_c.cpp

## Memory Sharing Between Application and GPU Plugin

The classes that implement the `ov::RemoteTensor` interface are the wrappers for native API
memory handles (which can be obtained from them at any time).

To create a shared tensor from a native memory handle, use dedicated `create_tensor` or `create_tensor_nv12` methods
of the `ov::RemoteContext` sub-classes.
`ov::intel_gpu::ocl::ClContext` has multiple overloads of `create_tensor` methods which allow to wrap pre-allocated native handles with the `ov::RemoteTensor`
object or request plugin to allocate specific device memory. There also provides C APIs to do the same things with C++ APIs.
For more details, see the code snippets below:

Wrap native handles/C++

USM pointer

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation.cpp

cl\_mem

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation.cpp

cl::Buffer

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation.cpp

cl::Image2D

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation.cpp

biplanar NV12 surface

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation.cpp

Allocate device memory/C++

USM host memory

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation.cpp

USM device memory

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation.cpp

cl::Buffer

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation.cpp

Wrap native handles/C

USM pointer

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation\_c.cpp

cl\_mem

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation\_c.cpp

cl::Buffer

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation\_c.cpp

cl::Image2D

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation\_c.cpp

biplanar NV12 surface

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation\_c.cpp

Allocate device memory/C

USM host memory

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation\_c.cpp

USM device memory

docs/articles\_en/assets/snippets/gpu/remote\_objects\_creation\_c.cpp

The `ov::intel_gpu::ocl::D3DContext` and `ov::intel_gpu::ocl::VAContext` classes are derived from `ov::intel_gpu::ocl::ClContext`.
Therefore, they provide the functionality described above and extend it
to enable creation of `ov::RemoteTensor` objects from `ID3D11Buffer`, `ID3D11Texture2D`
pointers or the `VASurfaceID` handle, as shown in the examples below:

ID3D11Buffer

``` cpp
// ...

// initialize the core and load the network
ov::Core core;
auto model = core.read_model("model.xml");
auto compiled_model = core.compile_model(model, "GPU");
auto infer_request = compiled_model.create_infer_request();


// obtain the RemoteContext from the compiled model object and cast it to D3DContext
auto gpu_context = compiled_model.get_context().as<ov::intel_gpu::ocl::D3DContext>();

auto input = model->get_parameters().at(0);
ID3D11Buffer* d3d_handle = get_d3d_buffer();
auto tensor = gpu_context.create_tensor(input->get_element_type(), input->get_shape(), d3d_handle);
infer_request.set_tensor(input, tensor);
```

ID3D11Texture2D

``` cpp
using namespace ov::preprocess;
auto p = PrePostProcessor(model);
p.input().tensor().set_element_type(ov::element::u8)
                  .set_color_format(ov::preprocess::ColorFormat::NV12_TWO_PLANES, {"y", "uv"})
                  .set_memory_type(ov::intel_gpu::memory_type::surface);
p.input().preprocess().convert_color(ov::preprocess::ColorFormat::BGR);
p.input().model().set_layout("NCHW");
model = p.build();

CComPtr<ID3D11Device> device_ptr = get_d3d_device_ptr()
// create the shared context object
auto shared_d3d_context = ov::intel_gpu::ocl::D3DContext(core, device_ptr);
// compile model within a shared context
auto compiled_model = core.compile_model(model, shared_d3d_context);

auto param_input_y = model->get_parameters().at(0);
auto param_input_uv = model->get_parameters().at(1);

D3D11_TEXTURE2D_DESC texture_description = get_texture_desc();
CComPtr<ID3D11Texture2D> dx11_texture = get_texture();
//     ...
//wrap decoder output into RemoteBlobs and set it as inference input
auto nv12_blob = shared_d3d_context.create_tensor_nv12(texture_description.Heights, texture_description.Width, dx11_texture);

auto infer_request = compiled_model.create_infer_request();
infer_request.set_tensor(param_input_y->get_friendly_name(), nv12_blob.first);
infer_request.set_tensor(param_input_uv->get_friendly_name(), nv12_blob.second);
infer_request.start_async();
infer_request.wait();
```

VASurfaceID

``` cpp
using namespace ov::preprocess;
auto p = PrePostProcessor(model);
p.input().tensor().set_element_type(ov::element::u8)
                  .set_color_format(ov::preprocess::ColorFormat::NV12_TWO_PLANES, {"y", "uv"})
                  .set_memory_type(ov::intel_gpu::memory_type::surface);
p.input().preprocess().convert_color(ov::preprocess::ColorFormat::BGR);
p.input().model().set_layout("NCHW");
model = p.build();

CComPtr<ID3D11Device> device_ptr = get_d3d_device_ptr()
// create the shared context object
auto shared_va_context = ov::intel_gpu::ocl::VAContext(core, device_ptr);
// compile model within a shared context
auto compiled_model = core.compile_model(model, shared_va_context);

auto param_input_y = model->get_parameters().at(0);
auto param_input_uv = model->get_parameters().at(1);

auto shape = param_input_y->get_shape();
auto width = shape[1];
auto height = shape[2];

VASurfaceID va_surface = decode_va_surface();
//     ...
//wrap decoder output into RemoteBlobs and set it as inference input
auto nv12_blob = shared_va_context.create_tensor_nv12(height, width, va_surface);

auto infer_request = compiled_model.create_infer_request();
infer_request.set_tensor(param_input_y->get_friendly_name(), nv12_blob.first);
infer_request.set_tensor(param_input_uv->get_friendly_name(), nv12_blob.second);
infer_request.start_async();
infer_request.wait();
```

Important

Currently, only sharing of D3D11 surfaces is supported via the
[cl\_intel\_d3d11\_nv12\_media\_sharing](https://github.com/KhronosGroup/OpenCL-Registry/blob/main/extensions/intel/cl_intel_d3d11_nv12_media_sharing.txt)
extension, which provides interoperability between OpenCL and DirectX.

## Direct NV12 Video Surface Input

To support the direct consumption of a hardware video decoder output, the GPU plugin accepts:

  - Two-plane NV12 video surface input - calling the `create_tensor_nv12()` function creates
    a pair of `ov::RemoteTensor` objects, representing the Y and UV planes.
  - Single-plane NV12 video surface input - calling the `create_tensor()` function creates one
    `ov::RemoteTensor` object, representing the Y and UV planes at once (Y elements before UV elements).
  - NV12 to Grey video surface input conversion - calling the `create_tensor()` function creates one
    `ov::RemoteTensor` object, representing only the Y plane.

To ensure that the plugin generates a correct execution graph, static preprocessing
should be added before model compilation:

two-plane

C++

docs/articles\_en/assets/snippets/gpu/preprocessing\_nv12\_two\_planes.cpp

C

docs/articles\_en/assets/snippets/gpu/preprocessing\_nv12\_two\_planes\_c.cpp

single-plane

docs/articles\_en/assets/snippets/gpu/preprocessing\_nv12\_single\_plane.cpp

NV12 to Grey

docs/articles\_en/assets/snippets/gpu/preprocessing\_nv12\_to\_gray.cpp

Since the `ov::intel_gpu::ocl::ClImage2DTensor` and its derived classes do not support batched surfaces,
if batching and surface sharing are required at the same time,
inputs need to be set via the `ov::InferRequest::set_tensors` method with vector of shared surfaces for each plane:

Single Batch

two-plane

C++

docs/articles\_en/assets/snippets/gpu/preprocessing\_nv12\_two\_planes.cpp

C

docs/articles\_en/assets/snippets/gpu/preprocessing\_nv12\_two\_planes\_c.cpp

single-plane

docs/articles\_en/assets/snippets/gpu/preprocessing\_nv12\_single\_plane.cpp

NV12 to Grey

docs/articles\_en/assets/snippets/gpu/preprocessing\_nv12\_to\_gray.cpp

Multiple Batches

two-plane

docs/articles\_en/assets/snippets/gpu/preprocessing\_nv12\_two\_planes.cpp

single-plane

docs/articles\_en/assets/snippets/gpu/preprocessing\_nv12\_single\_plane.cpp

NV12 to Grey

docs/articles\_en/assets/snippets/gpu/preprocessing\_nv12\_to\_gray.cpp

I420 color format can be processed in a similar way

## Context & Queue Sharing

The GPU plugin supports creation of shared context from the `cl_command_queue` handle. In that case,
the `opencl` context handle is extracted from the given queue via OpenCL™ API, and the queue itself is used inside
the plugin for further execution of inference primitives. Sharing the queue changes the behavior of the `ov::InferRequest::start_async()`
method to guarantee that submission of inference primitives into the given queue is finished before
returning control back to the calling thread.

This sharing mechanism allows performing pipeline synchronization on the app side and avoiding blocking the host thread
on waiting for the completion of inference. The pseudo-code may look as follows:

Queue and context sharing example

docs/articles\_en/assets/snippets/gpu/queue\_sharing.cpp

### Limitations

  - Some primitives in the GPU plugin may block the host thread on waiting for the previous primitives before adding its kernels to the command queue. In such cases, the `ov::InferRequest::start_async()` call takes much more time to return control to the calling thread as internally it waits for a partial or full network completion. Examples of operations: Loop, TensorIterator, DetectionOutput, NonMaxSuppression
  - Synchronization of pre/post processing jobs and inference pipeline inside a shared queue is user's responsibility.
  - Throughput mode is not available when queue sharing is used, i.e., only a single stream can be used for each compiled model.

## Low-Level Methods for RemoteContext and RemoteTensor Creation

The high-level wrappers mentioned above bring a direct dependency on native APIs to the user program.
If you want to avoid the dependency, you still can directly use the `ov::Core::create_context()`,
`ov::RemoteContext::create_tensor()`, and `ov::RemoteContext::get_params()` methods.
On this level, native handles are re-interpreted as void pointers and all arguments are passed
using `ov::AnyMap` containers that are filled with `std::string, ov::Any` pairs.
Two types of map entries are possible: descriptor and container.
Descriptor sets the expected structure and possible parameter values of the map.

For possible low-level properties and their description, refer to the header file:
[remote\_properties.hpp](https://github.com/openvinotoolkit/openvino/blob/releases/2026/0/src/inference/include/openvino/runtime/intel_gpu/remote_properties.hpp).

## Examples

To see pseudo-code of usage examples, refer to the sections below.

Note

For low-level parameter usage examples, see the source code of user-side wrappers from the include files mentioned above.

OpenCL Kernel Execution on a Shared Buffer

This example uses the OpenCL context obtained from a compiled model object.

docs/articles\_en/assets/snippets/gpu/context\_sharing.cpp

Running GPU Plugin Inference within User-Supplied Shared Context

docs/articles\_en/assets/snippets/gpu/context\_sharing.cpp

Direct Consuming of the NV12 VAAPI Video Decoder Surface on Linux

C++

docs/articles\_en/assets/snippets/gpu/context\_sharing\_va.cpp

C

docs/articles\_en/assets/snippets/gpu/context\_sharing\_va\_c.cpp

## See Also

  - [ov::Core](https://docs.openvino.ai/2026/api/c_cpp_api/classov_1_1_core.html)
  - [ov::RemoteTensor](https://docs.openvino.ai/2026/api/c_cpp_api/classov_1_1_remote_tensor.html)
