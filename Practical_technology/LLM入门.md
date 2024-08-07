+++

title = "LLM入门"

date = "2023-11-13"

description = ""

tags = [

 "LLM",

]

categories = [

 "实用技术",

]

image = "https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231114133128113.png"

+++

# LLM大语言模型简介

## 一、概念

大语言模型（Large Language Model）是一种人工智能模型，旨在理解和生成人类语言。

尽管这些大型语言模型与小型语言模型使用相似的架构和预训练任务，但它们展现出截然不同的能力，尤其在解决复杂任务时表现出了惊人的潜力，这被称为“涌现能力”。以 GPT-3 和 GPT-2 为例，GPT-3 可以通过学习上下文来解决少样本任务，而 GPT-2 在这方面表现较差。

![image-20231114133128113](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231114133128113.png)

## 二、能力

### 1. 涌现能力（emergent abilities)：模型性能随着规模增大而迅速提升，并获得处理复杂任务的能力

​	三个典型的LLM涌现能力：

- **上下文学习**：允许语言模型在提供自然语言指令或多个任务示例的情况下，通过理解上下文并生成相应输出的方式来执行任务
- **指令遵循**：通过使用自然语言描述的多任务数据进行*微调*（指令微调），即能够根据任务指令执行任务，而无需事先见过具体示例
- **逐步推理**：可以利用包含中间推理步骤的提示机制来解决数学问题等涉及多个推理步骤的复杂任务

### 2. 作为基座模型支持多元应用

基座模型（foundation model）是一种全新的AI技术范式，借助于海量无标注数据的训练，获得可以适用于大量下游任务的大模型。

### 3.支持对话作为统一入口

## 三、特点

1. 使用预训练和微调作为学习方法
2. 多语言与多模态支持
3. 多领域应用
4. 伦理和风险问题

## 四、langchain框架

### 1. 简介

LangChain 框架是一个开源工具，旨在帮助开发者们快速构建基于大型语言模型的端到端应用程序或工作流程。

其充分利用了大型语言模型的强大能力，以便开发各种下游应用。它的目标是为各种大型语言模型应用提供通用接口，从而简化应用程序的开发流程。

具体来说，LangChain 框架可以实现数据感知和环境互动，也就是说，它能够让语言模型与其他数据来源连接，并且允许语言模型与其所处的环境进行互动。

### 2. 核心组件

- **模型输入/输出（Model I/O）**：与语言模型交互的接口
- **数据连接（Data connection）**：与特定应用程序的数据进行交互的接口
- **链（Chains）**：将组件组合实现端到端应用。
- **记忆（Memory）**：用于链的多次运行之间持久化应用程序状态；
- **代理（Agents）**：扩展模型的推理能力。用于复杂的应用的调用序列；
- **回调（Callbacks）**：扩展模型的推理能力。用于复杂的应用的调用序列。



# Prompt设计原则

### 1. 原则一：编写清晰/具体的指令

> 使用详尽/简介的语言表达Prompt。与此同时，Prompt应该尽可能丰富，更长、更复杂的 Prompt 反而会让语言模型更容易抓住关键点

#### 1.1 使用分隔符表示输入的不同部分

可以选择用 ```，"""，< >， ，: 等做分隔符，只要能明确起到隔断作用即可

#### 1.2 寻求结构化的输出

结构化输出是按照某种格式组织的内容，例如JSON、HTML等

#### 1.3 要求模型检查是否满足条件

如果任务不满足假设的条件，则让大模型指出并停止执行后续的完整流程，这可以避免一些意外情况发生

#### 1.4 提供少量示例

"Few-shot" prompting（少样本提示）：在要求模型执行实际任务之前，给模型一两个已完成的样例，让模型了解我们的要求和期望的输出样式

> prompt = f"""
> 您的任务是以一致的风格回答问题（注意：文言文和白话的区别）。
> <学生>: 请教我何为耐心。
> <圣贤>: 天生我材必有用，千金散尽还复来。
> <学生>: 请教我何为坚持。
> <圣贤>: 故不积跬步，无以至千里；不积小流，无以成江海。骑骥一跃，不能十步；驽马十驾，功在不舍。
> <学生>: 请教我何为孝顺。
> """
> response = get_completion(prompt)
> print(response)

### 2. 原则二：给予模型充足的思考时间

> 可以要求模型先列出对问题的各种看法，说明推理依据，然后再得出最终结论。也就是在Prompt中添加逐步推理的要求

#### 2.1 指定完成任务所需的步骤

#### 2.2 指导模型在下结论之前找出一个自己的解法

注意应该要求模型先自行解决问题，再根据模型得到的的解法与自己的解法进行对比

## GPT使用技巧

## 1. 设置gpt的角色

You are a marketing consultant with 20 years of experience at a Fortune 500 company. I have recently opened a milk tea shop named 'Misu Ice City.' Please help me come up with three marketing and promotional themes.

## 2. 设置自己的角色

I am an independent developer with no experience in system programming, but I hope you can help me complete the development work and solve the difficulties in the process. I want your answers to be beginner-friendly, teaching me step by step how to do it, while also reflecting on the quality of the code you provide, trying to ensure accuracy and no bugs, and acting as a mentor to developers. If my questions are not accurate or complete, you can ask me for more information to confirm my needs.

## 3. 限制回答长度

## 4. 让gpt一步步思考

## 5. 明确要求和目的

## 6. 给予充分的背景信息

## 7. 结构化prompt

## 8. 系统化设计自定义指令（Custom Instruction）

- 越丰富越好（？存疑）
- 主题
- 需求
  “How GPT know about you”
  I hope to gain a deep understanding of the knowledge and thought in various professional fields.
  “How GPT respond”
  I may engage in conversations with you that involve a variety of types and themes. Under different types of conversations, I have different requirements for your style of response. I need you to first determine what type of conversation we are having each time, and then generate answers according to my characteristics and needs under this type.
  1……（使用场景）(导入个人角色）

## 9. 输出长内容前，让gpt为自己的输出内容梳理大纲

bookai.top（职业prompt）

## 调用大模型API参数

**Prompt** ：给 LLM 的输入

**Completion** ： LLM 的输出

**Temprature**：*随机性/创造性*控制参数（1为高随机性，0为低随机性）

> 对于不同的问题与应用场景，我们可能需要设置不同的 Temprature
>
> Temprature 设置为0，可以保证LLM的稳定使用，规避错误内容、模型幻觉
>
> 使用更高的Temprature来获得更具有创意性的Completion

**System Prompt**：该种 Prompt 内容会在整个会话过程中持久地影响模型的回复

> System Prompt 可以用来对模型进行一些初始化设定
