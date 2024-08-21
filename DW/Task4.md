+++
title = "[Datawhale AI 夏令营] Task 3 源大模型微调实战"
date = "2024-08-21"
description = ""
tags = [
 "LLM",
]
categories = [
 "",
]
image = ""

+++

## # 微调简介

### 用途：

提高大模型指令遵循能力，通过零样本学习的方式解决多种下游任务

![image-20240821232335436](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240821232335436.png)

### 常用技术路径——轻量化微调

全量参数微调消耗非常多的算力，而轻量化微调通过训练极少的模型参数达到全量微调相当的效果

轻量化微调有LoRA、Adapter 和 Prompt Tuning



### 具体流程：

### **1. 环境准备**

### **2. 模型下载**

### **3. 数据处理**

- 读取数据并转换为`Dataset`格式。
- 加载`tokenizer`，并定义数据处理函数`process_func`，该函数将文本转为`input_ids`和`attention_mask`，并进行拼接与截断处理。
- 对整个数据集应用此数据处理函数，并检查结果。

### **4. 模型训练**

- 加载源大模型并启用训练所需的梯度计算。
- 配置Lora微调的参数，生成`PeftModel`。
- 设置训练参数，初始化`Trainer`。
- 执行训练过程，监控并记录模型的loss值，以检查训练收敛情况。

### **5. 效果验证**

- 定义生成函数`generate()`用于验证模型效果。
- 使用训练好的模型对输入样例进行测试，验证微调后的模型性能。

### **6. Demo搭建**

- 重启内核清空显存，以准备部署Demo。
- 启动`streamlit`服务并在浏览器中访问Demo界面，输入测试数据进行验证。

<img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240821233614151.png" alt="image-20240821233614151" style="zoom:67%;" />

------

## # 基于LangChain的RAG流程

### 1. **环境配置**

```bash
pip install --upgrade openai langchain
```

### 2. **API和模型初始化**

```python
from langchain_community.chat_models import ChatYuan2
from langchain_core.messages import AIMessage, HumanMessage, SystemMessage

# 设置API密钥和基础URL
yuan2_api_base = "http://127.0.0.1:8001/v1"
yuan2_api_key = "your_api_key"

# 初始化ChatYuan2模型
chat = ChatYuan2(
    yuan2_api_base=yuan2_api_base,
    temperature=1.0,
    model_name="yuan2",
    max_retries=3,
    streaming=False
)
```

### 3. **数据准备**

```python
from datasets import load_dataset

# 加载数据集
dataset = load_dataset('your_dataset_name')

# 示例数据预处理（视你的数据情况而定）
def preprocess_function(examples):
    return tokenizer(examples['text'], truncation=True)

tokenized_datasets = dataset.map(preprocess_function, batched=True)
```

### 4. **模型微调**：使用LangChain的`FineTuner`工具

```python
from langchain import FineTuner

# 初始化FineTuner并传入Yuan模型
fine_tuner = FineTuner(model=chat)

# 训练模型
fine_tuner.train(training_data=tokenized_datasets['train'], epochs=5)

# 可选：保存微调后的模型
fine_tuner.save("fine_tuned_yuan_model")
```

### 5. **模型评估**：在验证集上评估模型

```python
results = fine_tuner.evaluate(validation_data=tokenized_datasets['validation'])
results
```

### 6. **部署微调后的模型**

```python
# 使用微调后的模型
fine_tuned_chat = ChatYuan2(
    yuan2_api_base=yuan2_api_base,
    temperature=1.0,
    model_name="fine_tuned_yuan_model",
    max_retries=3,
    streaming=False
)

# 生成响应
messages = [
    SystemMessage(content="你是一个人工智能助手。"),
    HumanMessage(content="你好，你是谁？")
]

response = fine_tuned_chat.invoke(messages)
response
```
