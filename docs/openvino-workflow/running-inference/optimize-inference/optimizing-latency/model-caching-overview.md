---
sidebar_label: 'Model Caching Overview'
format: md
---

# Model Caching Overview

> automatically and reusing it can significantly
> reduce duration of model compilation on application startup.

As described in \[Integrate OpenVINO™ with Your Application\](../../../running-inference.md),
a common workflow consists of the following steps:

1.  **Create a Core object**:  
      First step to manage available devices and read model objects

2.  **Read the Intermediate Representation**:  
      Read an Intermediate Representation file into the [ov::Model](https://docs.openvino.ai/2026/api/c_cpp_api/classov_1_1_model.html) object

3.  **Prepare inputs and outputs**:  
      If needed, manipulate precision, memory layout, size or color format

4.  **Set configuration**:  
      Add device-specific loading configurations to the device

5.  **Compile and Load Network to device**:  
      Use the [ov::Core::compile\_model()](https://docs.openvino.ai/2026/api/c_cpp_api/classov_1_1_core.html) method with a specific device

6.  **Set input data**:  
      Specify input tensor

7.  **Execute**:  
      Carry out inference and process results

Step 5 can potentially perform several time-consuming device-specific optimizations and network compilations.
To reduce the resulting delays at application startup, you can use Model Caching. It exports the compiled model
automatically and reuses it to significantly reduce the model compilation time.

Important

Not all devices support import/export of models. They will perform normally but will not
enable the compilation stage speed-up.

## Set configuration options

Use the `device_name` option to specify the inference device.  
Specify `cache_dir` to enable model caching.

Python

docs/articles\_en/assets/snippets/ov\_caching.py

C++

docs/articles\_en/assets/snippets/ov\_caching.cpp

If the specified device supports import/export of models,
a cached blob file: `.cl_cache` (GPU) or `.blob` (CPU) is automatically
created inside the `/path/to/cache/dir` folder.
If the device does not support import/export of models, the cache is not
created and no error is thrown.

Note that the first `compile_model` operation takes slightly more time,
as the cache needs to be created - the compiled blob is saved into a file:

![image](img/caching_enabled.svg)

## Use optimized methods

Applications do not always require an initial customization of inputs and
outputs, as they can call `model = core.read_model(...)`, then `core.compile_model(model, ..)`,
which can be further optimized. Thus, the model can be compiled conveniently in a single call,
skipping the read step:

Python

docs/articles\_en/assets/snippets/ov\_caching.py

C++

docs/articles\_en/assets/snippets/ov\_caching.cpp

The total load time is even shorter, when model caching is enabled and `read_model` is optimized as well.

Python

docs/articles\_en/assets/snippets/ov\_caching.py

C++

docs/articles\_en/assets/snippets/ov\_caching.cpp

![image](img/caching_times.svg)

## Advanced Examples

Enabling model caching has no effect when the specified device does not support
import/export of models. To check in advance if a particular device supports
model caching, use the following code in your application:

Python

docs/articles\_en/assets/snippets/ov\_caching.py

C++

docs/articles\_en/assets/snippets/ov\_caching.cpp

## Set `CacheMode` property to `OPTIMIZE_SIZE` to enable weightless caching

Weightless caching is a feature that allows you to create a cache file which doesn't contain the weights of the model. Instead, the weights are loaded from the original model file. This helps to reduce the size of the cache file.

Python

docs/articles\_en/assets/snippets/ov\_caching.py

C++

docs/articles\_en/assets/snippets/ov\_caching.cpp

Important

Currently, this property is supported only by the GPU Plugin and IR model format.

Important

Some weights which undergo transformations during model compilation may not be eligible for weightless caching. In such cases, the cache file will contain these weights while still using the weightless caching mechanism for the rest. The feature supports some of the common transformations and replicates them after loading the model from the cache.

## Enable cache encryption

If model caching is enabled in the CPU Plugin, set the "cache\_encryption\_callbacks"
config option to encrypt the model while caching it and decrypt it when
loading it from the cache. Currently, this property can be set only in `compile_model`.

Python

docs/articles\_en/assets/snippets/ov\_caching.py

C++

docs/articles\_en/assets/snippets/ov\_caching.cpp

If model caching is enabled in the GPU Plugin, the model topology is encrypted when saved to the cache and decrypted when loaded from the cache if the `CacheMode` property is set to `OPTIMIZE_SIZE`. The weights are encrypted only when `CacheMode` is set to `OPTIMIZE_SPEED`. Weight encryption requires extra disk space equal to the size of the weights and may introduce runtime memory overhead for decryption, depending on the encryption algorithm.

Python

docs/articles\_en/assets/snippets/ov\_caching.py

C++

docs/articles\_en/assets/snippets/ov\_caching.cpp

Important

Currently, encryption is supported only by the CPU and GPU plugins. Enabling this
feature for other HW plugins will not encrypt/decrypt model topology in the
cache and will not affect performance.
