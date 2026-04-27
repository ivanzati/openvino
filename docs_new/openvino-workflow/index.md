---
sidebar_label: 'Conventional AI Workflow'
format: md
---

# Conventional AI Workflow

> optimization, and compression of models, running inference and
> deploying deep learning applications.

OpenVINO offers multiple workflows, depending on the use case and personal or project preferences.
This section will give you a detailed view of how you can go from preparing your model,
through optimizing it, to executing inference, and deploying your solution.

Once you obtain a model in one of the \[supported model formats\](openvino-workflow/model-preparation.md),
you can decide how to proceed:

Workflow for convenience

This approach assumes you run your model directly.

![OpenVINO workflow diagram for convenience](img/ov_workflow_diagram_convenience.svg)

Workflow for performance (recommended for production)

This approach assumes you convert your model to OpenVINO IR explicitly, which means the
conversion stage is not part of the final application.

![OpenVINO workflow diagram for performance](img/ov_workflow_diagram_performance.svg)

OpenVINO uses the following functions for reading, converting, and saving models:

read\_model

  - Creates an ov.Model from a file.
  - Supported file formats: OpenVINO IR, ONNX, PaddlePaddle, TensorFlow and TensorFlow Lite. PyTorch files are not directly supported.
  - OpenVINO files are read directly while other formats are converted automatically.

compile\_model

  - Creates an ov.CompiledModel from a file or ov.Model object.
  - Supported file formats: OpenVINO IR, ONNX, PaddlePaddle, TensorFlow and TensorFlow Lite. PyTorch files are not directly supported.
  - OpenVINO files are read directly while other formats are converted automatically.

convert\_model

  - Creates an ov.Model from a file or Python memory object.
  - Supported file formats: ONNX, PaddlePaddle, TensorFlow and TensorFlow Lite.
  - Supported framework objects: PaddlePaddle, TensorFlow and PyTorch.
  - This method is only available in the Python API.

save\_model

  - Saves an ov.Model to OpenVINO IR format.
  - Compresses weights to FP16 by default.
  - This method is only available in the Python API.

\[Model Preparation\](openvino-workflow/model-preparation.md)  
   Learn how to convert pre-trained models to OpenVINO IR.

\[Model Optimization and Compression\](openvino-workflow/model-optimization.md)  
   Find out how to optimize a model to achieve better inference performance, utilizing multiple optimization methods for both in-training compression and post-training quantization.

\[Running Inference\](openvino-workflow/running-inference.md)  
   See how to run inference with OpenVINO, which is the most basic form of deployment, and the quickest way of running a deep learning model.

\[Deployment Option 1. Using OpenVINO Runtime\](openvino-workflow/deployment-locally.md)  
   Deploy a model locally, reading the file directly from your application and utilizing resources available to the system.  
   Deployment on a local system uses the steps described in the section on running inference.

\[Deployment Option 2. Using Model Server\](../model-server/ovms\_what\_is\_openvino\_model\_server.md)  
   Deploy a model remotely, connecting your application to an inference server and utilizing external resources, with no impact on the app's performance.  
   Deployment on OpenVINO Model Server is quick and does not require any additional steps described in the section on running inference.

\[Deployment Option 3. Using torch.compile for PyTorch 2.0\](openvino-workflow/torch-compile.md)  
   Deploy a PyTorch model using OpenVINO in a PyTorch-native application.
