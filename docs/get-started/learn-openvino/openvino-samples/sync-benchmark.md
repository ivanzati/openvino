---
sidebar_label: 'Sync Benchmark Sample'
format: md
---

# Sync Benchmark Sample

This sample demonstrates how to estimate performance of a model using Synchronous
Inference Request API. It makes sense to use synchronous inference only in latency
oriented scenarios. Models with static input shapes are supported.
This sample does not have other configurable command-line
arguments. Feel free to modify sample's source code to try out different options.
Before using the sample, refer to the following requirements:

  - The sample accepts any file format supported by `core.read_model`.
  - The sample has been validated with: the yolo-v3-tf and face-detection-0200 models.
  - To build the sample, use instructions available at *Build the Sample Applications*
    section in "Get Started with Samples" guide.

## How It Works

The sample compiles a model for a given device, randomly generates input data,
performs synchronous inference multiple times for a given number of seconds.
Then, it processes and reports performance results.

Python

samples/python/benchmark/sync\_benchmark/sync\_benchmark.py

C++

samples/cpp/benchmark/sync\_benchmark/main.cpp

You can see the explicit description of
each sample step at \[Integration Steps\](../../../openvino-workflow/running-inference.md)
section of "Integrate OpenVINO™ Runtime with Your Application" guide.

## Running

Python

``` console
python sync_benchmark.py <path_to_model> <device_name>(default: CPU)
```

C++

``` console
sync_benchmark <path_to_model> <device_name>(default: CPU)
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
    python sync_benchmark.py googlenet-v1.xml
    ```
    
    C++
    
    ``` console
    sync_benchmark googlenet-v1.xml
    ```

## Sample Output

Python

The application outputs performance results.

``` console
[ INFO ] OpenVINO:
[ INFO ] Build ................................. <version>
[ INFO ] Count:          2333 iterations
[ INFO ] Duration:       10003.59 ms
[ INFO ] Latency:
[ INFO ]     Median:     3.90 ms
[ INFO ]     Average:    4.29 ms
[ INFO ]     Min:        3.30 ms
[ INFO ]     Max:        10.11 ms
[ INFO ] Throughput: 233.22 FPS
```

C++

The application outputs performance results.

``` console
[ INFO ] OpenVINO:
[ INFO ] Build ................................. <version>
[ INFO ] Count:      992 iterations
[ INFO ] Duration:   15009.8 ms
[ INFO ] Latency:
[ INFO ]        Median:     14.00 ms
[ INFO ]        Average:    15.13 ms
[ INFO ]        Min:        9.33 ms
[ INFO ]        Max:        53.60 ms
[ INFO ] Throughput: 66.09 FPS
```

## Additional Resources

  - \[Integrate the OpenVINO™ Runtime with Your Application\](../../../openvino-workflow/running-inference.md)
  - \[Get Started with Samples\](get-started-demos.md)
  - \[Using OpenVINO Samples\](../openvino-samples.md)
  - \[Convert a Model\](../../../openvino-workflow/model-preparation/convert-model-to-ir.md)
  - [Sync Benchmark Python Sample on Github](https://github.com/openvinotoolkit/openvino/blob/master/samples/python/benchmark/sync_benchmark/README.md)
  - [Sync Benchmark C++ Sample on Github](https://github.com/openvinotoolkit/openvino/blob/master/samples/cpp/benchmark/sync_benchmark/README.md)
