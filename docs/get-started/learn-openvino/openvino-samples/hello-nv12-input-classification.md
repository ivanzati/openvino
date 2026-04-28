---
sidebar_label: 'Hello NV12 Input Classification Sample'
format: md
---

# Hello NV12 Input Classification Sample

> classification models with images in NV12 color format using
> Synchronous Inference Request (C++) API.

This sample demonstrates how to execute an inference of image classification models
with images in NV12 color format using Synchronous Inference Request API. Before
using the sample, refer to the following requirements:

  - The sample accepts any file format supported by `ov::Core::read_model`.
  - To build the sample, use instructions available at *Build the Sample Applications*
    section in "Get Started with Samples" guide.

## How It Works

At startup, the sample application reads command line parameters, loads the
specified model and an image in the NV12 color format to an OpenVINO™ Runtime
plugin. Then, the sample creates an synchronous inference request object. When
inference is done, the application outputs data to the standard output stream.
You can place labels in `.labels` file near the model to get pretty output.

C++

samples/cpp/hello\_nv12\_input\_classification/main.cpp

C

samples/c/hello\_nv12\_input\_classification/main.c

You can see the explicit description of each sample step at
\[Integration Steps\](../../../openvino-workflow/running-inference.md)
section of "Integrate OpenVINO™ Runtime with Your Application" guide.

## Running

C++

``` console
hello_nv12_input_classification <path_to_model> <path_to_image> <image_size> <device_name>
```

C

``` console
hello_nv12_input_classification_c <path_to_model> <path_to_image> <device_name>
```

To run the sample, you need to specify a model and an image:

  - You can get a model specific for your inference task from one of model
    repositories, such as TensorFlow Zoo, HuggingFace, or TensorFlow Hub.
  - You can use images from the media files collection available at
    [the storage](https://storage.openvinotoolkit.org/data/test_data).

The sample accepts an uncompressed image in the NV12 color format. To run the
sample, you need to convert your BGR/RGB image to NV12. To do this, you can use
one of the widely available tools such as FFmpeg or GStreamer. Using FFmpeg and
the following command, you can convert an ordinary image to an uncompressed NV12 image:

``` sh
ffmpeg -i cat.jpg -pix_fmt nv12 cat.yuv
```

Note

  - Because the sample reads raw image files, you should provide a correct image
    size along with the image path. The sample expects the logical size of the
    image, not the buffer size. For example, for 640x480 BGR/RGB image the
    corresponding NV12 logical image size is also 640x480, whereas the buffer
    size is 640x720.
  - By default, this sample expects that model input has BGR channels order. If
    you trained your model to work with RGB order, you need to reconvert your
    model using model conversion API with `reverse_input_channels` argument
    specified. For more information about the argument, refer to the
    **Color Conversion** section of \[Preprocessing API\](../../../openvino-workflow/running-inference/optimize-inference/optimize-preprocessing/preprocessing-api-details.md).
  - Before running the sample with a trained model, make sure the model is
    converted to the intermediate representation (IR) format (\*.xml + \*.bin)
    using the \[model conversion API\](../../../openvino-workflow/model-preparation/convert-model-to-ir.md).
  - The sample accepts models in ONNX format (.onnx) that do not require preprocessing.

### Example

1.  Download a pre-trained model.

2.  You can convert it by using:
    
    ``` console
    ovc ./models/alexnet
    ```

3.  Perform inference of an NV12 image, using a model on a `CPU`, for example:
    
    C++
    
    ``` console
    hello_nv12_input_classification ./models/alexnet.xml ./images/cat.yuv 300x300 CPU
    ```
    
    C
    
    ``` console
    hello_nv12_input_classification_c ./models/alexnet.xml ./images/cat.yuv 300x300 CPU
    ```

## Sample Output

C++

The application outputs top-10 inference results.

``` console
[ INFO ] OpenVINO Runtime version ......... <version>
[ INFO ] Build ........... <build>
[ INFO ]
[ INFO ] Loading model files: \models\alexnet.xml
[ INFO ] model name: AlexNet
[ INFO ]     inputs
[ INFO ]         input name: data
[ INFO ]         input type: f32
[ INFO ]         input shape: {1, 3, 227, 227}
[ INFO ]     outputs
[ INFO ]         output name: prob
[ INFO ]         output type: f32
[ INFO ]         output shape: {1, 1000}

Top 10 results:

Image \images\car.yuv

classid probability
------- -----------
656     0.6668988
654     0.1125269
581     0.0679280
874     0.0340229
436     0.0257744
817     0.0169367
675     0.0110199
511     0.0106134
569     0.0083373
717     0.0061734
```

C

The application outputs top-10 inference results.

``` console
Top 10 results:

Image ./cat.yuv

classid probability
------- -----------
435       0.091733
876       0.081725
999       0.069305
587       0.043726
666       0.038957
419       0.032892
285       0.030309
700       0.029941
696       0.021628
855       0.020339

This sample is an API example, for any performance measurements please use the dedicated benchmark_app tool
```

## Additional Resources

  - \[Integrate the OpenVINO™ Runtime with Your Application\](../../../openvino-workflow/running-inference.md)
  - \[Get Started with Samples\](get-started-demos.md)
  - \[Using OpenVINO Samples\](../openvino-samples.md)
  - \[Convert a Model\](../../../openvino-workflow/model-preparation/convert-model-to-ir.md)
  - [API Reference](https://docs.openvino.ai/2026/api/api_reference.html)
  - [Hello NV12 Input Classification C++ Sample on Github](https://github.com/openvinotoolkit/openvino/blob/master/samples/cpp/hello_nv12_input_classification/README.md)
  - [Hello NV12 Input Classification C Sample on Github](https://github.com/openvinotoolkit/openvino/blob/master/samples/c/hello_nv12_input_classification/README.md)
