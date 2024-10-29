# Qwen2-VL

## vLLM

Recommended version `vllm=>06.1`

### Manually

#### Installation

```python 
pip install git+https://github.com/huggingface/transformers@21fac7abba2a37fae86106f87fcf9974fd1e3830
pip install accelerate
pip install qwen-vl-utils
# Change to your CUDA version
CUDA_VERSION=cu121
pip install 'vllm==0.6.1' --extra-index-url https://download.pytorch.org/whl/${CUDA_VERSION}
```

#### Start an OpenAI API Service

7B
```python
python -m vllm.entrypoints.openai.api_server --served-model-name Qwen2-VL-7B-Instruct --model Qwen/Qwen2-VL-7B-Instruct
```

2B
```python
python -m vllm.entrypoints.openai.api_server --served-model-name Qwen2-VL-2B-Instruct --model Qwen/Qwen2-VL-2B-Instruct
```

#### Use API service

```bash
curl http://localhost:8000/v1/chat/completions \
    -H "Content-Type: application/json" \
    -d '{
    "model": "Qwen2-VL-7B-Instruct",
    "messages": [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": [
        {"type": "image_url", "image_url": {"url": "https://modelscope.oss-cn-beijing.aliyuncs.com/resource/qwen.png"}},
        {"type": "text", "text": "What is the text in the illustrate?"}
    ]}
    ]
    }'
```

### Docker

Qwen + vLLM docker image: [https://github.com/QwenLM/Qwen2-VL#-docker](https://github.com/QwenLM/Qwen2-VL#-docker)