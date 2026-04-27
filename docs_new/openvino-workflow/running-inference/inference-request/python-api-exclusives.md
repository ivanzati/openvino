---
sidebar_label: 'OpenVINO™ Python API Exclusives'
format: md
---

# OpenVINO™ Python API Exclusives

> improve user experience and provide simple yet powerful tool
> for Python users.

OpenVINO™ Runtime Python API offers additional features and helpers to enhance user experience. The main goal of Python API is to provide user-friendly and simple yet powerful tool for Python users.

## Easier Model Compilation

`CompiledModel` can be easily created with the helper method. It hides the creation of `Core` and applies `AUTO` inference mode by default.

docs/articles\_en/assets/snippets/ov\_python\_exclusives.py

## Model/CompiledModel Inputs and Outputs

Besides functions aligned to C++ API, some of them have their Python counterparts or extensions. For example, `Model` and `CompiledModel` inputs/outputs can be accessed via properties.

docs/articles\_en/assets/snippets/ov\_python\_exclusives.py

Refer to [Python API documentation](../../../api/ie_python_api/api.html),
where helper functions or properties are available for different classes.

## Working with Tensor

Python API allows passing data as tensors. The `Tensor` object holds a copy of the data from the given array. The `dtype` of *numpy* arrays is converted to OpenVINO™ types automatically.

docs/articles\_en/assets/snippets/ov\_python\_exclusives.py

### Shared Memory Mode

`Tensor` objects can share the memory with *numpy* arrays. By specifying the `shared_memory` argument, the `Tensor` object does not copy data. Instead, it has access to the memory of the *numpy* array.

docs/articles\_en/assets/snippets/ov\_python\_exclusives.py

## Running Inference

Python API supports extra calling methods to synchronous and asynchronous modes for inference.

All infer methods allow users to pass data as popular *numpy* arrays, gathered in either Python dicts or lists.

docs/articles\_en/assets/snippets/ov\_python\_exclusives.py

Results from inference can be obtained in various ways:

docs/articles\_en/assets/snippets/ov\_python\_exclusives.py

### Synchronous Mode - Extended

Python API provides different synchronous calls to infer model, which block the application execution. Additionally, these calls return results of inference:

docs/articles\_en/assets/snippets/ov\_python\_exclusives.py

### Inference Results - OVDict

Synchronous calls return a special data structure called `OVDict`. It can be compared to a "frozen dictionary". There are various ways of accessing the object's elements:

docs/articles\_en/assets/snippets/ov\_python\_exclusives.py

Note

It is possible to convert `OVDict` to a native dictionary using the `to_dict()` method.

Warning

Using `to_dict()` results in losing access via strings and integers. Additionally,
it performs a shallow copy, thus any modifications may affect the original
object as well.

### AsyncInferQueue

Asynchronous mode pipelines can be supported with a wrapper class called `AsyncInferQueue`. This class automatically spawns the pool of `InferRequest` objects (also called "jobs") and provides synchronization mechanisms to control the flow of the pipeline.

Each job is distinguishable by a unique `id`, which is in the range from 0 up to the number of jobs specified in the `AsyncInferQueue` constructor.

The `start_async` function call is not required to be synchronized - it waits for any available job if the queue is busy/overloaded. Every `AsyncInferQueue` code block should end with the `wait_all` function which provides the "global" synchronization of all jobs in the pool and ensure that access to them is safe.

docs/articles\_en/assets/snippets/ov\_python\_exclusives.py

Warning

`InferRequest` objects that can be acquired by iterating over a `AsyncInferQueue` object or by `[id]` guaranteed to work with read-only methods like getting tensors.
Any mutating methods (e.g. start\_async, set\_callback) of a single request will put the parent AsyncInferQueue object in an invalid state.

#### Acquiring Results from Requests

After the call to `wait_all`, jobs and their data can be safely accessed. Acquiring a specific job with `[id]` will return the `InferRequest` object, which will result in seamless retrieval of the output data.

docs/articles\_en/assets/snippets/ov\_python\_exclusives.py

#### Setting Callbacks

Another feature of `AsyncInferQueue` is the ability to set callbacks. When callback is set, any job that ends inference calls upon the Python function. The callback function must have two arguments: one is the request that calls the callback, which provides the `InferRequest` API; the other is called "userdata", which provides the possibility of passing runtime values. Those values can be of any Python type and later used within the callback function.

The callback of `AsyncInferQueue` is uniform for every job. When executed, GIL is acquired to ensure safety of data manipulation inside the function.

docs/articles\_en/assets/snippets/ov\_python\_exclusives.py

### Working with u1, u4 and i4 Element Types

Since OpenVINO™ supports low precision element types, there are a few ways to handle them in Python.
To create an input tensor with such element types, you may need to pack your data in the new *numpy* array, with which the byte size matches the original input size:

docs/articles\_en/assets/snippets/ov\_python\_exclusives.py

To extract low precision values from a tensor into the *numpy* array, you can use the following helper:

docs/articles\_en/assets/snippets/ov\_python\_exclusives.py

### Release of GIL

Some functions in Python API release the Global Lock Interpreter (GIL) while running work-intensive code. This can help you achieve more parallelism in your application, using Python threads. For more information about GIL, refer to the [Python API documentation](../../../api/ie_python_api/api.html).

docs/articles\_en/assets/snippets/ov\_python\_exclusives.py

Note

While GIL is released, functions can still modify and/or operate on Python objects in C++. Hence, there is no reference counting. You should pay attention to thread safety in case sharing of these objects with another thread occurs. It might affect code only if multiple threads are spawned in Python.

#### List of Functions that Release the GIL

  - openvino.AsyncInferQueue.start\_async
  - openvino.AsyncInferQueue.is\_ready
  - openvino.AsyncInferQueue.wait\_all
  - openvino.AsyncInferQueue.get\_idle\_request\_id
  - openvino.CompiledModel.create\_infer\_request
  - openvino.CompiledModel.infer\_new\_request
  - openvino.CompiledModel.\_\_call\_\_
  - openvino.CompiledModel.export
  - openvino.CompiledModel.get\_runtime\_model
  - openvino.Core.compile\_model
  - openvino.Core.read\_model
  - openvino.Core.import\_model
  - openvino.Core.query\_model
  - openvino.Core.get\_available\_devices
  - openvino.InferRequest.infer
  - openvino.InferRequest.start\_async
  - openvino.InferRequest.wait
  - openvino.InferRequest.wait\_for
  - openvino.InferRequest.get\_profiling\_info
  - openvino.InferRequest.query\_state
  - openvino.Model.reshape
  - openvino.preprocess.PrePostProcessor.build
