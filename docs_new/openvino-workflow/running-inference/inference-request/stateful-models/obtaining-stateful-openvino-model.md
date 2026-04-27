---
sidebar_label: 'Obtaining a Stateful OpenVINO Model'
format: md
---

# Obtaining a Stateful OpenVINO Model

If the original framework does not offer a dedicated API for working with states, the
resulting OpenVINO IR model will not be stateful by default. This means it will not contain
either a state or the \[Assign\](../../../../documentation/openvino-ir-format/operation-sets/operation-specs/infrastructure/assign-6.md) and
\[ReadValue\](../../../../documentation/openvino-ir-format/operation-sets/operation-specs/infrastructure/read-value-6.md) operations. You can still
make such models stateful (\[see benefits\](../stateful-models.md)),
and you have three ways to do it:

  - [Optimum-Intel](https://github.com/huggingface/optimum-intel) - an automated solution
    applicable to a selection of models (not covered by this article, for a usage guide
    refer to the \[LLM Inference with Hugging Face and Optimum Intel\](../../../../openvino-workflow-generative.md) article).
  - *MakeStateful transformation* - to choose which pairs of
    Parameter and Result to replace.
  - *LowLatency2 transformation* - to detect and replace Parameter
    and Result pairs connected to hidden and cell state inputs of LSTM/RNN/GRU operations
    or Loop/TensorIterator operations.

## MakeStateful Transformation

The MakeStateful transformation changes the structure of the model by replacing the
user-defined pairs of Parameter and Results with the Assign and ReadValue operations:

![diagram of MakeStateful Transformation](img/make_stateful_simple.svg)

**Only strict syntax is supported**. As shown in the example below, the transformation call
must be enclosed in double quotes "MakeStateful\[...\]", tensor names - in single quotes
without spaces 'tensor\_name\_1'.

**State naming rule**: in most cases, the name of a state is a concatenation of the
Parameter/Result tensor names. If there are no tensor names,
\[friendly names\](../../../../documentation/openvino-extensibility/transformation-api.md) are used.

**Examples:**

![detailed diagram of MakeStateful Transformation](img/make_stateful_detailed.png)

Python

Using tensor names

docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.py

Using Parameter/Result operations

docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.py

C++

Using tensor names

docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.cpp

Using Parameter/Result operations

docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.cpp

command line

Using tensor names

``` sh
--input_model <INPUT_MODEL> --transform "MakeStateful[param_res_names={'tensor_name_1':'tensor_name_4','tensor_name_3':'tensor_name_6'}]"
```

## LowLatency2 Transformation

The LowLatency2 transformation changes the structure of a model containing
\[TensorIterator\](../../../../documentation/openvino-ir-format/operation-sets/operation-specs/infrastructure/tensor-iterator-1.md)
and \[Loop\](../../../../documentation/openvino-ir-format/operation-sets/operation-specs/infrastructure/loop-5.md) by automatically detecting
and replacing pairs of Parameter and Results with the Assign and ReadValue operations,
as illustrated by the following example:

![diagram of LowLatency Transformation](img/applying_low_latency_2.svg)

After applying the transformation, ReadValue operations can receive other operations as
input, as shown in the picture above. These inputs should set the initial value for the
initialization of ReadValue operations. However, such initialization is not supported in
the current State API implementation. Input values are ignored, and the initial values
for the ReadValue operations are set to zeros unless the user specifies otherwise via
\[State API\](../stateful-models.md).

To apply LowLatency2 Transformation, follow the instruction below:

1.  Get \[ov::Model\](../../model-representation.md),
    for example:
    
    Python
    
    docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.py
    
    C++
    
    docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.cpp

2.  Change the number of iterations inside TensorIterator/Loop nodes in the model using the
    \[Reshape\](../../model-input-output/changing-input-shape.md) feature.
    
    For example, the *sequence\_lengths* dimension of the model input > 1, it means the
    TensorIterator layer has the number\_of\_iterations > 1. You can reshape the model
    inputs to set the *sequence\_dimension* to exactly 1.
    
    Python
    
    docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.py
    
    C++
    
    docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.cpp
    
    **Unrolling**: If the LowLatency2 transformation is applied to a model containing
    TensorIterator/Loop nodes with exactly one iteration inside, these nodes are unrolled.
    Otherwise, the nodes remain as they are. See the picture above for more details.

3.  Apply LowLatency2 transformation.
    
    Python
    
    docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.py
    
    C++
    
    docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.cpp
    
    (Optional) Use Const Initializer argument:
    
    By default, the LowLatency2 transformation inserts a constant subgraph of the same shape
    as the previous input node. The initializing value for ReadValue nodes is set to zero.
    For more information, see the picture below. You can disable the insertion of this subgraph
    by setting the `use_const_initializer` argument to `false`.
    
    Python
    
    docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.py
    
    C++
    
    docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.cpp
    
    ![diagram of constant subgraph initialization](img/llt2_use_const_initializer.svg)
    
    **State naming rule:** the name of a state is a concatenation of several names: the
    original TensorIterator operation, the parameter of the body, and an additional suffix
    `"variable_"` + id (zero-based indexing, new indexing for each TensorIterator). You can
    use these rules to predict the name of the inserted state after applying the transformation.
    For example:
    
    Python
    
    docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.py
    
    C++
    
    docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.cpp

4.  Use state API. See sections \[OpenVINO State API\](../stateful-models.md),
    *Stateful Model Inference*.
    
    ![diagram showing low latency limitation](img/low_latency_limitation_2.svg)
    
    The only way to change the number iterations of TensorIterator/Loop layer is to use the
    \[Reshape\](../../model-input-output/changing-input-shape.md) feature. However, some models may be
    non-reshapable, typically because the value of shapes is hardcoded in a constant
    somewhere in the model.
    
    In such a case, trim non-reshapable layers via
    \[Conversion Parameters\](../../../model-preparation/conversion-parameters.md):
    `--input` and `--output`. For example, check the [OpenVINO Model Conversion Tutorial](https://github.com/openvinotoolkit/openvino_notebooks/tree/latest/notebooks/convert-to-openvino).
    
    As for the parameter and the problematic constant in the picture above, it can be
    trimmed by using the `--input Reshape_layer_name` command-line option. The problematic
    constant can be also replaced using OpenVINO, as shown in the following example:
    
    Python
    
    docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.py
    
    C++
    
    docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.cpp

## Stateful Model from Scratch

The main approach to obtaining stateful OpenVINO IR models is converting from other
frameworks. Nonetheless, it is possible to create a model from scratch. Check how to
do so in the \[Build OpenVINO Model section\](../../model-representation.md).

Here is also an example of how `ov::SinkVector` is used to create `ov::Model`. For a
model with states, except inputs and outputs, `Assign` nodes should also point to `Model`
to avoid deleting it during graph transformations. You can do it with the constructor, as in
the example, or with the add\_sinks(const SinkVector& sinks) method. Also, you can delete
a sink from ov::Model after deleting the node from the graph with the delete\_sink() method.

Python

docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.py

C++

docs/articles\_en/assets/snippets/ov\_stateful\_models\_intro.cpp

Note

**ONNX and frameworks supported via ONNX format:** *LSTM, RNN, GRU* original layers are
converted to the GRU/RNN/LSTM Sequence operations. *ONNX Loop* layer is converted to the
OpenVINO Loop operation.

**TensorFlow:** *BlockLSTM* is converted to a TensorIterator operation. The TensorIterator
body contains LSTM Cell operation. Modifications such as Peepholes and InputForget are
not supported. The *While* layer is converted to a TensorIterator. The TensorIterator body
can contain any supported operations. However, dynamic cases where the count of iterations
cannot be calculated during shape inference are not supported.

**TensorFlow2:** *While* layer is converted to a Loop operation. The Loop body can contain
any supported operations.
