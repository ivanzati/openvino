---
sidebar_label: 'Hello Classification Sample'
format: md
---

# Hello Classification Sample

> models using Synchronous Inference Request API (Python, C++, C).

This sample demonstrates how to do inference of image classification models using
Synchronous Inference Request API. Before using the sample, refer to the following requirements:

  - Models with only one input and output are supported.
  - The sample accepts any file format supported by `core.read_model`.
  - To build the sample, use instructions available at *Build the Sample Applications*
    section in "Get Started with Samples" guide.

## How It Works

At startup, the sample application sets log message capturing callback and reads command-line parameters.
Then it prepares input data, loads a specified model and image to the OpenVINO™ Runtime plugin, performs synchronous
inference, and processes output data, logging each step in a standard output stream.

Python

samples/python/hello\_classification/hello\_classification.py

C++

samples/cpp/hello\_classification/main.cpp

C

samples/c/hello\_classification/main.c

You can see the explicit description of each sample step at
\[Integration Steps\](../../../openvino-workflow/running-inference.md)
section of "Integrate OpenVINO™ Runtime with Your Application" guide.

## Running

Python

``` console
python hello_classification.py <path_to_model> <path_to_image> <device_name>
```

C++

``` console
hello_classification <path_to_model> <path_to_image> <device_name>
```

C

``` console
hello_classification_c <path_to_model> <path_to_image> <device_name>
```

To run the sample, you need to specify a model and an image:

  - You can get a model specific for your inference task from one of model
    repositories, such as TensorFlow Zoo, HuggingFace, or TensorFlow Hub.
  - You can use images from the media files collection available at
    [the storage](https://storage.openvinotoolkit.org/data/test_data).

Note

  - By default, OpenVINO™ Toolkit Samples and demos expect input with BGR
    channels order. If you trained your model to work with RGB order, you need
    to manually rearrange the default channels order in the sample or demo
    application or reconvert your model using model conversion API with
    `reverse_input_channels` argument specified. For more information about
    the argument, refer to the **Color Conversion** section of
    \[Preprocessing API\](../../../openvino-workflow/running-inference/optimize-inference/optimize-preprocessing/preprocessing-api-details.md).
  - Before running the sample with a trained model, make sure the model is
    converted to the intermediate representation (IR) format (\*.xml + \*.bin)
    using the \[model conversion API\](../../../openvino-workflow/model-preparation/convert-model-to-ir.md).
  - The sample accepts models in ONNX format (.onnx) that do not require preprocessing.
  - The sample supports NCHW model layout only.

### Example

1.  Download a pre-trained model.

2.  You can convert it by using:
    
    Python
    
    ``` python
    import openvino as ov
    
    ov_model = ov.convert_model('./models/alexnet')
    # or, when model is a Python model object
    ov_model = ov.convert_model(alexnet)
    ```
    
    CLI
    
    ``` console
    ovc ./models/alexnet
    ```

3.  Perform inference of an image, using a model on a `GPU`, for example:
    
    Python
    
    ``` console
    python hello_classification.py ./models/alexnet/alexnet.xml ./images/banana.jpg GPU
    ```
    
    C++
    
    ``` console
    hello_classification ./models/googlenet-v1.xml ./images/car.bmp GPU
    ```
    
    C
    
    ``` console
    hello_classification_c alexnet.xml ./opt/intel/openvino/samples/scripts/car.png GPU
    ```

## Sample Output

Python

The sample application logs each step in a standard output stream and
outputs top-10 inference results.

``` console
[ INFO ] Creating OpenVINO Runtime Core
[ INFO ] Reading the model: /models/alexnet/alexnet.xml
[ INFO ] Loading the model to the plugin
[ INFO ] Starting inference in synchronous mode
[ INFO ] Image path: /images/banana.jpg
[ INFO ] Top 10 results:
[ INFO ] class_id probability
[ INFO ] --------------------
[ INFO ] 954      0.9703885
[ INFO ] 666      0.0219518
[ INFO ] 659      0.0033120
[ INFO ] 435      0.0008246
[ INFO ] 809      0.0004433
[ INFO ] 502      0.0003852
[ INFO ] 618      0.0002906
[ INFO ] 910      0.0002848
[ INFO ] 951      0.0002427
[ INFO ] 961      0.0002213
[ INFO ]
[ INFO ] This sample is an API example, for any performance measurements please use the dedicated benchmark_app tool
```

C++

The application outputs top-10 inference results.

``` console
[ INFO ] OpenVINO Runtime version ......... <version>
[ INFO ] Build ........... <build>
[ INFO ]
[ INFO ] Loading model files: /models/googlenet-v1.xml
[ INFO ] model name: GoogleNet
[ INFO ]     inputs
[ INFO ]         input name: data
[ INFO ]         input type: f32
[ INFO ]         input shape: {1, 3, 224, 224}
[ INFO ]     outputs
[ INFO ]         output name: prob
[ INFO ]         output type: f32
[ INFO ]         output shape: {1, 1000}

Top 10 results:

Image /images/car.bmp

classid probability
------- -----------
656     0.8139648
654     0.0550537
468     0.0178375
436     0.0165405
705     0.0111694
817     0.0105820
581     0.0086823
575     0.0077515
734     0.0064468
785     0.0043983
```

C

The application outputs top-10 inference results.

``` console
Top 10 results:

Image /opt/intel/openvino/samples/scripts/car.png

classid probability
------- -----------
656       0.666479
654       0.112940
581       0.068487
874       0.033385
436       0.026132
817       0.016731
675       0.010980
511       0.010592
569       0.008178
717       0.006336

This sample is an API example, for any performance measurements use the dedicated benchmark_app tool.
```

## Additional Resources

  - \[Integrate the OpenVINO™ Runtime with Your Application\](../../../openvino-workflow/running-inference.md)
  - \[Get Started with Samples\](get-started-demos.md)
  - \[Using OpenVINO Samples\](../openvino-samples.md)
  - \[Convert a Model\](../../../openvino-workflow/model-preparation/convert-model-to-ir.md)
  - [OpenVINO Runtime C API](https://docs.openvino.ai/2026/api/c_cpp_api/group__ov__c__api.html)
  - [Hello Classification Python Sample on Github](https://github.com/openvinotoolkit/openvino/blob/master/samples/python/hello_classification/README.md)
  - [Hello Classification C++ Sample on Github](https://github.com/openvinotoolkit/openvino/blob/master/samples/cpp/hello_classification/README.md)
  - [Hello Classification C Sample on Github](https://github.com/openvinotoolkit/openvino/blob/master/samples/c/hello_classification/README.md)
