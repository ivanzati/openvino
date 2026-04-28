---
sidebar_label: 'Install OpenVINO™ Runtime from Conan Package Manager'
format: md
---

# Install OpenVINO™ Runtime from Conan Package Manager

> macOS operating systems, using Conan Package Manager.

Note

Note that the Conan Package Manager distribution:

  - offers C/C++ API only
  - does not offer support for NPU inference
  - is dedicated to users of all major OSes: Windows, Linux, and macOS
    (all x86\_64 / arm64 architectures)

Before installing OpenVINO, see the
\[System Requirements page\](../../../about-openvino/release-notes-openvino/system-requirements.md).

Note

This community-maintained distribution channel is provided to help users explore and evaluate OpenVINO.

Please note that accuracy, performance, and behavior may differ from officially supported OpenVINO distributions, and are not guaranteed by the OpenVINO team in this channel. Due to the community-driven nature of this distribution channel, the OpenVINO team does not guarantee timely updates aligned with official releases, nor update availability for all OpenVINO versions.

For production deployments and product integration, we recommend using officially supported distribution channels (for example, official S3 archives or PyPI packages), which provide validated builds and defined guarantees.

## Installing OpenVINO Runtime with Conan Package Manager

1.  [Install Conan](https://docs.conan.io/2/installation.html) 2.0.8 or higher, for example, using pip:
    
    ``` sh
    python3 -m pip install 'conan>=2.0.8'
    ```

2.  Create a `conanfile.txt` file for your OpenVINO project and add "*openvino*" dependency in there:
    
    ``` sh
    [requires]
    openvino/2026.0.0
    [generators]
    CMakeDeps
    CMakeToolchain
    [layout]
    cmake_layout
    ```
    
    Run the command below to create `conan_toolchain.cmake` file, which will be used to compile your project with OpenVINO:
    
    ``` sh
    conan install conanfile.txt --build=missing
    ```
    
    By default, OpenVINO is statically compiled, together with all available
    plugins and frontends. To build a version tailored to your needs, check
    what options there are on the [Conan Package Manager page for OpenVINO](https://conan.io/center/recipes/openvino)
    and extend the command, like so:
    
    ``` sh
    conan install conanfile.txt --build=missing -o:h 'openvino/*:enable_intel_gpu=False' -o:h 'openvino/*:enable_onnx_frontend=False' -o:h 'openvino/*:shared=True'
    ```

3.  Configure and compile your project with OpenVINO:
    
    ``` sh
    cmake -DCMAKE_TOOLCHAIN_FILE=<path to conan_toolchain.cmake> -DCMAKE_BUILD_TYPE=Release -S <path to CMakeLists.txt of your project> -B <build dir>
    cmake --build <build dir> --parallel
    ```
    
    Note
    
    OpenVINO can be used with any build interface, as long as it is supported by Conan 2.0. Read [more](https://docs.conan.io/2/examples/tools.html).

## Additional Resources

  - [Conan Package Manager](https://conan.io).
  - Learn more about \[OpenVINO Workflow\](../../../openvino-workflow.md).
  - To prepare your models for working with OpenVINO, see \[Model Preparation\](../../../openvino-workflow/model-preparation.md).
  - Learn more about \[Inference with OpenVINO Runtime\](../../../openvino-workflow/running-inference.md).
  - See sample applications in \[OpenVINO toolkit Samples Overview\](../../../get-started/learn-openvino/openvino-samples.md).
  - Check out the OpenVINO [product home page](https://software.intel.com/en-us/openvino-toolkit).
