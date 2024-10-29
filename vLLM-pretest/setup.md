# Setup

## Download the model

For CPU, quantization is not an option with vLLM. Use the base model.

```bash
git lfs install;
git clone https://huggingface.co/Qwen/Qwen2-VL-2B-Instruct
```

## Prepare the python environment

```bash
#From this directory (vLLM-pretest)
python3 -m venv venv
source venv/bin/activate
pip install OpenAI
```

## Create a test.py test file if it does not exist.

> As we mount a local model, the name of the model will be its absolute path "/qwen".
> Make sure to change the absolute path to that of the example image.

```python
import base64
from openai import OpenAI
# Set OpenAI's API key and API base to use vLLM's API server.
openai_api_key = "EMPTY"
openai_api_base = "http://localhost:8000/v1"
client = OpenAI(
    api_key=openai_api_key,
    base_url=openai_api_base,
)
# Set the path to the test image
image_path = "/path/to/this/folder/fapiao.png"
with open(image_path, "rb") as f:
    encoded_image = base64.b64encode(f.read())
encoded_image_text = encoded_image.decode("utf-8")
base64_qwen = f"data:image;base64,{encoded_image_text}"
chat_response = client.chat.completions.create(
    model="/qwen",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {
            "role": "user",
            "content": [
                {
                    "type": "image_url",
                    "image_url": {
                        "url": base64_qwen
                    },
                },
                {"type": "text", "text": "What is the name of the seller?"},
            ],
        },
    ],
)
print("Chat response:", chat_response)
```

## Start with the docker

```bash
sudo docker run -v /path/to/qwen/cloned/from/huggingface:/qwen:ro -p 8000:8000 vllm/vllm-openai:latest --model /qwen --device cpu --dtype half
```
