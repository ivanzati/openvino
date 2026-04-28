---
sidebar_label: 'Converting a TensorFlow Lite Model'
format: md
---

# Converting a TensorFlow Lite Model

> TensorFlow Lite format to the OpenVINO Model.

You can download a TensorFlow Lite model from
[Kaggle](https://www.kaggle.com/models?framework=tfLite&subtype=module,placeholder&tfhub-redirect=true)
or [Hugging Face](https://huggingface.co/models).
To convert the model, run model conversion with the path to the `.tflite` model file:

Python

``` py
import openvino as ov
ov.convert_model('your_model_file.tflite')
```

CLI

``` sh
ovc your_model_file.tflite
```

Note

TensorFlow Lite model file can be loaded by `openvino.Core.read_model` or
`openvino.Core.compile_model` methods by OpenVINO runtime API without preparing
OpenVINO IR first. Refer to the
\[inference example\](../running-inference.md)
for more details. Using `openvino.convert_model` is still recommended if model
load latency matters for the inference application.

## Supported TensorFlow Lite Layers

For the list of supported standard layers, refer to the
\[Supported Operations\](../../documentation/compatibility-and-support/supported-operations.md)
page.

## Supported TensorFlow Lite Models

More than eighty percent of public TensorFlow Lite models are supported from open
sources [Kaggle](https://www.kaggle.com/models?framework=tfLite&subtype=module,placeholder&tfhub-redirect=true)
and [MediaPipe](https://developers.google.com/mediapipe).
Unsupported models usually have custom TensorFlow Lite operations.
