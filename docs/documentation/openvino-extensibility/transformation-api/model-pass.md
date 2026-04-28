---
sidebar_label: 'OpenVINO Model Pass'
format: md
---

# OpenVINO Model Pass

> ov::Model as input and process it.

`ov::pass::ModelPass` is used for transformations that take entire `ov::Model` as an input and process it.

Template for ModelPass transformation class

docs/articles\_en/assets/snippets/template\_model\_transformation.hpp

C++

docs/articles\_en/assets/snippets/template\_model\_transformation.cpp

Python

docs/articles\_en/assets/snippets/ov\_model\_pass.py

Using `ov::pass::ModelPass`, you need to override the `run_on_model` method where you will write the transformation code.
Return value is `true` if the original model has changed during transformation (new operation was added, or operations replacement was made, or node attributes were changed); otherwise, it is `false`.
Also `ov::pass::ModelPass` based transformations can be executed via `ov::pass::Manager`.

## See Also

  - \[OpenVINO™ Transformations\](../transformation-api.md)
