---
sidebar_label: 'Torchvision Preprocessing Converter'
format: md
---

# Torchvision Preprocessing Converter

> to optimize model inference.

The Torchvision-to-OpenVINO converter enables automatic translation of operators from the
torchvision preprocessing pipeline to the OpenVINO format and embed them in your model. It is
often used to adjust images serving as input for AI models to have proper dimensions or data
types.

As the converter is fully based on the **openvino.preprocess** module, you can implement the
**torchvision.transforms** feature easily and without the use of external libraries, reducing
the overall application complexity and enabling additional performance optimizations.

Note

Not all torchvision transforms are supported yet. The following operations are available:

    transforms.Compose
    transforms.Normalize
    transforms.ConvertImageDtype
    transforms.Grayscale
    transforms.Pad
    transforms.ToTensor
    transforms.CenterCrop
    transforms.Resize

## Example

docs/articles\_en/assets/snippets/torchvision\_preprocessing.py
