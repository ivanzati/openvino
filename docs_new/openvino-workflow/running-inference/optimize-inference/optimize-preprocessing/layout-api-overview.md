---
sidebar_label: 'Layout API Overview'
format: md
---

# Layout API Overview

> dimension of input/ output tensor properly and the input size
> can be resized to fit the model.

The concept of layout helps you (and your application) to understand what each particular dimension of input/output tensor means. For example, if your input has the `\{1, 3, 720, 1280\}` shape and the `NCHW` layout, it is clear that `N(batch) = 1`, `C(channels) = 3`, `H(height) = 720`, and `W(width) = 1280`. Without the layout information, the `\{1, 3, 720, 1280\}` tuple does not give any idea to your application on what these numbers mean and how to resize the input image to fit the expectations of the model.

With the `NCHW` layout, it is easier to understand what the `\{8, 3, 224, 224\}` model shape means. Without the layout, it is just a 4-dimensional tensor.

Below is a list of cases where input/output layout is important:

  - Performing model modification:
      - Applying the \[preprocessing\](../optimize-preprocessing.md) steps, such as subtracting means, dividing by scales, resizing an image, and converting `RGB` \<-> `BGR`.
      - Setting/getting a batch for a model.
  - Doing the same operations as used during the model conversion phase. For more information, refer to the:
      - \[Convert to OpenVINO\](../../../model-preparation/convert-model-to-ir.md)
      - [OpenVINO Model Conversion Tutorial](https://github.com/openvinotoolkit/openvino_notebooks/tree/latest/notebooks/convert-to-openvino)
  - Improving the readability of a model input and output.

## Syntax of Layout

### Short Syntax

The easiest way is to fully specify each dimension with one alphabet letter.

Python

docs/articles\_en/assets/snippets/ov\_layout.py

C++

docs/articles\_en/assets/snippets/ov\_layout.cpp

This assigns `N` to the first dimension, `C` to the second, `H` to the third, and `W` to the fourth.

### Advanced Syntax

The advanced syntax allows assigning a word to a dimension. To do this, wrap a layout with square brackets `[]` and specify each name separated by a comma `,`.

Python

docs/articles\_en/assets/snippets/ov\_layout.py

C++

docs/articles\_en/assets/snippets/ov\_layout.cpp

### Partially Defined Layout

If a certain dimension is not important, its name can be set to `?`.

Python

docs/articles\_en/assets/snippets/ov\_layout.py

C++

docs/articles\_en/assets/snippets/ov\_layout.cpp

### Dynamic Layout

If several dimensions are not important, an ellipsis `...` can be used to specify those dimensions.

Python

docs/articles\_en/assets/snippets/ov\_layout.py

C++

docs/articles\_en/assets/snippets/ov\_layout.cpp

### Predefined Names

A layout has some pre-defined dimension names, widely used in computer vision:

  - `N`/`Batch` - batch size
  - `C`/`Channels` - channels
  - `D`/`Depth` - depth
  - `H`/`Height` - height
  - `W`/`Width` - width

These names are used in \[PreProcessing API\](../optimize-preprocessing.md). There is a set of helper functions to get appropriate dimension index from a layout.

Python

docs/articles\_en/assets/snippets/ov\_layout.py

C++

docs/articles\_en/assets/snippets/ov\_layout.cpp

### Equality

Layout names are case-insensitive, which means that `Layout("NCHW")` = `Layout("nChW")` = `Layout("[N,c,H,w]")`.

### Dump Layout

A layout can be converted to a string in the advanced syntax format. It can be useful for debugging and serialization purposes.

Python

docs/articles\_en/assets/snippets/ov\_layout.py

C++

docs/articles\_en/assets/snippets/ov\_layout.cpp

### Get layout from Model Input/Output

OpenVINO provides helpers which provide a simple interface to get layout from Model input or output.

Python

docs/articles\_en/assets/snippets/ov\_layout.py

C++

docs/articles\_en/assets/snippets/ov\_layout.cpp

## See also

  - API Reference: `ov::Layout` C++ class
