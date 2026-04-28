---
sidebar_label: 'Hello Reshape SSD Sample'
format: md
---

# Hello Reshape SSD Sample

> models using shape inference feature and Synchronous
> Inference Request API (Python, C++).

This sample demonstrates how to do synchronous inference of object detection models
using \[Shape Inference feature\](../../../openvino-workflow/running-inference/model-input-output/changing-input-shape.md). Before
using the sample, refer to the following requirements:

  - Models with only one input and output are supported.
  - The sample accepts any file format supported by `core.read_model`.
  - The sample has been validated with the person-detection-retail-0013
    model and the NCHW layout format.
  - To build the sample, use instructions available at *Build the Sample Applications*
    section in "Get Started with Samples" guide.

## How It Works

At startup, the sample application reads command-line parameters, prepares input data, loads a specified model and image to the OpenVINO™ Runtime plugin, performs synchronous inference, and processes output data.
As a result, the program creates an output image, logging each step in a standard output stream.

Python

samples/python/hello\_reshape\_ssd/hello\_reshape\_ssd.py

C++

samples/cpp/hello\_reshape\_ssd/main.cpp

You can see the explicit description of
each sample step at \[Integration Steps\](../../../openvino-workflow/running-inference.md) section of "Integrate OpenVINO™ Runtime with Your Application" guide.

## Running

Python

``` console
python hello_reshape_ssd.py <path_to_model> <path_to_image> <device_name>
```

C++

``` console
hello_reshape_ssd <path_to_model> <path_to_image> <device_name>
```

To run the sample, you need to specify a model and an image:

  - You can get a model specific for your inference task from one of model
    repositories, such as TensorFlow Zoo, HuggingFace, or TensorFlow Hub.
  - You can use images from the media files collection available at
    [the storage](https://storage.openvinotoolkit.org/data/test_data).

Note

  - By default, OpenVINO™ Toolkit Samples and demos expect input with BGR channels
    order. If you trained your model to work with RGB order, you need to manually
    rearrange the default channels order in the sample or demo application or
    reconvert your model using model conversion API with `reverse_input_channels`
    argument specified. For more information about the argument, refer to the
    **Color Conversion** section of
    \[Preprocessing API\](../../../openvino-workflow/running-inference/optimize-inference/optimize-preprocessing/preprocessing-api-details.md).
  - Before running the sample with a trained model, make sure the model is
    converted to the intermediate representation (IR) format (\*.xml + \*.bin)
    using \[model conversion API\](../../../openvino-workflow/model-preparation/convert-model-to-ir.md).
  - The sample accepts models in ONNX format (.onnx) that do not require preprocessing.

### Example

1.  Download a pre-trained model:

2.  You can convert it by using:
    
    Python
    
    ``` python
    import openvino as ov
    
    ov_model = ov.convert_model('./test_data/models/mobilenet-ssd')
    # or, when model is a Python model object
    ov_model = ov.convert_model(mobilenet-ssd)
    ```
    
    CLI
    
    ``` console
    ovc ./test_data/models/mobilenet-ssd
    ```

3.  Perform inference of an image, using a model on a `GPU`, for example:
    
    Python
    
    ``` console
    python hello_reshape_ssd.py ./test_data/models/mobilenet-ssd.xml banana.jpg GPU
    ```
    
    C++
    
    ``` console
    hello_reshape_ssd ./models/person-detection-retail-0013.xml person_detection.bmp GPU
    ```

## Sample Output

Python

The sample application logs each step in a standard output stream and
creates an output image, drawing bounding boxes for inference results
with an over 50% confidence.

``` console
[ INFO ] Creating OpenVINO Runtime Core
[ INFO ] Reading the model: C:/test_data/models/mobilenet-ssd.xml
[ INFO ] Reshaping the model to the height and width of the input image
[ INFO ] Loading the model to the plugin
[ INFO ] Starting inference in synchronous mode
[ INFO ] Found: class_id = 52, confidence = 0.98, coords = (21, 98), (276, 210)
[ INFO ] Image out.bmp was created!
[ INFO ] This sample is an API example, for any performance measurements please use the dedicated benchmark_app tool
```

C++

The application renders an image with detected objects enclosed in rectangles.
It outputs the list of classes of the detected objects along with the
respective confidence values and the coordinates of the rectangles to the
standard output stream.

``` console
[ INFO ] OpenVINO Runtime version ......... <version>
[ INFO ] Build ........... <build>
[ INFO ]
[ INFO ] Loading model files: \models\person-detection-retail-0013.xml
[ INFO ] model name: ResMobNet_v4 (LReLU) with single SSD head
[ INFO ]     inputs
[ INFO ]         input name: data
[ INFO ]         input type: f32
[ INFO ]         input shape: {1, 3, 320, 544}
[ INFO ]     outputs
[ INFO ]         output name: detection_out
[ INFO ]         output type: f32
[ INFO ]         output shape: {1, 1, 200, 7}
Reshape network to the image size = [960x1699]
[ INFO ] model name: ResMobNet_v4 (LReLU) with single SSD head
[ INFO ]     inputs
[ INFO ]         input name: data
[ INFO ]         input type: f32
[ INFO ]         input shape: {1, 3, 960, 1699}
[ INFO ]     outputs
[ INFO ]         output name: detection_out
[ INFO ]         output type: f32
[ INFO ]         output shape: {1, 1, 200, 7}
[0,1] element, prob = 0.716309,    (852,187)-(983,520)
The resulting image was saved in the file: hello_reshape_ssd_output.bmp

This sample is an API example, for any performance measurements please use the dedicated benchmark_app tool
```

## Additional Resources

  - \[Integrate the OpenVINO™ Runtime with Your Application\](../../../openvino-workflow/running-inference.md)
  - \[Get Started with Samples\](get-started-demos.md)
  - \[Using OpenVINO Samples\](../openvino-samples.md)
  - \[Convert a Model\](../../../openvino-workflow/model-preparation/convert-model-to-ir.md)
  - [Hello Reshape SSD Python Sample on Github](https://github.com/openvinotoolkit/openvino/blob/master/samples/python/hello_reshape_ssd/README.md)
  - [Hello Reshape SSD C++ Sample on Github](https://github.com/openvinotoolkit/openvino/blob/master/samples/cpp/hello_reshape_ssd/README.md)
