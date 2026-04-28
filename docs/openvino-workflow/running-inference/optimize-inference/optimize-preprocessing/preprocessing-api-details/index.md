---
sidebar_label: 'Preprocessing API - details'
format: md
---

# Preprocessing API - details

The purpose of this article is to present details on preprocessing API, such as its capabilities and post-processing.

## Pre-processing Capabilities

Below is a full list of pre-processing API capabilities:

### Addressing Particular Input/Output

If the model has only one input, then simple `ov::preprocess::PrePostProcessor::input()` will get a reference to pre-processing builder for this input (a tensor, the steps, a model):

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

In general, when a model has multiple inputs/outputs, each one can be addressed by a tensor name.

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

Or by it's index.

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

C++ references:

  - `ov::preprocess::InputTensorInfo`
  - `ov::preprocess::OutputTensorInfo`
  - `ov::preprocess::PrePostProcessor`

### Supported Pre-processing Operations

C++ references:

  - `ov::preprocess::PreProcessSteps`

#### Mean/Scale Normalization

Typical data normalization includes 2 operations for each data item: subtract mean value and divide to standard deviation. This can be done with the following code:

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

In Computer Vision area normalization is usually done separately for R, G, B values. To do this, \[layout with 'C' dimension\](layout-api-overview.md) shall be defined. Example:

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

C++ references:

  - `ov::preprocess::PreProcessSteps::mean()`
  - `ov::preprocess::PreProcessSteps::scale()`

#### Converting Precision

In Computer Vision, the image is represented by an array of unsigned 8-bit integer values (for each color), but the model accepts floating point tensors.

To integrate precision conversion into an execution graph as a pre-processing step:

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

C++ references:

  - `` `ov::preprocess::InputTensorInfo::set_element_type() ``
  - `` `ov::preprocess::PreProcessSteps::convert_element_type() ``

#### Converting layout (transposing)

Transposing of matrices/tensors is a typical operation in Deep Learning - you may have a BMP image 640x480, which is an array of `\{480, 640, 3\}` elements, but Deep Learning model can require input with shape `\{1, 3, 480, 640\}`.

Conversion can be done implicitly, using the \[layout\](layout-api-overview.md) of a user's tensor and the layout of an original model.

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

For a manual transpose of axes without the use of a \[layout\](layout-api-overview.md) in the code:

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

It performs the same transpose. However, the approach where source and destination layout are used can be easier to read and understand.

C++ references:

  - `ov::preprocess::PreProcessSteps::convert_layout()`
  - `ov::preprocess::InputTensorInfo::set_layout()`
  - `ov::preprocess::InputModelInfo::set_layout()`
  - `ov::Layout`

#### Resizing Image

Resizing an image is a typical pre-processing step for computer vision tasks. With pre-processing API, this step can also be integrated into an execution graph and performed on a target device.

To resize the input image, it is needed to define `H` and `W` dimensions of the \[layout\](layout-api-overview.md).

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

When original model has known spatial dimensions (`width`+`height`), target `width`/`height` can be omitted.

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

C++ references:
\* `ov::preprocess::PreProcessSteps::resize()`
\* `ov::preprocess::ResizeAlgorithm`

#### Color Conversion

Typical use case is to reverse color channels from `RGB` to `BGR` and vice versa. To do this, specify source color format in `tensor` section and perform `convert_color` pre-processing operation. In the example below, a `BGR` image needs to be converted to `RGB` as required for the model input.

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

#### Color Conversion - NV12/I420

Pre-processing also supports YUV-family source color formats, i.e. NV12 and I420.
In advanced cases, such YUV images can be split into separate planes, e.g., for NV12 images Y-component may come from one source and UV-component from another one. Concatenating such components in user's application manually is not a perfect solution from performance and device utilization perspectives. However, there is a way to use Pre-processing API. For such cases there are `NV12_TWO_PLANES` and `I420_THREE_PLANES` source color formats, which will split the original `input` into 2 or 3 inputs.

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

In this example, the original `input` is split to `input/y` and `input/uv` inputs. You can fill `input/y` from one source, and `input/uv` from another source. Color conversion to `RGB` will be performed, using these sources. It is more efficient as there will be no additional copies of NV12 buffers.

C++ references:

  - `ov::preprocess::ColorFormat`
  - `ov::preprocess::PreProcessSteps::convert_color`

### Custom Operations

Pre-processing API also allows adding `custom` preprocessing steps into an execution graph. The `custom` function accepts the current `input` node, applies the defined preprocessing operations, and returns a new node.

Note

Custom pre-processing function should only insert node(s) after the input. It is done during model compilation. This function will NOT be called during the execution phase. This may appear to be complicated and require knowledge of \[OpenVINO™ operations\](../../../../documentation/openvino-ir-format/operation-sets/available-opsets.md).

If there is a need to insert additional operations to the execution graph right after the input, like some specific crops and/or resizes - Pre-processing API can be a good choice to implement this.

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

C++ references:

  - `ov::preprocess::PreProcessSteps::custom()`
  - \[Available Operations Sets\](../../../../documentation/openvino-ir-format/operation-sets/available-opsets.md)

## Post-processing

Post-processing steps can be added to model outputs. As for pre-processing, these steps will be also integrated into a graph and executed on a selected device.

Pre-processing uses the following flow: **User tensor** -> **Steps** -> **Model input**.

Post-processing uses the reverse: **Model output** -> **Steps** -> **User tensor**.

Compared to pre-processing, there are not as many operations needed for the post-processing stage. Currently, only the following post-processing operations are supported:

  - Convert a \[layout\](layout-api-overview.md).
  - Convert an element type.
  - Customize operations.

Usage of these operations is similar to pre-processing. See the following example:

Python

docs/articles\_en/assets/snippets/ov\_preprocessing.py

C++

docs/articles\_en/assets/snippets/ov\_preprocessing.cpp

C++ references:

  - `ov::preprocess::PostProcessSteps`
  - `ov::preprocess::OutputModelInfo`
  - `ov::preprocess::OutputTensorInfo`
