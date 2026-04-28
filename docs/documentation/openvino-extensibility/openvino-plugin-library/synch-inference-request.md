---
sidebar_label: 'Synchronous Inference Request'
format: md
---

# Synchronous Inference Request

`InferRequest` class functionality:

  - Allocate input and output tensors needed for a backend-dependent network inference.
  - Define functions for inference process stages (for example, `preprocess`, `upload`, `infer`, `download`, `postprocess`). These functions can later be used to define an execution pipeline during \[Asynchronous Inference Request\](asynch-inference-request.md) implementation.
  - Call inference stages one by one synchronously.

## InferRequest Class

OpenVINO Plugin API provides the interface ov::ISyncInferRequest which should be
used as a base class for a synchronous inference request implementation. Based of that, a declaration
of a synchronous request class can look as follows:

src/plugins/template/src/sync\_infer\_request.hpp

### Class Fields

The example class has several fields:

  - `m_profiling_task` - array of the `std::array<openvino::itt::handle_t, numOfStages>` type. Defines names for pipeline stages. Used to profile an inference pipeline execution with the Intel® instrumentation and tracing technology (ITT).
  - `m_durations` - array of durations of each pipeline stage.
  - backend-specific fields:
      - `m_backend_input_tensors` - input backend tensors.
      - `m_backend_output_tensors` - output backend tensors.
      - `m_executable` - an executable object / backend computational graph.
      - `m_eval_context` - an evaluation context to save backend states after the inference.
      - `m_variable_states` - a vector of variable states.

### InferRequest Constructor

The constructor initializes helper fields and calls methods which allocate tensors:

src/plugins/template/src/sync\_infer\_request.cpp

Note

Use inputs/outputs information from the compiled model to understand shape and element type of tensors, which you can set with ov::InferRequest::set\_tensor and get with ov::InferRequest::get\_tensor. A plugin uses these hints to determine its internal layouts and element types for input and output tensors if needed.

### \~InferRequest Destructor

Destructor can contain plugin specific logic to finish and destroy infer request.

src/plugins/template/src/sync\_infer\_request.cpp

### set\_tensors\_impl()

The method allows to set batched tensors in case if the plugin supports it.

src/plugins/template/src/sync\_infer\_request.cpp

### query\_state()

The method returns variable states from the model.

src/plugins/template/src/sync\_infer\_request.cpp

### infer()

The method calls actual pipeline stages synchronously. Inside the method plugin should check input/output tensors, move external tensors to backend and run the inference.

src/plugins/template/src/sync\_infer\_request.cpp

#### 1\. infer\_preprocess()

Below is the code of the `infer_preprocess()` method. The method checks user input/output tensors and demonstrates conversion from user tensor to backend specific representation:

src/plugins/template/src/sync\_infer\_request.cpp

#### 2\. start\_pipeline()

Executes a pipeline synchronously using `m_executable` object:

src/plugins/template/src/sync\_infer\_request.cpp

#### 3\. wait\_pipeline()

Waits a pipeline in case of plugin asynchronous execution:

src/plugins/template/src/sync\_infer\_request.cpp

#### 4\. infer\_postprocess()

Converts backend specific tensors to tensors passed by user:

src/plugins/template/src/sync\_infer\_request.cpp

### get\_profiling\_info()

The method returns the profiling info which was measured during pipeline stages execution:

src/plugins/template/src/sync\_infer\_request.cpp

### cancel()

The plugin specific method allows to interrupt the synchronous execution from the AsyncInferRequest:

src/plugins/template/src/sync\_infer\_request.cpp

The next step in the plugin library implementation is the \[Asynchronous Inference Request\](asynch-inference-request.md) class.
