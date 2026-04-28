---
sidebar_label: 'Use Case - Integrate and Save Preprocessing Steps Into IR'
format: md
---

# Use Case - Integrate and Save Preprocessing Steps Into IR

> can be added and then the resulting model can be saved to
> OpenVINO Intermediate Representation.

Previous sections covered the \[preprocessing steps\](../preprocessing-api-details.md)
and the overview of \[Layout API\](../layout-api-overview.md).

For many applications, it is also important to minimize read/load time of a model.
Therefore, performing integration of preprocessing steps every time on application
startup, after `ov::runtime::Core::read_model`, may seem inconvenient. In such cases,
once pre and postprocessing steps have been added, it can be useful to store new execution
model to OpenVINO Intermediate Representation (OpenVINO IR, .xml format).

Most available preprocessing steps can also be performed via command-line options,
using `ovc`. For details on such command-line options, refer to the
*Model Conversion*.

## Code example - Saving Model with Preprocessing to OpenVINO IR

In the following example:

  - Original ONNX model takes one `float32` input with the `\{1, 3, 224, 224\}` shape, the `RGB` channel order, and mean/scale values applied.
  - Application provides `BGR` image buffer with a non-fixed size and input images as batches of two.

Below is the model conversion code that can be applied in the model preparation script for this case:

  - Includes / Imports

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

  - Preprocessing & Saving to the OpenVINO IR code.

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

## Application Code - Load Model to Target Device

Next, the application code can load a saved file and stop preprocessing. In this case, enable
\[model caching\](../../optimizing-latency/model-caching-overview.md) to minimize load
time when the cached model is available.

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

## Additional Resources

  - \[Preprocessing Details\](../preprocessing-api-details.md)
  - \[Layout API overview\](../layout-api-overview.md)
  - \[Model Caching Overview\](../../optimizing-latency/model-caching-overview.md)
  - \[Model Preparation\](../../../../model-preparation.md)
  - The [ov::preprocess::PrePostProcessor](https://docs.openvino.ai/2026/api/c_cpp_api/classov_1_1preprocess_1_1_pre_post_processor.html) C++ class documentation
  - The [ov::pass::Serialize](https://docs.openvino.ai/2026/api/c_cpp_api/classov_1_1pass_1_1_serialize.html) - pass to serialize model to XML/BIN
  - The `ov::set_batch` - update batch dimension for a given model
