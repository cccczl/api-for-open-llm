## docker build

```shell
docker build -t llm-api:pytorch .
```

## docker run

### 主要参数

+ `model_name`: 模型名称，如 `chatglm`、`phoenix`、`moss`等


+ `model_path`: 开源大模型的文件所在路径


+ `device`: 是否使用 `GPU`，可选值为 `cuda` 和 `cpu`，默认值为 `cuda`


+ `adapter_model_path`（可选项）: `lora` 或 `ptuing_v2` 模型文件所在路径


+ `embedding_name`（可选项）: 嵌入模型的文件所在路径


+ `context_len`（可选项）: 上下文长度，默认为 `2048`


+ `quantize`（可选项）: `chatglm` 模型的量化等级，可选项为 16、8、4


+ `load_in_8bit`（可选项）: 使用模型 `8bit` 量化


+ `load_in_4bit`（可选项）: 使用模型 `4bit` 量化


+ `use_ptuning_v2`（可选项）: 使用 `ptuning_v2` 加载模型


+ `stream_interval`（可选项）: 单词流式输出的 `token` 数量


### ChatGLM

chatglm-6b:

```shell
docker run -it -d --gpus all --ipc=host --net=host -p 80:80 --name=chatglm \
    --ulimit memlock=-1 --ulimit stack=67108864 \
    -v `pwd`:/workspace \
    llm-api:pytorch \
    python api/app.py \
    --port 80 \
    --allow-credentials \
    --model_name chatglm \
    --model_path THUDM/chatglm-6b \
    --device cuda \
    --embedding_name moka-ai/m3e-base
```

chatglm2-6b:

```shell
docker run -it -d --gpus all --ipc=host --net=host -p 80:80 --name=chatglm \
    --ulimit memlock=-1 --ulimit stack=67108864 \
    -v `pwd`:/workspace \
    llm-api:pytorch \
    python api/app.py \
    --port 80 \
    --allow-credentials \
    --model_name chatglm \
    --model_path THUDM/chatglm2-6b \
    --device cuda \
    --embedding_name moka-ai/m3e-base
```

ptuing-v2:

```shell
docker run -it -d --gpus all --ipc=host --net=host -p 80:80 --name=chatglm \
    --ulimit memlock=-1 --ulimit stack=67108864 \
    -v `pwd`:/workspace \
    llm-api:pytorch \
    python api/app.py \
    --port 80 \
    --allow-credentials \
    --model_name chatglm \
    --model_path THUDM/chatglm-6b \
    --device cuda \
    --adapter_model_path ptuing_v2_chekpint_dir \
    --use_ptuning_v2 \
    --embedding_name moka-ai/m3e-base
```

### MOSS

```shell
docker run -it -d --gpus all --ipc=host --net=host -p 80:80 --name=moss \
    --ulimit memlock=-1 --ulimit stack=67108864 \
    -v `pwd`:/workspace \
    llm-api:pytorch \
    python api/app.py \
    --port 80 \
    --allow-credentials \
    --model_name moss \
    --model_path fnlp/moss-moon-003-sft-int4 \
    --device cuda \
    --embedding_name moka-ai/m3e-base
```

### Phoenix

```shell
docker run -it -d --gpus all --ipc=host --net=host -p 80:80 --name=phoenix \
    --ulimit memlock=-1 --ulimit stack=67108864 \
    -v `pwd`:/workspace \
    llm-api:pytorch \
    python api/app.py \
    --port 80 \
    --allow-credentials \
    --model_name phoenix \
    --model_path FreedomIntelligence/phoenix-inst-chat-7b \
    --device cuda \
    --embedding_name moka-ai/m3e-base
```

### Guanaco

```shell
docker run -it -d --gpus all --ipc=host --net=host -p 80:80 --name=guanaco \
    --ulimit memlock=-1 --ulimit stack=67108864 \
    -v `pwd`:/workspace \
    llm-api:pytorch \
    python api/app.py \
    --port 80 \
    --allow-credentials \
    --model_name guanaco \
    --model_path timdettmers/guanaco-33b-merged \
    --device cuda \
    --load_in_4bit \
    --embedding_name moka-ai/m3e-base
```

### Tiger

```shell
docker run -it -d --gpus all --ipc=host --net=host -p 80:80 --name=tiger \
    --ulimit memlock=-1 --ulimit stack=67108864 \
    -v `pwd`:/workspace \
    llm-api:pytorch \
    python api/app.py \
    --port 80 \
    --allow-credentials \
    --model_name tiger \
    --model_path TigerResearch/tigerbot-7b-sft \
    --device cuda \
    --load_in_4bit \
    --embedding_name moka-ai/m3e-base
```

### OpenBuddy

LLaMA

```shell
docker run -it -d --gpus all --ipc=host --net=host -p 80:80 --name=openbuddy-llama \
    --ulimit memlock=-1 --ulimit stack=67108864 \
    -v `pwd`:/workspace \
    llm-api:pytorch \
    python api/app.py \
    --port 80 \
    --allow-credentials \
    --model_name openbuddy-llama \
    --model_path OpenBuddy/openbuddy-llama-7b-v1.4-fp16 \
    --device cuda \
    --embedding_name moka-ai/m3e-base
```

Falcon

```shell
docker run -it -d --gpus all --ipc=host --net=host -p 80:80 --name=openbuddy-falcon \
    --ulimit memlock=-1 --ulimit stack=67108864 \
    -v `pwd`:/workspace \
    llm-api:pytorch \
    python api/app.py \
    --port 80 \
    --allow-credentials \
    --model_name openbuddy-falcon \
    --model_path OpenBuddy/openbuddy-falcon-7b-v5-fp16 \
    --device cuda \
    --embedding_name moka-ai/m3e-base
```

### Baichuan-7b

使用半精度加载模型（大约需要14G显存）

```shell
docker run -it -d --gpus all --ipc=host --net=host -p 80:80 --name=baichuan \
    --ulimit memlock=-1 --ulimit stack=67108864 \
    -v `pwd`:/workspace \
    llm:pytorch-1.14 \
    python api/app.py \
    --port 80 \
    --allow-credentials \
    --model_name baichuan \
    --model_path baichuan-inc/baichuan-7B \
    --adapter_model_path YeungNLP/firefly-baichuan-7b-qlora-sft \
    --device cuda \
    --embedding_name moka-ai/m3e-base
```

如果想要使用全精度加载模型，需要修改 [api/model.py](./api/models.py) 第 426 行

```python
@property
def model_kwargs(self):
    return {"trust_remote_code": True, "device_map": "auto", "torch_dtype": torch.float32}
```
