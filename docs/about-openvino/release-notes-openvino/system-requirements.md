---
sidebar_label: 'System Requirements'
format: md
---

# System Requirements

Note

Certain hardware, including but not limited to GPU and NPU, requires manual installation of
specific drivers and/or other software components to work correctly and/or to utilize
hardware capabilities at their best. This might require updates to the operating
system, including but not limited to Linux kernel, please refer to their documentation
for details. These modifications should be handled by user and are not part of OpenVINO
installation.

## CPU

Supported Hardware

  - Intel® Core™ Ultra Series 1, Series 2 and Series 3
  - Intel® Xeon® 6 processor
  - Intel Atom® Processor X Series
  - Intel Atom® processor with Intel® SSE4.2 support
  - Intel® Pentium® processor N4200/5, N3350/5, N3450/5 with Intel® HD Graphics
  - 6th - 14th generation Intel® Core™ processors
  - 1st - 5th generation Intel® Xeon® Scalable Processors
  - ARM CPUs with armv7a and higher, ARM64 CPUs with arm64-v8a and higher, Apple® Mac with Apple silicon

Supported Operating Systems

  - Windows 11, 64-bit
  - Windows 10, 64-bit
  - Ubuntu 24.04 long-term support (LTS), 64-bit (Kernel 6.8+)
  - Ubuntu 22.04 long-term support (LTS), 64-bit (Kernel 5.15+)
  - Ubuntu 20.04 long-term support (LTS), 64-bit (Kernel 5.15+)
  - macOS 12.6 and above, 64-bit and ARM64
  - CentOS 7
  - Red Hat Enterprise Linux (RHEL) 8 and 9, 64-bit
  - openSUSE Tumbleweed, 64-bit and ARM64
  - Ubuntu 20.04 ARM64

## GPU

Supported Hardware

  - Intel® Arc™ GPU Series
  - Intel® HD Graphics
  - Intel® UHD Graphics
  - Intel® Iris® Pro Graphics
  - Intel® Iris® Xe Graphics
  - Intel® Iris® Xe Max Graphics
  - Intel® Data Center GPU Flex Series
  - Intel® Data Center GPU Max Series

Supported Operating Systems

  - Windows 11, 64-bit
  - Windows 10, 64-bit
  - Ubuntu 24.04 long-term support (LTS), 64-bit
  - Ubuntu 22.04 long-term support (LTS), 64-bit
  - Ubuntu 20.04 long-term support (LTS), 64-bit
  - CentOS 7
  - Red Hat Enterprise Linux (RHEL) 8 and 9, 64-bit

Additional considerations

  - The use of GPU requires drivers that are not included in the Intel®
    Distribution of OpenVINO™ toolkit package.
  - Processor graphics are not included in all processors. See
    [Product Specifications](https://ark.intel.com/)
    for information about your processor.
  - While this release of OpenVINO supports Ubuntu 20.04, the driver stack
    for Intel discrete graphic cards does not fully support Ubuntu 20.04.
    We recommend using Ubuntu 22.04 and later when executing on discrete graphics.
  - OpenCL™ driver versions required may vary, depending on hardware and operating Systems
    used. Consult driver documentation to select the best version for your setup.

## Intel® Neural Processing Unit

Operating Systems for NPU

  - Ubuntu 24.04 long-term support (LTS), 64-bit (preview support)
  - Ubuntu 22.04 long-term support (LTS), 64-bit
  - Windows 11, 64-bit (22H2 and later)

Additional considerations

  - These Accelerators require \[drivers\](../../get-started/install-openvino/configurations/configurations-intel-npu.md)
    that are not included in the Intel® Distribution of OpenVINO™ toolkit package.
  - Users can access the NPU plugin through the OpenVINO archives on
    the \[download page\](../../get-started/install-openvino.md).

## Operating systems and developer environment

Linux OS

  - Ubuntu 24.04 with Linux kernel 6.8+
  - Ubuntu 22.04 with Linux kernel 5.15+
  - Ubuntu 20.04 with Linux kernel 5.15+
  - Red Hat Enterprise Linux 9.3-9.4 with Linux kernel 5.4

Build environment components:

  - Python 3.10-3.14
  - [Intel® HD Graphics Driver](https://downloadcenter.intel.com/product/80939/Graphics-Drivers)
    required for inference on GPU
  - GNU Compiler Collection and CMake are needed for building from source:
      - [GNU Compiler Collection (GCC)](https://www.gnu.org/software/gcc/) 7.5 and above
      - [CMake](https://cmake.org/download/) 3.13 or higher

Higher versions of kernel might be required for 10th Gen Intel® Core™ Processors and above,
Intel® Core™ Ultra Processors, 4th Gen Intel® Xeon® Scalable Processors and above
to support CPU, GPU, NPU or hybrid-cores CPU capabilities.

Windows 10 and 11

OpenVINO Runtime requires certain C++ libraries to operate. To execute ready-made apps,
the libraries distributed by [Visual Studio redistributable package](https://aka.ms/vs/17/release/vc_redist.x64.exe)
are suggested. For development and compilation of OpenVINO-integrated apps, the build
environment components are required instead.

Build environment components:

  - [Microsoft Visual Studio 2019 or later](https://visualstudio.microsoft.com/downloads/)
  - [CMake](https://cmake.org/download/) 3.16 or higher
  - [Python](https://www.python.org/downloads/) 3.10-3.14
  - [Intel® HD Graphics Driver](https://downloadcenter.intel.com/product/80939/Graphics-Drivers)
    required for inference on GPU

macOS

  - macOS 12.6 and above

Build environment components:

  - [Xcode](https://developer.apple.com/xcode/) 10.3
  - [CMake](https://cmake.org/download/) 3.13 or higher
  - [Python](https://www.python.org/downloads/) 3.10-3.14

DL framework versions:

  - TensorFlow 1.15.5 - 2.17
  - PyTorch 2.4
  - ONNX 1.16
  - PaddlePaddle 2.6
  - JAX 0.4.31 (via a path of jax2tf with native\_serialization=False)

This package can be installed on other versions of DL Frameworks
but only the versions specified here are fully validated.

Note

OpenVINO Python binaries are built with and redistribute oneTBB libraries.

## OpenVINO Distributions

Different OpenVINO distributions may support slightly different sets of features.
Read installation guides for particular distributions for more details.
Refer to the \[OpenVINO Release Policy\](../../../about-openvino/release-notes-openvino/release-policy.md)
to learn more about the release types.

Archive

Linux

  - [CMake 3.13 or higher, 64-bit](https://cmake.org/download/)

  - [Python 3.10 - 3.14, 64-bit](https://www.python.org/downloads/)

  - GCC:
    
    Ubuntu
    
      - GCC 9.3.0 (for Ubuntu 20.04), GCC 11.3.0 (for Ubuntu 22.04) or GCC 13.2.0 (for Ubuntu 24.04)
    
    RHEL 8
    
      - GCC 8.4.1
    
    CentOS 7
    
      - GCC 8.3.1
        
        Use the following instructions to install it:
        
        Install GCC 8.3.1 via devtoolset-8
        
        ``` sh
        sudo yum update -y && sudo yum install -y centos-release-scl epel-release
        sudo yum install -y devtoolset-8
        ```
        
        Enable devtoolset-8 and check current gcc version
        
        ``` sh
        source /opt/rh/devtoolset-8/enable
        gcc -v
        ```

macOS

  - [CMake 3.13 or higher](https://cmake.org/download/) (choose "macOS 10.13 or later"). Add `/Applications/CMake.app/Contents/bin` to path (for default install).
  - [Python 3.10 - 3.14](https://www.python.org/downloads/mac-osx/) (choose 3.10 - 3.14). Install and add to path.
  - Apple Xcode Command Line Tools. In the terminal, run `xcode-select --install` from any directory
  - (Optional) Apple Xcode IDE (not required for OpenVINO™, but useful for development)

Windows

  - [C++ libraries (included in Visual Studio redistributable)](https://aka.ms/vs/17/release/vc_redist.x64.exe) (a core dependency for OpenVINO Runtime)
  - [Microsoft Visual Studio 2019 or later](http://visualstudio.microsoft.com/downloads/) (for development and app compilation with OpenVINO)
  - [CMake 3.14 or higher, 64-bit](https://cmake.org/download/) (optional, only required for building sample applications)
  - [Python 3.10 - 3.14, 64-bit](https://www.python.org/downloads/windows/)

Note

To install Microsoft Visual Studio, follow the [Microsoft Visual Studio installation guide](https://docs.microsoft.com/en-us/visualstudio/install/install-visual-studio?view=vs-2022).
You can choose to download the Community version. During installation in the **Workloads** tab, choose **Desktop development with C++**.

Note

You can either use cmake\<version>.msi which is the installation wizard or cmake\<version>.zip where you have to go into the bin folder and then manually add the path to environmental variables.

Important

When installing Python, make sure you click the option **Add Python 3.x to PATH** to [add Python](https://docs.python.org/3/using/windows.html#installation-steps) to your PATH environment variable.

APT

Linux

  - [CMake 3.13 or higher, 64-bit](https://cmake.org/download/)
  - GCC 9.3.0 (for Ubuntu 20.04), GCC 11.3.0 (for Ubuntu 22.04) or GCC 13.2.0 (for Ubuntu 24.04)
  - [Python 3.10 - 3.14, 64-bit](https://www.python.org/downloads/)

Homebrew

Linux

  - [Homebrew](https://brew.sh/)
  - [CMake 3.13 or higher, 64-bit](https://cmake.org/download/)
  - GCC 9.3.0 (for Ubuntu 20.04), GCC 11.3.0 (for Ubuntu 22.04) or GCC 13.2.0 (for Ubuntu 24.04)
  - [Python 3.10 - 3.14, 64-bit](https://www.python.org/downloads/)

macOS

  - [Homebrew](https://brew.sh/)
  - [CMake 3.13 or higher](https://cmake.org/download/) (choose "macOS 10.13 or later"). Add `/Applications/CMake.app/Contents/bin` to path (for default installation).
  - [Python 3.10 - 3.14](https://www.python.org/downloads/mac-osx/) . Install and add it to path.
  - Apple Xcode Command Line Tools. In the terminal, run `xcode-select --install` from any directory to install it.
  - (Optional) Apple Xcode IDE (not required for OpenVINO™, but useful for development)

npm

Linux

All x86\_64 / arm64 architectures are supported.

  - [Node.js version 21.0.0 and higher](https://nodejs.org/en/download/package-manager)

macOS

All x86\_64 / arm64 architectures are supported, however, only for CPU inference.

  - [Node.js version 21.0.0 and higher](https://nodejs.org/en/download/package-manager)

Windows

All x86\_64 architectures are supported. Windows ARM is not supported.

  - [Node.js version 21.0.0 and higher](https://nodejs.org/en/download/package-manager/)

YUM

Linux

OpenVINO RPM packages are compatible with and can be run on the following operating systems:

  - RHEL 8.2 and higher
  - Amazon Linux 2022 and 2023
  - Rocky Linux 8.7, 8.8 and 9.2-9.3
  - Alma Linux 8.7, 8.8 and 9.2-9.4
  - Oracle Linux 8.7, 8.8 and 9.2-9.4
  - Fedora 29 and higher up to 41
  - OpenEuler 20.03, 22.03, 23.03 and 24.03
  - Anolis OS 8.6 and 8.8
  - CentOS Stream 8 and 9

Software:

  - [CMake 3.13 or higher, 64-bit](https://cmake.org/download/)
  - GCC 8.4.1
  - [Python 3.10 - 3.14, 64-bit](https://www.python.org/downloads/)

ZYPPER

Linux

OpenVINO RPM packages are compatible with and can be run on openSUSE Tumbleweed only.

Software:

  - [CMake 3.13 or higher, 64-bit](https://cmake.org/download/)
  - GCC 8.2.0
  - [Python 3.10 - 3.14, 64-bit](https://www.python.org/downloads/)

The claims stated here may not apply to all use cases and setups. See
\[Legal notices and terms of use\](../additional-resources/terms-of-use.md) for more information.
