---
sidebar_label: 'OpenVINO™ Inference Request'
format: md
---

# OpenVINO™ Inference Request

> models on different devices in asynchronous or synchronous
> modes of inference.

To set up and run inference, use the `ov::InferRequest` class. It enables you to run
inference on different devices either synchronously or asynchronously. It also includes
methods to retrieve data or adjust data from model inputs and outputs.

The `ov::InferRequest` can be created from the `ov::CompiledModel`.

Python

docs/articles\_en/assets/snippets/ov\_infer\_request.py

C++

docs/articles\_en/assets/snippets/ov\_infer\_request.cpp

## Synchronous / asynchronous inference

The synchronous mode is the basic mode of inference and means that inference stages block
the application execution, as one waits for the other to finish. Use `ov::InferRequest::infer`
to execute in this mode.

Python

docs/articles\_en/assets/snippets/ov\_infer\_request.py

C++

docs/articles\_en/assets/snippets/ov\_infer\_request.cpp

The asynchronous mode may improve application performance, as it enables the app to operate
before inference finishes, with the accelerator still running inference. Use
`ov::InferRequest::start_async` to execute in this mode.

Python

docs/articles\_en/assets/snippets/ov\_infer\_request.py

C++

docs/articles\_en/assets/snippets/ov\_infer\_request.cpp

The asynchronous mode supports two ways the application waits for inference results.
Both are thread-safe.

  - `ov::InferRequest::wait_for` - the method is blocked until the specified time has passed
    or the result becomes available, whichever comes first.
    
    Python
    
    docs/articles\_en/assets/snippets/ov\_infer\_request.py
    
    C++
    
    docs/articles\_en/assets/snippets/ov\_infer\_request.cpp

  - `ov::InferRequest::wait` - waits until inference results become available.
    
    Python
    
    docs/articles\_en/assets/snippets/ov\_infer\_request.py
    
    C++
    
    docs/articles\_en/assets/snippets/ov\_infer\_request.cpp
    
    Keep in mind that the completion order cannot be guaranteed when processing inference
    requests simultaneously, possibly complicating the application logic. Therefore, for
    multi-request scenarios, consider also the `ov::InferRequest::set_callback` method, to
    trigger a callback when the request is complete. Note that to avoid cyclic references
    in the callback, weak reference of infer\_request should be used (`ov::InferRequest*`,
    `ov::InferRequest&, std::weal_ptr<ov::InferRequest>`, etc.).
    
    Python
    
    docs/articles\_en/assets/snippets/ov\_infer\_request.py
    
    C++
    
    docs/articles\_en/assets/snippets/ov\_infer\_request.cpp
    
    If you want to abort a running inference request, use the `ov::InferRequest::cancel`
    method.
    
    Python
    
    docs/articles\_en/assets/snippets/ov\_infer\_request.py
    
    C++
    
    docs/articles\_en/assets/snippets/ov\_infer\_request.cpp

For more information, see the
\[Classification Async Sample\](../../get-started/learn-openvino/openvino-samples/image-classification-async.md),
as well as the articles on
\[synchronous\](../../documentation/openvino-extensibility/openvino-plugin-library/synch-inference-request.md)
and
\[asynchronous\](../../documentation/openvino-extensibility/openvino-plugin-library/asynch-inference-request.md)
inference requests.

## Working with Input and Output tensors

`ov::InferRequest` enables you to get input/output tensors by tensor name, index, and port.
Note that a similar logic is applied to retrieving data using the `ov::Model` methods.

`get_input_tensor`, `set_input_tensor`, `get_output_tensor`, `set_output_tensor`

m-4

  - for a model with only one input/output, no arguments are required
    
    Python
    
    docs/articles\_en/assets/snippets/ov\_infer\_request.py
    
    C++
    
    docs/articles\_en/assets/snippets/ov\_infer\_request.cpp

  - to select a specific input/output tensor provide its index number as a parameter
    
    Python
    
    docs/articles\_en/assets/snippets/ov\_infer\_request.py
    
    C++
    
    docs/articles\_en/assets/snippets/ov\_infer\_request.cpp

`ov::InferRequest::get_tensor`, `ov::InferRequest::set_tensor`

m-4

  - to select an input/output tensor by tensor name, provide it as a parameter
    
    Python
    
    docs/articles\_en/assets/snippets/ov\_infer\_request.py
    
    C++
    
    docs/articles\_en/assets/snippets/ov\_infer\_request.cpp

  - to select an input/output tensor by port
    
    Python
    
    docs/articles\_en/assets/snippets/ov\_infer\_request.py
    
    C++
    
    docs/articles\_en/assets/snippets/ov\_infer\_request.cpp

## Infer Request Use Scenarios

### Cascade of Models

`ov::InferRequest` can be used to organize a cascade of models. Infer Requests are required
for each model. In this case, you can get the output tensor from the first request, using
`ov::InferRequest::get_tensor` and set it as input for the second request, using
`ov::InferRequest::set_tensor`. Keep in mind that tensors shared across compiled models can
be rewritten by the first model if the first infer request is run once again, while the
second model has not started yet.

Python

docs/articles\_en/assets/snippets/ov\_infer\_request.py

C++

docs/articles\_en/assets/snippets/ov\_infer\_request.cpp

### Re-use shared input in several models (e.g. ROI Tensors)

If a model processes data created by a different model in the same pipeline, you may be
able to reuse the input, instead of allocating two separate input tensors. Just allocate
memory for the first model input, and then reuse it for the second model, adjusting it
if necessary. A good example is, when the first model detects objects in a video frame
(stored as an input tensor), and the second model uses the generated Region of Interest
(ROI) to perform additional operations. In this case, the second model may take the
pre-allocated input and crop the frame to the size of the generated bounding boxes.
In this case, use `ov::Tensor` with `ov::Tensor` and `ov::Coordinate` as parameters.

Python

docs/articles\_en/assets/snippets/ov\_infer\_request.py

C++

docs/articles\_en/assets/snippets/ov\_infer\_request.cpp

### Using Remote Tensors

By using `ov::RemoteContext` you can create a remote tensor to work with remote device memory.

Python

docs/articles\_en/assets/snippets/ov\_infer\_request.py

C++

docs/articles\_en/assets/snippets/ov\_infer\_request.cpp
