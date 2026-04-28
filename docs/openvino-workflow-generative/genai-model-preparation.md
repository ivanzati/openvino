---
sidebar_label: 'Generative Model Preparation'
format: md
---

# Generative Model Preparation

> prepare generative models for inference.

Since generative AI models tend to be big and resource-heavy, it is advisable to
optimize them for efficient inference. This article will show how to prepare
LLM models for inference with OpenVINO by:

  - [Downloading Models from Hugging Face](#download-generative-models-from-hugging-face-hub)
  - [Downloading Models from Model Scope](#download-generative-models-from-model-scope)
  - [Converting and Optimizing Generative Models](#convert-and-optimize-generative-models)

## Download Generative Models From Hugging Face Hub

Pre-converted and pre-optimized models are available in the [OpenVINO Toolkit](https://huggingface.co/OpenVINO)
organization, under the [model section](https://huggingface.co/OpenVINO#models), or under
different model collections:

  - [LLM:](https://huggingface.co/collections/OpenVINO/llm-6687aaa2abca3bbcec71a9bd)
  - [Speech-to-Text](https://huggingface.co/collections/OpenVINO/speech-to-text-672321d5c070537a178a8aeb)
  - [Speculative Decoding Draft Models](https://huggingface.co/collections/OpenVINO/speculative-decoding-draft-models-673f5d944d58b29ba6e94161)

You can also use the **huggingface\_hub** package to download models:

``` console
pip install huggingface_hub
huggingface-cli download "OpenVINO/phi-2-fp16-ov" --local-dir model_path
```

The models can be used in OpenVINO immediately after download. No dependencies
are required except **huggingface\_hub**.

## Download Generative Models From Model Scope

To download models from [Model Scope](https://www.modelscope.cn/home),
use the **modelscope** package:

``` console
pip install modelscope
modelscope download --model "Qwen/Qwen2-7b" --local_dir model_path
```

Models downloaded via Model Scope are available in Pytorch format only and they must
be \[converted to OpenVINO IR\](../../openvino-workflow/model-preparation/convert-model-to-ir.md)
before inference.

## Convert and Optimize Generative Models

OpenVINO works best with models in the OpenVINO IR format, both in full precision and quantized.
If your selected model has not been pre-optimized, you can easily do it yourself, using a single
**optimum-cli** command. For that, make sure optimum-intel is installed on your system:

``` console
pip install optimum-intel[openvino]
```

While optimizing models, you can decide to keep the original precision or select one that is lower.

Keeping full model precision

``` console
optimum-cli export openvino --model <model_id> --weight-format fp16 <exported_model_name>
```

Examples:

LLM (text generation)

``` console
optimum-cli export openvino --model meta-llama/Llama-2-7b-chat-hf --weight-format fp16 ov_llama_2
```

Diffusion models (text2image)

``` console
optimum-cli export openvino --model stabilityai/stable-diffusion-xl-base-1.0 --weight-format fp16 ov_SDXL
```

VLM (Image processing):

``` console
optimum-cli export openvino --model openbmb/MiniCPM-V-2_6 --trust-remote-code –weight-format fp16 ov_MiniCPM-V-2_6
```

Whisper models (speech2text):

``` console
optimum-cli export openvino --trust-remote-code --model openai/whisper-base ov_whisper
```

SpeechT5 TTS models (text2speech):

``` console
optimum-cli export openvino --model microsoft/speecht5_tts --model-kwargs "{\"vocoder\": \"microsoft/speecht5_hifigan\"}" ov_speecht5_tts
```

Exporting to selected precision

``` console
optimum-cli export openvino --model <model_id> --weight-format int4 <exported_model_name>
```

Examples:

LLM (text generation)

``` console
optimum-cli export openvino --model meta-llama/Llama-2-7b-chat-hf --weight-format int4 ov_llama_2
```

Diffusion models (text2image)

``` console
optimum-cli export openvino --model stabilityai/stable-diffusion-xl-base-1.0 --weight-format int4 ov_SDXL
```

VLM (Image processing)

``` console
optimum-cli export openvino -m model_path --task text-generation-with-past --weight-format int4 ov_MiniCPM-V-2_6
```

Note

Any other `model_id`, for example `openbmb/MiniCPM-V-2_6`, or the path
to a local model file can be used.

Also, you can specify different data type like `int8`.

## Additional Resources

  - [Full set of optimum-cli parameters](https://huggingface.co/docs/optimum/en/intel/openvino/export)
  - \[Model conversion in OpenVINO\](../../openvino-workflow/model-preparation/convert-model-to-ir.md)
  - \[Model optimization in OpenVINO\](../../openvino-workflow/model-optimization.md)
