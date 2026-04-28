---
sidebar_label: 'Throughput Benchmark Sample'
format: md
---

# Throughput Benchmark Sample

This sample demonstrates how to estimate performance of a model using Asynchronous
Inference Request API in throughput mode. This sample
does not have other configurable command-line arguments. Feel free to modify sample's
source code to try out different options.

The reported results may deviate from what \[benchmark\_app\](benchmark-tool.md)
reports. One example is model input precision for computer vision tasks. benchmark\_app
sets `uint8`, while the sample uses default model precision which is usually `float32`.

Before using the sample, refer to the following requirements:

  - The sample accepts any file format supported by `core.read_model`.
  - The sample has been validated with: yolo-v3-tf and face-detection-0200 models.
  - To build the sample, use instructions available at *Build the Sample Applications*
    section in "Get Started with Samples" guide.

## How It Works

The sample compiles a model for a given device, randomly generates input data,
performs asynchronous inference multiple times for a given number of seconds.
Then, it processes and reports performance results.

Python

samples/python/benchmark/throughput\_benchmark/throughput\_benchmark.py

C++

samples/cpp/benchmark/throughput\_benchmark/main.cpp

You can see the explicit description of each sample step at
\[Integration Steps\](../../../openvino-workflow/running-inference.md)
section of "Integrate OpenVINO™ Runtime with Your Application" guide.

## Running

Python

``` console
python throughput_benchmark.py <path_to_model> <device_name>(default: CPU)
```

C++

``` console
throughput_benchmark <path_to_model> <device_name>(default: CPU)
```

To run the sample, you need to specify a model. You can get a model specific for
your inference task from one of model repositories, such as TensorFlow Zoo, HuggingFace, or TensorFlow Hub.

### Example

1.  Download a pre-trained model.

2.  You can convert it by using:
    
    Python
    
    ``` python
    import openvino as ov
    
    ov_model = ov.convert_model('./models/googlenet-v1')
    # or, when model is a Python model object
    ov_model = ov.convert_model(googlenet-v1)
    ```
    
    CLI
    
    ``` console
    ovc ./models/googlenet-v1
    ```

3.  Perform benchmarking, using the `googlenet-v1` model on a `CPU`:
    
    Python
    
    ``` console
    python throughput_benchmark.py ./models/googlenet-v1.xml
    ```
    
    C++
    
    ``` console
    throughput_benchmark ./models/googlenet-v1.xml
    ```

## Sample Output

Python

The application outputs performance results.

``` console
[ INFO ] OpenVINO:
[ INFO ] Build ................................. <version>
[ INFO ] Count:          2817 iterations
[ INFO ] Duration:       10012.65 ms
[ INFO ] Latency:
[ INFO ]     Median:     13.80 ms
[ INFO ]     Average:    14.10 ms
[ INFO ]     Min:        8.35 ms
[ INFO ]     Max:        28.38 ms
[ INFO ] Throughput: 281.34 FPS
```

C++

The application outputs performance results.

``` console
[ INFO ] OpenVINO:
[ INFO ] Build ................................. <version>
[ INFO ] Count:      1577 iterations
[ INFO ] Duration:   15024.2 ms
[ INFO ] Latency:
[ INFO ]        Median:     38.02 ms
[ INFO ]        Average:    38.08 ms
[ INFO ]        Min:        25.23 ms
[ INFO ]        Max:        49.16 ms
[ INFO ] Throughput: 104.96 FPS
```

## Additional Resources

  - \[Integrate the OpenVINO™ Runtime with Your Application\](../../../openvino-workflow/running-inference.md)
  - \[Get Started with Samples\](get-started-demos.md)
  - \[Using OpenVINO Samples\](../openvino-samples.md)
  - \[Convert a Model\](../../../openvino-workflow/model-preparation/convert-model-to-ir.md)
  - [Throughput Benchmark Python Sample on Github](https://github.com/openvinotoolkit/openvino/blob/master/samples/python/benchmark/throughput_benchmark/README.md)
  - [Throughput Benchmark C++ Sample on Github](https://github.com/openvinotoolkit/openvino/blob/master/samples/cpp/benchmark/throughput_benchmark/README.md)
