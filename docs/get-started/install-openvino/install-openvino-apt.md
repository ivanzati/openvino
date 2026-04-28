---
sidebar_label: 'Install Intel® Distribution of OpenVINO™ Toolkit for Linux Using APT Repository'
format: md
---

# Install Intel® Distribution of OpenVINO™ Toolkit for Linux Using APT Repository

> system, using the APT repository.

Note

Note that the APT distribution:

  - offers both C/C++ and Python APIs
  - is dedicated to Linux users only
  - additionally includes code samples

Before installing OpenVINO, see the
\[System Requirements page\](../../../about-openvino/release-notes-openvino/system-requirements.md).

## Installing OpenVINO Runtime

### Step 1: Set Up the OpenVINO Toolkit APT Repository

1.  Install the GPG key for the repository
    
    1.  Download the [GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB](https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB)
        
        You can also use the following command:
        
        ``` sh
        wget https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB
        ```
    
    2.  Add this key to the system keyring:
        
        ``` sh
        sudo gpg --output /etc/apt/trusted.gpg.d/intel.gpg --dearmor GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB
        ```
        
        Note
        
        You might need to install GnuPG:
        
        ``` sh
        sudo apt-get install gnupg
        ```

2.  Add the repository via the following command:
    
    Ubuntu 24
    
    ``` sh
    echo "deb https://apt.repos.intel.com/openvino ubuntu24 main" | sudo tee /etc/apt/sources.list.d/intel-openvino.list
    ```
    
    Ubuntu 22
    
    ``` sh
    echo "deb https://apt.repos.intel.com/openvino ubuntu22 main" | sudo tee /etc/apt/sources.list.d/intel-openvino.list
    ```
    
    Ubuntu 20
    
    ``` sh
    echo "deb https://apt.repos.intel.com/openvino ubuntu20 main" | sudo tee /etc/apt/sources.list.d/intel-openvino.list
    ```

3.  Update the list of packages via the update command:
    
    ``` sh
    sudo apt update
    ```

4.  Verify that the APT repository is properly set up. Run the apt-cache command to see a list of all available OpenVINO packages and components:
    
    ``` sh
    apt-cache search openvino
    ```

### Step 2: Install OpenVINO Runtime Using the APT Package Manager

1.  Install OpenVINO Runtime

The Latest Version

Run the following command:

``` sh
sudo apt install openvino
```

A Specific Version

1.  Get a list of OpenVINO packages available for installation:
    
    ``` sh
    sudo apt-cache search openvino
    ```

2.  Install a specific version of an OpenVINO package:
    
    ``` sh
    sudo apt install openvino-<VERSION>.<UPDATE>.<PATCH>
    ```
    
    For example:
    
    ``` sh
    sudo apt install openvino-2026.0.0
    ```

Note

You can use `--no-install-recommends` option to install only required packages.
Keep in mind that the build tools must be installed **separately** if you want to compile the samples.

2.  Check for Installed Packages and Versions

Run the following command:

``` sh
apt list --installed | grep openvino
```

Congratulations\! You've just Installed OpenVINO\! For some use cases you may still
need to install additional components. Check the
\[list of additional configurations\](./configurations.md)
to see if your case needs any of them.

With the APT distribution, you can build OpenVINO sample files, as explained in the
\[guide for OpenVINO sample applications\](../../../get-started/learn-openvino/openvino-samples.md).
For C++ and C, just run the `build_samples.sh` script:

C++

``` sh
/usr/share/openvino/samples/cpp/build_samples.sh
```

C

``` sh
/usr/share/openvino/samples/c/build_samples.sh
```

Python samples can run as following:

``` sh
python3 /usr/share/openvino/samples/python/hello_query_device/hello_query_device.py
```

## Uninstalling OpenVINO Runtime

To uninstall OpenVINO Runtime via APT, run the following command based on your needs:

The Latest Version

``` sh
sudo apt autoremove openvino
```

A Specific Version

``` sh
sudo apt autoremove openvino-<VERSION>.<UPDATE>.<PATCH>
```

For example:

``` sh
sudo apt autoremove openvino-2026.0.0
```

## What's Next?

Now that you've installed OpenVINO Runtime, you're ready to run your own machine learning applications\!
Learn more about how to integrate a model in OpenVINO applications by trying out the following tutorials:

  - Try the \[C++ Quick Start Example\](../../../get-started/learn-openvino/openvino-samples/get-started-demos.md) for step-by-step
    instructions on building and running a basic image classification C++ application.
    
    ![image](https://user-images.githubusercontent.com/36741649/127170593-86976dc3-e5e4-40be-b0a6-206379cd7df5.jpg)

  - Visit the *Samples* page for other C++ example applications to get you started with OpenVINO, such as:
    
      - \[Basic object detection with the Hello Reshape SSD C++ sample\](../../../get-started/learn-openvino/openvino-samples/hello-reshape-ssd.md)
      - \[Object classification sample\](../../../get-started/learn-openvino/openvino-samples/hello-classification.md)

You can also try the following:

  - Learn more about \[OpenVINO Workflow\](../../../openvino-workflow.md).
  - To prepare your models for working with OpenVINO, see \[Model Preparation\](../../../openvino-workflow/model-preparation.md).
  - See pre-trained deep learning models on [Hugging Face](https://huggingface.co/OpenVINO)
  - Learn more about \[Inference with OpenVINO Runtime\](../../../openvino-workflow/running-inference.md).
  - See sample applications in \[OpenVINO toolkit Samples Overview\](../../../get-started/learn-openvino/openvino-samples.md).
  - Take a glance at the OpenVINO [product home page](https://software.intel.com/en-us/openvino-toolkit) .
