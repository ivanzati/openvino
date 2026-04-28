---
sidebar_label: 'Install Intel® Distribution of OpenVINO™ Toolkit from npm Registry'
format: md
---

# Install Intel® Distribution of OpenVINO™ Toolkit from npm Registry

> macOS operating systems, using the npm registry.

Note

Note that the npm distribution:

  - offers the JavaScript API only
  - is dedicated to users of all major OSes: Windows, Linux, and macOS
    (all x86\_64 / arm64 architectures)
  - macOS offers support only for CPU inference

Before installing OpenVINO, see the
\[System Requirements page\](../../../about-openvino/release-notes-openvino/system-requirements.md).

## Installing OpenVINO Node.js

1.  Make sure that you have installed [Node.js and npm](https://nodejs.org/en/download)
    on your system.

2.  Navigate to your project directory and run the following command in the terminal:
    
    ``` sh
    npm install openvino-node
    ```

Note

The *openvino-node* npm package runs in Node.js environment only and provides
a subset of [OpenVINO Runtime C++ API](https://docs.openvino.ai/2026/api/c_cpp_api/group__ov__cpp__api.html).

## What's Next?

Now that you’ve installed OpenVINO npm package, you’re ready to run your own machine
learning applications\! Explore \[OpenVINO Node.js API\](../../api/nodejs\_api/nodejs\_api.md)
to learn more about how to integrate a model in Node.js applications.

## Additional Resources

  - Intel® Distribution of OpenVINO™ toolkit home page: [https://software.intel.com/en-us/openvino-toolkit](https://software.intel.com/en-us/openvino-toolkit)
