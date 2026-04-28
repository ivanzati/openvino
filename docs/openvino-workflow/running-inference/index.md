---
sidebar_label: 'Running and Integrating Inference Pipeline'
format: md
---

# Running and Integrating Inference Pipeline

> Runtime in an application.⠀

OpenVINO Runtime is a set of C++ libraries with C and Python bindings, providing a common
API to run inference on various devices. Each device (integrated with OpenVINO’s plugin
architecture) offers the common, as well as hardware-specific APIs for more configuration
options. Note that OpenVINO Runtime may also be integrated with other frameworks and work
as their backend, for example, using torch.compile.
The scheme below illustrates the typical workflow for deploying a trained deep learning
model in an application:

![image](img/IMPLEMENT_PIPELINE_with_API_C.svg)

This guide will show you how to implement a typical OpenVINO™ Runtime inference pipeline
in your application. Before proceeding, check how
\[model conversion\](model-preparation/convert-model-to-ir.md)
works in OpenVINO and how it may affect your applications’ performance. Make sure you have
installed OpenVINO Runtime and set environment variables (otherwise, the `find_package`
calls will not find OpenVINO\_DIR):

Linux

``` console
<INSTALL_DIR>/setupvars.sh
```

Windows

PowerShell:

``` console
<INSTALL_DIR>/setupvars.sh
```

Command Prompt

``` console
cd  <INSTALL_DIR>
setupvars.bat
```

## Step 1. Create OpenVINO Runtime Core

Initiate working with OpenVINO in your application by including the OpenVINO™ Runtime
components:

Python

docs/snippets/src/main.py

docs/snippets/src/main.py

C++

docs/snippets/src/main.cpp

docs/snippets/src/main.cpp

C

docs/snippets/src/main.c

docs/snippets/src/main.c

## Step 2. Compile the Model

Compile the model with `ov::Core::compile_model()`, defining the device or mode to use
for inference. The following example uses the
\[AUTO mode\](running-inference/inference-devices-and-modes/auto-device-selection.md),
which selects the device for you. To learn more about supported devices and inference modes,
see the \[Inference Devices and Modes\](running-inference/inference-devices-and-modes.md)
section.

Python

IR

docs/snippets/src/main.py

ONNX

docs/snippets/src/main.py

PaddlePaddle

docs/snippets/src/main.py

TensorFlow

docs/snippets/src/main.py

TensorFlow Lite

docs/snippets/src/main.py

ov::Model

docs/snippets/src/main.py

C++

IR

docs/snippets/src/main.cpp

ONNX

docs/snippets/src/main.cpp

PaddlePaddle

docs/snippets/src/main.cpp

TensorFlow

docs/snippets/src/main.cpp

TensorFlow Lite

docs/snippets/src/main.cpp

ov::Model

docs/snippets/src/main.cpp

C

IR

docs/snippets/src/main.c

ONNX

docs/snippets/src/main.c

PaddlePaddle

docs/snippets/src/main.c

TensorFlow

docs/snippets/src/main.c

TensorFlow Lite

docs/snippets/src/main.c

ov::Model

docs/snippets/src/main.c

The `ov::CompiledModel` class represents a compiled model and enables you to get
information inputs or output ports by a tensor name or index. This approach is aligned with
most frameworks. The `ov::Model` object represents any models inside the OpenVINO™ Runtime.
For more details, refer to
\[OpenVINO™ Model representation\](running-inference/model-representation.md).

## Step 3. Create an Inference Request

Use the `ov::InferRequest` class methods to create an infer request. For more details,
see the article on
\[InferRequest\](running-inference/inference-request.md).

Python

docs/snippets/src/main.py

C++

docs/snippets/src/main.cpp

C

docs/snippets/src/main.c

## Step 4. Set Inputs

Create `ov::Tensor`, you can use external memory for that , and use the
`ov::InferRequest::set_input_tensor` method to send this tensor to the device.
For more info on textual data as input, see the
\[String Tensors\](running-inference/model-input-output/string-tensors.md) article.

Python

docs/snippets/src/main.py

C++

docs/snippets/src/main.cpp

C

docs/snippets/src/main.c

## Step 5. Start Inference

Use either `ov::InferRequest::start_async` or `ov::infer_request.infer()` to start model
inference. To learn how they work, see the
\[OpenVINO Inference Request\](running-inference/inference-request.md)
article. The following example uses the asynchronous option and calls
`ov::InferRequest::wait` to wait for the inference results.

Python

docs/snippets/src/main.py

C++

docs/snippets/src/main.cpp

C

docs/snippets/src/main.c

## Step 6. Process the Inference Results

Get output tensors and process the inference results.
For more info on textual data as input, see the
\[String Tensors\](running-inference/model-input-output/string-tensors.md) article.

Python

docs/snippets/src/main.py

C++

docs/snippets/src/main.cpp

C

docs/snippets/src/main.c

## Step 7. \[only for C\] Release the allocated objects

To avoid memory leak, applications developed with the C API need to release the allocated
objects in the following order.

C

docs/snippets/src/main.c

## Build Your Application

If you have integrated OpenVINO with your application, you will need to adjust your
application build process as well. Of course, there are multiple ways this stage may be
done, so you will need to choose the one best for your project. To learn about the basics of
OpenVINO build process, refer to the
[documentation on GitHub](https://github.com/openvinotoolkit/openvino/blob/master/docs/dev/build.md).

The following example uses a C++ & C application together with CMake,
for project configuration.

1.  Create Structure for project:
    
    C++
    
    docs/snippets/src/main.cpp
    
    C
    
    docs/snippets/src/main.c

2.  Configure the CMake build
    
    For details on additional CMake build options, refer to the
    [CMake page](https://cmake.org/cmake/help/latest/manual/cmake.1.html#manual:cmake\(1\)).
    
    C++
    
    docs/snippets/CMakeLists.txt
    
    C
    
    docs/snippets/CMakeLists.txt
    
    C++ (PyPI)
    
    docs/snippets/CMakeLists.txt

3.  Build Project
    
    Use CMake to build the project on your system:
    
    ``` sh
    cd build/
    cmake ../project
    cmake --build .
    ```

## Additional Resources

  - To see working implementation of the steps, check out the
    \[Learn OpenVINO\](../get-started/learn-openvino.md) section, including
    [OpenVINO™ Runtime API Tutorial](./../../notebooks/openvino-api-with-output.html).
  - Models in the OpenVINO IR format on [Hugging Face](https://huggingface.co/models).
  - *Using Encrypted Models with OpenVINO*
