---
sidebar_label: 'Debugging Auto-Device Plugin'
format: md
---

# Debugging Auto-Device Plugin

> Profiler to control trace data during execution of AUTO plugin.

## Using Debug Log

In case of execution problems, just like all other plugins, Auto-Device provides the user with information on exceptions and error values. If the returned data is not enough for debugging purposes, more information may be acquired by means of `ov::log::Level`.

There are six levels of logs, which can be called explicitly or set via the `OPENVINO_LOG_LEVEL` environment variable (can be overwritten by `compile_model()` or `set_property()`):

0 - `ov::log::Level::NO`
1 - `ov::log::Level::ERR`
2 - `ov::log::Level::WARNING`
3 - `ov::log::Level::INFO`
4 - `ov::log::Level::DEBUG`
5 - `ov::log::Level::TRACE`

Python

docs/articles\_en/assets/snippets/ov\_auto.py

C++

docs/articles\_en/assets/snippets/AUTO6.cpp

OS environment variable

``` sh
When defining it via the variable,
a number needs to be used instead of a log level name, e.g.:

Linux
export OPENVINO_LOG_LEVEL=0

Windows
set OPENVINO_LOG_LEVEL=0
```

The property returns information in the following format:

``` sh
[time]LOG_LEVEL[file] [PLUGIN]: message
```

in which the `LOG_LEVEL` is represented by the first letter of its name (ERROR being an exception and using its full name). For example:

``` sh
[17:09:36.6188]D[plugin.cpp:167] deviceName:GPU, defaultDeviceID:, uniqueName:GPU_
[17:09:36.6242]I[executable_network.cpp:181] [AUTOPLUGIN]:select device:GPU
[17:09:36.6809]ERROR[executable_network.cpp:384] [AUTOPLUGIN] load failed, GPU:[ GENERAL_ERROR ]
```

## Instrumentation and Tracing Technology

All major performance calls of both OpenVINO™ Runtime and the AUTO plugin are instrumented with Instrumentation and Tracing Technology (ITT) APIs. ITT is enabled by default with BASE level tracing. For full instrumentation, compile with the following option:

``` sh
-DENABLE_PROFILING_ITT=FULL
```

For more information, you can refer to:

  - [Intel® VTune™ Profiler User Guide](https://www.intel.com/content/www/us/en/docs/vtune-profiler/user-guide/2023-0/instrumentation-and-tracing-technology-apis.html)

### Analyze Code Performance on Linux

You can analyze code performance using Intel® VTune™ Profiler. For more information and
installation instructions refer to the
[Intel® VTune™ Profiler User Guide](https://www.intel.com/content/www/us/en/docs/vtune-profiler/user-guide/2023-0/instrumentation-and-tracing-technology-apis.html)
With Intel® VTune™ Profiler installed you can configure your analysis with the following steps:

1.  Open Intel® VTune™ Profiler GUI on the host machine with the following command:
    
    ``` sh
    cd /vtune install dir/intel/oneapi/vtune/2021.6.0/env
    source vars.sh
    vtune-gui
    ```

2.  Select **Configure Analysis**

3.  In the **where** pane, select **Local Host**
    
    ![image](img/OV_UG_supported_plugins_AUTO_debugging-img01-localhost.png)

4.  In the **what** pane, specify your target application/script on the local system.
    
    ![image](img/OV_UG_supported_plugins_AUTO_debugging-img02-launch.png)

5.  In the **how** pane, choose and configure the analysis type you want to perform, for example, **Hotspots Analysis**: identify the most time-consuming functions and drill down to see time spent on each line of source code. Focus optimization efforts on hot code for the greatest performance impact.
    
    ![image](img/OV_UG_supported_plugins_AUTO_debugging-img03-hotspots.png)

6.  Start the analysis by clicking the start button. When it is done, you will get a summary of the run, including top hotspots and top tasks in your application:
    
    ![image](img/OV_UG_supported_plugins_AUTO_debugging-img04-vtunesummary.png)

7.  To analyze ITT info related to the Auto plugin, click on the **Bottom-up** tab, choose the **Task Domain/Task Type/Function/Call Stack** from the dropdown list - Auto plugin-related ITT info is under the MULTIPlugin task domain:
    
    ![image](img/OV_UG_supported_plugins_AUTO_debugging-img05-vtunebottomup.png)
