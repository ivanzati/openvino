---
sidebar_label: 'OpenVINO™ Integrations'
format: md
---

# OpenVINO™ Integrations

OpenVINO has been adopted by multiple AI projects in various areas. For an extensive list of
community-based projects involving OpenVINO, see the
[Awesome OpenVINO repository](https://github.com/openvinotoolkit/awesome-openvino).

**Hugging Face Optimum-Intel**

1 1 2 2

Grab and use models leveraging OpenVINO within the Hugging Face API. The repository hosts pre-optimized OpenVINO IR models, so that you can use them in your projects without the need for any adjustments.  
Benefits:  
\- Minimize complex coding for Generative AI.

  - \[Run inference with HuggingFace and Optimum Intel\](../../openvino-workflow-generative/inference-with-optimum-intel.md)
  - [A notebook example: llm-chatbot](https://github.com/openvinotoolkit/openvino_notebooks/tree/latest/notebooks/llm-chatbot)
  - [Hugging Face Inference documentation](https://huggingface.co/docs/optimum/main/intel/openvino/inference)
  - [Hugging Face Compression documentation](https://huggingface.co/docs/optimum/main/intel/openvino/optimization)
  - [Hugging Face Reference Documentation](https://huggingface.co/docs/optimum/main/intel/openvino/reference)

Check example code

``` py
-from transformers import AutoModelForCausalLM
+from optimum.intel.openvino import OVModelForCausalLM

from transformers import AutoTokenizer, pipeline
model_id = "togethercomputer/RedPajama-INCITE-Chat-3B-v1"

-model = AutoModelForCausalLM.from_pretrained(model_id)
+model = OVModelForCausalLM.from_pretrained(model_id, export=True)
```

**OpenVINO Execution Provider for ONNX Runtime**

1 1 2 2

Utilize OpenVINO as a backend with your existing ONNX Runtime code.  
Benefits:  
\- Enhanced inference performance on Intel hardware with minimal code modifications.

  - A notebook example: YOLOv8 object detection
  - [ONNX User documentation](https://onnxruntime.ai/docs/execution-providers/OpenVINO-ExecutionProvider.html)
  - [Build ONNX RT with OV EP](https://oliviajain.github.io/onnxruntime/docs/build/eps.html#openvino)
  - [ONNX Examples](https://onnxruntime.ai/docs/execution-providers/OpenVINO-ExecutionProvider.html#openvino-execution-provider-samples-tutorials)

Check example code

``` cpp
device = `CPU_FP32`
# Set OpenVINO as the Execution provider to infer this model
sess.set_providers([`OpenVINOExecutionProvider`], [{device_type` : device}])
```

**Torch.compile with OpenVINO**

1 1 2 2

Use OpenVINO for Python-native applications by JIT-compiling code into optimized kernels.  
Benefits:  
\- Enhanced inference performance on Intel hardware with minimal code modifications.

  - \[PyTorch Deployment via torch.compile\](../../openvino-workflow/torch-compile.md)
  - [torch.compiler documentation](https://pytorch.org/docs/stable/torch.compiler.html)
  - [torch.compiler API reference](https://pytorch.org/docs/stable/torch.compiler_api.html)

Check example code

``` python
import openvino.torch

...
model = torch.compile(model, backend='openvino')
...
```

**OpenVINO LLMs with LlamaIndex**

1 1 2 2

Build context-augmented GenAI applications with the LlamaIndex framework and enhance runtime performance with OpenVINO.  
Benefits:  
\- Minimize complex coding for Generative AI.

  - \[LLM inference with Optimum-intel\](../../openvino-workflow-generative/inference-with-optimum-intel.md)
  - [A notebook example: llm-agent-rag](https://github.com/openvinotoolkit/openvino_notebooks/blob/latest/notebooks/llm-agent-react/llm-agent-rag-llamaindex.ipynb)
  - [Inference documentation](https://docs.llamaindex.ai/en/stable/examples/llm/openvino/)
  - [Rerank documentation](https://docs.llamaindex.ai/en/stable/examples/node_postprocessor/openvino_rerank/)
  - [Embeddings documentation](https://docs.llamaindex.ai/en/stable/examples/embeddings/openvino/)
  - [API Reference](https://docs.llamaindex.ai/en/stable/api_reference/llms/openvino/)

Check example code

``` python
ov_config = {
    "PERFORMANCE_HINT": "LATENCY",
    "NUM_STREAMS": "1",
    "CACHE_DIR": "",
}

ov_llm = OpenVINOLLM(
    model_id_or_path="HuggingFaceH4/zephyr-7b-beta",
    context_window=3900,
    max_new_tokens=256,
    model_kwargs={"ov_config": ov_config},
    generate_kwargs={"temperature": 0.7, "top_k": 50, "top_p": 0.95},
    messages_to_prompt=messages_to_prompt,
    completion_to_prompt=completion_to_prompt,
    device_map="cpu",
)
```

**OpenVINO Backend for ExecuTorch**

1 1 2 2

Export and run AI models using OpenVINO with ExecuTorch to optimize performance on Intel hardware.  
Benefits:  
\- Accelerate inference, reduce latency, and simplify deployment for efficient AI applications.

  - [OpenVINO Backend for ExecuTorch](https://github.com/pytorch/executorch/blob/main/backends/openvino/README.md)
  - [OpenVINO Backend Examples](https://github.com/pytorch/executorch/blob/main/examples/openvino/README.md)
  - [Building and Running ExecuTorch with OpenVINO Backend](https://github.com/pytorch/executorch/blob/main/docs/source/build-run-openvino.md)

Check example code

``` python
python aot_optimize_and_infer.py --export --suite timm --model vgg16 --input_shape "[1, 3, 224, 224]" --device CPU
```

**OpenVINO Integration for LangChain**

1 1 2 2

Integrate OpenVINO with the LangChain framework to enhance runtime performance for GenAI applications.  
Benefits:  
\- Streamline the integration and chaining of language models for efficient AI workflows.

  - [LangChain Integration Guide](https://python.langchain.com/docs/integrations/llms/openvino/)

Check example code

``` python
ov_llm = HuggingFacePipeline.from_model_id(
   model_id="ov_model_dir",
   task="text-generation",
   backend="openvino",
   model_kwargs={"device": "CPU", "ov_config": ov_config},
   pipeline_kwargs={"max_new_tokens": 10},
)

chain = prompt | ov_llm

question = "What is electroencephalography?"

print(chain.invoke({"question": question}))
```

**Intel® Geti™**

1 1 2 2

Build computer vision models faster with less data using Intel® Geti™. It streamlines labeling, training, and deployment, exporting models optimized for OpenVINO.  
Benefits:  
\- Train with less data and deploy models faster.

  - [Intel® Geti Overview](https://docs.geti.intel.com/)
  - [Documentation](https://docs.geti.intel.com/docs/user-guide/getting-started/introduction)
  - [Tutorials](https://docs.geti.intel.com/docs/user-guide/getting-started/use-geti/tutorials)

**AI Playground™**

1 1 2 2

Use Intel® OpenVINO™ in AI Playground to optimize and run AI models efficiently on Intel CPUs and Arc GPUs, enabling local image generation, editing, and video processing. It supports OpenVINO-optimized models like TinyLlama, Mistral 7B, and Phi-3 mini, no conversion needed.  
Benefits:  
\- Easily set up pre-optimized models.  
\- Run faster, hardware-accelerated inference with OpenVINO.

  - [AI Playground Overview](https://www.intel.com/content/www/us/en/products/docs/discrete-gpus/arc/software/ai-playground.html)
  - [GitHub Repository](https://github.com/intel/AI-Playground)
  - [User Guide](https://github.com/intel/ai-playground/blob/main/AI%20Playground%20Users%20Guide.pdf)

**Intel® AI Assistant Builder**

1 1 2 2

Run local AI assistants with Intel® AI Assistant Builder using OpenVINO-optimized models like Phi-3 and Qwen2.5. Build secure, efficient assistants customized for your data and workflows.  
Benefits:  
\- Build custom assistants with agentic workflows and knowledge bases.  
\- Keep data secure by running fully local.

  - [GitHub Repository](https://github.com/intel/intel-ai-assistant-builder)
