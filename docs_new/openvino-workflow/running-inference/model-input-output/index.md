---
sidebar_label: 'Model Input/Output'
format: md
---

# Model Input/Output

> outputs.

`ov::Model::inputs()` and `ov::Model::outputs()` methods retrieve vectors of all
input/output ports.

Note that a similar logic is applied to retrieving data using the `ov::InferRequest` methods.

Python

docs/articles\_en/assets/snippets/ov\_model\_snippets.py

C++

docs/articles\_en/assets/snippets/ov\_model\_snippets.cpp

`ov::Model::input()` and `ov::Model::output()` methods retrieve vectors of specific
input/output ports. To select the ports, you may use:

  - no arguments, if the model has only one input or output,

  - the index of inputs or outputs from the original model framework,
    
    Python
    
    ``` python
    ov_model_input = model.input(index)
    ov_model_output = model.output(index)
    ```
    
    C++
    
    ``` cpp
    auto ov_model_input = ov_model->input(index);
    auto ov_model_output = ov_model->output(index);
    ```

  - tensor names of inputs or outputs from the original model framework.
    
    Python
    
    ``` python
    ov_model_input = model.input(original_fw_in_tensor_name)
    ov_model_output = model.output(original_fw_out_tensor_name)
    ```
    
    C++
    
    ``` cpp
    auto ov_model_input = ov_model->input(original_fw_in_tensor_name);
    auto ov_model_output = ov_model->output(original_fw_out_tensor_name);
    ```

Since all `ov::Model` inputs and outputs are always numbered, using the index is the
recommended way. That is because the original frameworks do not necessarily require tensor
names, and so, `ov::Model` may contain an empty list of tensor\_names for inputs/outputs.
The `get_any_name` and `get_names` methods enable you to retrieve one or all tensor names
associated with an input/output. If the names are not present, the methods will
return empty names.

For information on how `ov::InferRequest` methods retrieve vectors of input output ports,
see the \[Inference Request\](inference-request.md) article.

For more details on how to work with model inputs and outputs, see other articles in this category:

  - \[Changing Input Shapes\](model-input-output/changing-input-shape.md)
  - \[Dynamic Shape Models\](model-input-output/dynamic-shapes.md)
  - \[String Tensors\](model-input-output/string-tensors.md)
