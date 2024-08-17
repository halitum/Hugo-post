+++
title = "[Datawhale AI 夏令营] Task 3 源大模型RAG实战"
date = "2024-08-17"
description = ""
tags = [
 "LLM",
]
categories = [
 "DataWhale组队学习",
]
image = "https://typora-picturelib.oss-cn-beijing.aliyuncs.com/1-1-0.png"

+++
## # RAG简介

### 用途：

解决了大型模型在知识局限性、数据安全性和生成幻觉等方面的问题

<img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240817234227874.png" alt="image-20240817234227874" style="zoom:67%;" />

### 具体流程：

- 索引：将文档库分割成较短的 **Chunk**，即文本块或文档片段，然后构建成向量索引。

- 检索：计算问题和 Chunks 的相似度，检索出若干个相关的 Chunk。

- 生成：将检索到的Chunks作为背景信息，生成问题的回答。

<img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/1-1-0.png" alt="1-1-0"  />

------

## # 搭建并使用向量数据库Chroma

安装Chroma

```bash
pip install chromadb
```

上手Chrome

```python
# 创建Chroma客户端
import chromadb
client = chromadb.Client()

# 创建集合（集合类似于传统数据库中的表）
collection = client.create_collection(name="your_collection_name")

# 添加文档（Chroma会自动处理文本嵌入和索引）
collection.add(documents=["Your first document text"])

# 查询集合
results = collection.query(query_texts=["Sample query"])
```

------

## # 基于LangChain的RAG流程

安装LangChain

```bash
pip install langchain langchain-community langchain-core langchain-openai unstructured sentence-transformers chromadb
```

上手LangChain-RAG
```python
# 创建LLM客户端（基于Yuan2.0 Inference-Server）
from langchain.chains import LLMChain
from langchain_community.llms.yuan2 import Yuan2
infer_api = "http://127.0.0.1:8000/yuan"	# default infer_api for a local deployed Yuan2.0 inference server

# 初始化模型
yuan_llm = Yuan2(
    infer_api=infer_api,
    max_tokens=2048,
    temp=1.0,
    top_p=0.9,
    use_history=False,
)
```

