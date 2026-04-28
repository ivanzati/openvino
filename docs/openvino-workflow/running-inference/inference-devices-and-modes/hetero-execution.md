---
sidebar_label: 'Heterogeneous Execution'
format: md
---

# Heterogeneous Execution

> the inference of one model on several computing devices.

Heterogeneous execution enables executing inference of one model on several devices.
Its purpose is to:

  - Utilize the power of accelerators to process the heaviest parts of the model and to execute
    unsupported operations on fallback devices, like the CPU.
  - Utilize all available hardware more efficiently during one inference.

Execution via the heterogeneous mode can be divided into two independent steps:

1.  Setting hardware affinity to operations ([ov::Core::query\_model](https://docs.openvino.ai/2026/api/c_cpp_api/classov_1_1_core.html#class-ov-core) is used internally by the Hetero device).

2.  Compiling a model to the Heterogeneous device assumes splitting the model to parts, compiling them on the specified devices (via [ov::device::priorities](https://docs.openvino.ai/2026/api/c_cpp_api/structov_1_1device_1_1_priorities.html)), and executing them in the Heterogeneous mode. The model is split to subgraphs in accordance with the affinities, where a set of connected operations with the same affinity is to be a dedicated subgraph. Each subgraph is compiled on a dedicated device and multiple [ov::CompiledModel](https://docs.openvino.ai/2026/api/c_cpp_api/classov_1_1_compiled_model.html#class-ov-compiledmodel) objects are made, which are connected via automatically allocated intermediate tensors.
    
    If you set pipeline parallelism (via `ov::hint::model_distribution_policy`), the model is split into multiple stages, and each stage is assigned to a different device. The output of one stage is fed as input to the next stage.

These two steps are not interconnected and affinities can be set in one of two ways, used separately or in combination (as described below): in the `manual` or the `automatic` mode.

## Defining and configuring the Hetero device

  - Following the OpenVINO™ naming convention, the Hetero execution plugin is assigned the label of  
    `"HETERO".` It may be defined with no additional parameters, resulting in defaults being used,
    or configured further with the following setup options:

```html
<table>
<colgroup>
<col style="width: 26%" />
<col style="width: 36%" />
<col style="width: 36%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter Name &amp; C++ property</th>
<th>Property values</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><div class="line-block">"MULTI_DEVICE_PRIORITIES"<br />
<code>ov::device::priorities</code><br />
</div></td>
<td><div class="line-block"><code>HETERO: &lt;device names&gt;</code><br />
<br />
comma-separated, no spaces</div></td>
<td><div class="line-block">Lists the devices available for selection.<br />
The device sequence will be taken as priority<br />
from high to low.</div></td>
</tr>
<tr class="even">
<td><div class="line-block"><br />
"MODEL_DISTRIBUTION_POLICY"<br />
<br />
<code>ov::hint::model_distribution_policy</code><br />
</div></td>
<td><div class="line-block"><code>empty</code><br />
<code>ov::hint::ModelDistributionPolicy::PIPELINE_PARALLEL</code><br />
<br />
HETERO only supports PIPELINE_PARALLEL, The default value<br />
is empty</div></td>
<td><div class="line-block">Model distribution policy for inference with<br />
multiple devices. Distributes the model to multiple<br />
devices during model compilation.<br />
<br />
</div></td>
</tr>
</tbody>
</table>
```

## Manual and Automatic Modes for Assigning Affinities

### The Manual Mode

It assumes setting affinities explicitly for all operations in the model using [ov::Node::get\_rt\_info](https://docs.openvino.ai/2026/api/c_cpp_api/classov_1_1_node.html#class-ov-node) with the `"affinity"` key.

If you assign specific operation to a specific device, make sure that the device actually supports the operation.
Randomly selecting operations and setting affinities may lead to decrease in model accuracy. To avoid that, try to set the related operations or subgraphs of this operation to the same affinity, such as the constant operation that will be folded into this operation.

Python

docs/articles\_en/assets/snippets/ov\_hetero.py

C++

docs/articles\_en/assets/snippets/ov\_hetero.cpp

### Automatic Mode

#### Without Pipeline Parallelism

It decides automatically which operation is assigned to which device according to the support from dedicated devices (`GPU`, `CPU`, etc.) and query model step is called implicitly by Hetero device during model compilation.

The automatic mode causes "greedy" behavior and assigns all operations that can be executed on a given device to it, according to the priorities you specify (for example, `ov::device::priorities("GPU,CPU")`).
It does not take into account device peculiarities such as the inability to infer certain operations without other special operations placed before or after that layer. If the device plugin does not support the subgraph topology constructed by the HETERO device, then you should set affinity manually.

Python

docs/articles\_en/assets/snippets/ov\_hetero.py

C++

docs/articles\_en/assets/snippets/ov\_hetero.cpp

#### Pipeline Parallelism (Preview)

Pipeline parallelism is set via `ov::hint::model_distribution_policy`. This mode is an efficient technique for inferring large models on multiple devices. The model is divided into multiple stages, with each stage assigned to a different device (`dGPU`, `iGPU`, `CPU`, etc.) in the sequence of device priority. This mode estimates memory size required by operations (includes weights memory and runtime memory), assigns operations (stage) to each device per the available memory size and considering the minimal data transfer between devices. Different stages are executed in sequence of model flow.

Note

Since iGPU and CPU share the host memory and host resource should be always considered as a fallback, it is recommended to use at most one of the iGPU or CPU and put it at the end of device list.

For large models that do not fit on a single first-priority device, model pipeline parallelism is employed. This technique distributes certain parts of the model across different devices, ensuring that each device has enough memory to infer the operations.

Python

docs/articles\_en/assets/snippets/ov\_hetero.py

C++

docs/articles\_en/assets/snippets/ov\_hetero.cpp

### Using Manual and Automatic Modes in Combination

In some cases you may need to consider manually adjusting affinities which were set automatically. It usually serves minimizing the number of total subgraphs to optimize memory transfers. To do it, you need to "fix" the automatically assigned affinities like so:

Python

docs/articles\_en/assets/snippets/ov\_hetero.py

C++

docs/articles\_en/assets/snippets/ov\_hetero.cpp

Importantly, the automatic mode will not work if any operation in a model has its `"affinity"` already initialized.

Note

[ov::Core::query\_model](https://docs.openvino.ai/2026/api/c_cpp_api/classov_1_1_core.html#_CPPv4NK2ov4Core11query_modelERKNSt10shared_ptrIKN2ov5ModelEEERKNSt6stringERK6AnyMap) does not depend on affinities set by a user. Instead, it queries for an operation support based on device capabilities.

## Configure fallback devices

If you want different devices in Hetero execution to have different device-specific configuration options, you can use the special helper property [ov::device::properties](https://docs.openvino.ai/2026/api/c_cpp_api/structov_1_1device_1_1_properties.html#struct-ov-device-properties):

Python

docs/articles\_en/assets/snippets/ov\_hetero.py

C++

docs/articles\_en/assets/snippets/ov\_hetero.cpp

In the example above, the `GPU` device is configured to enable profiling data and uses the default execution precision, while `CPU` has the configuration property to perform inference in `fp32`.

## Handling of Difficult Topologies

Some topologies are not friendly to heterogeneous execution on some devices, even to the point of being unable to execute.
For example, models having activation operations that are not supported on the primary device are split by Hetero into multiple sets of subgraphs which leads to suboptimal execution.
If transmitting data from one subgraph to another part of the model in the heterogeneous mode takes more time than under normal execution, heterogeneous execution may be unsubstantiated.
In such cases, you can define the heaviest part manually and set the affinity to avoid sending data back and forth many times during one inference.

## Analyzing Performance of Heterogeneous Execution

After enabling the `OPENVINO_HETERO_VISUALIZE` environment variable, you can dump GraphViz `.dot` files with annotations of operations per devices.

The Heterogeneous execution mode can generate two files:

  - `hetero_affinity_<model name>.dot` - annotation of affinities per operation.
  - `hetero_subgraphs_<model name>.dot` - annotation of affinities per graph.

You can use the GraphViz utility or a file converter to view the images. On the Ubuntu operating system, you can use xdot:

  - `sudo apt-get install xdot`
  - `xdot hetero_subgraphs.dot`

You can use performance data (in sample applications, it is the option `-pc`) to get the performance data on each subgraph.

Here is an example of the output for Googlenet v1 running on HDDL (device no longer supported) with fallback to CPU:

``` sh
subgraph1: 1. input preprocessing (mean data/HDDL):EXECUTED layerType:          realTime: 129   cpu: 129  execType:
subgraph1: 2. input transfer to DDR:EXECUTED                layerType:          realTime: 201   cpu: 0    execType:
subgraph1: 3. HDDL execute time:EXECUTED                    layerType:          realTime: 3808  cpu: 0    execType:
subgraph1: 4. output transfer from DDR:EXECUTED             layerType:          realTime: 55    cpu: 0    execType:
subgraph1: 5. HDDL output postprocessing:EXECUTED           layerType:          realTime: 7     cpu: 7    execType:
subgraph1: 6. copy to IE blob:EXECUTED                      layerType:          realTime: 2     cpu: 2    execType:
subgraph2: out_prob:          NOT_RUN                       layerType: Output   realTime: 0     cpu: 0    execType: unknown
subgraph2: prob:              EXECUTED                      layerType: SoftMax  realTime: 10    cpu: 10   execType: ref
Total time: 4212 microseconds
```

## Sample Usage

OpenVINO™ sample programs can use the Heterogeneous execution used with the `-d` option:

``` sh
./hello_classification <path_to_model>/squeezenet1.1.xml <path_to_pictures>/picture.jpg HETERO:GPU,CPU
```

where:

  - `HETERO` stands for the Heterogeneous execution
  - `GPU,CPU` points to a fallback policy with the priority on GPU and fallback to CPU

You can also point to more than two devices: `-d HETERO:GPU,CPU`

## Additional Resources

  - \[Inference Devices and Modes\](../inference-devices-and-modes.md)
