---
sidebar_label: 'Changing Input Shapes'
format: md
---

# Changing Input Shapes

> input has a different size than the model's input shape.

OpenVINO™ enables you to change model input shape during the application runtime.
It may be useful when you want to feed the model an input that has different size than the model input shape.
The following instructions are for cases where you need to change the model input shape repeatedly.

Note

If you need to do this only once, prepare a model with updated shapes via
\[Model Conversion API\](../../model-preparation.md).
For more information, refer to the \[Setting Input Shapes\](../../model-preparation/setting-input-shapes.md) article.

## The reshape method

The reshape method is used as `ov::Model::reshape` in C++ and
[Model.reshape](https://docs.openvino.ai/2026/api/ie_python_api/_autosummary/openvino.Model.html#openvino.Model.reshape)
in Python. The method updates input shapes and propagates them down to the outputs
of the model through all intermediate layers. The code below is an example of how
to set a new batch size with the `reshape` method:

Python

docs/articles\_en/assets/snippets/ShapeInference.py

C++

docs/articles\_en/assets/snippets/ShapeInference.cpp

The diagram below presents the results of using the method, where the size of
model input is changed with an image input:

![image](img/original_vs_reshaped_model.svg)

When using the `reshape` method, you may take one of the approaches:

1.  You can pass a new shape to the method in order to change the input shape of
    the model with a single input. See the example of adjusting spatial dimensions to the input image:
    
    Python
    
    docs/articles\_en/assets/snippets/ShapeInference.py
    
    C++
    
    docs/articles\_en/assets/snippets/ShapeInference.cpp
    
    To do the opposite - to resize input image to match the input shapes of the model,
    use the \[pre-processing API\](../optimize-inference/optimize-preprocessing.md).

2.  You can express a reshape plan, specifying the input by the port, the index, and the tensor name:
    
    Port
    
    Python
    
    `openvino.Output` dictionary key specifies input by passing actual input object.
    Dictionary values representing new shapes could be `PartialShape`:
    
    docs/articles\_en/assets/snippets/ShapeInference.py
    
    C++
    
    `map<ov::Output<ov::Node>, ov::PartialShape` specifies input by passing actual input port:
    
    docs/articles\_en/assets/snippets/ShapeInference.cpp
    
    Index
    
    Python
    
    `int` dictionary key specifies input by its index.
    Dictionary values representing new shapes could be `tuple`:
    
    docs/articles\_en/assets/snippets/ShapeInference.py
    
    C++
    
    `map<size_t, ov::PartialShape>` specifies input by its index:
    
    docs/articles\_en/assets/snippets/ShapeInference.cpp
    
    Tensor Name
    
    Python
    
    `str` dictionary key specifies input by its name.
    Dictionary values representing new shapes could be `str`:
    
    docs/articles\_en/assets/snippets/ShapeInference.py
    
    C++
    
    `map<string, ov::PartialShape>` specifies input by its name:
    
    docs/articles\_en/assets/snippets/ShapeInference.cpp

You can find the usage scenarios of the `reshape` method in
\[Hello Reshape SSD Samples\](../../../get-started/learn-openvino/openvino-samples/hello-reshape-ssd.md).

Note

In some cases, models may not be ready to be reshaped. Therefore, a new input
shape cannot be set neither with \[Model Conversion API\](../../model-preparation.md)
nor the `reshape` method.

## The set\_batch method

The meaning of the model batch may vary depending on the model design.
To change the batch dimension of the model, *set the layout* and call the `set_batch` method.

Python

docs/articles\_en/assets/snippets/ShapeInference.py

C++

docs/articles\_en/assets/snippets/ShapeInference.cpp

The `set_batch` method is a high-level API of the reshape functionality, so all
information about the `reshape` method implications are applicable for `set_batch` too.

Once you set the input shape of the model, call the `compile_model` method to
get a `CompiledModel` object for inference with updated shapes.

There are other approaches to change model input shapes during the stage of
\[IR generation\](../../model-preparation/setting-input-shapes.md) or \[model representation\](../model-representation.md) in OpenVINO Runtime.

Important

Shape-changing functionality could be used to turn dynamic model input into a
static one and vice versa. Always set static shapes when the shape of data is
NOT going to change from one inference to another. Setting static shapes can
avoid memory and runtime overheads for dynamic shapes which may vary depending
on hardware plugin and model used. For more information, refer to the
\[Dynamic Shapes\](dynamic-shapes.md).

## Additional Resources

  - \[Extensibility documentation\](../../../documentation/openvino-extensibility.md) - describes a special mechanism in OpenVINO that allows adding support of shape inference for custom operations.
  - [ov::Model::reshape](https://docs.openvino.ai/2026/api/c_cpp_api/group__ov__model__c__api.html#_CPPv416ov_model_reshapePK10ov_model_tPPKcPK18ov_partial_shape_t6size_t) - in OpenVINO Runtime C++ API
  - [Model.reshape](https://docs.openvino.ai/2026/api/ie_python_api/_autosummary/openvino.Model.html#openvino.Model.reshape) - in OpenVINO Runtime Python API.
  - \[Dynamic Shapes\](dynamic-shapes.md)
  - \[OpenVINO samples\](../../../get-started/learn-openvino/openvino-samples.md)
  - \[Preprocessing API\](../optimize-inference/optimize-preprocessing.md)
