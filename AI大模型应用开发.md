# OpenAI大模型基础

### 安装OpenAI库
pip install OpenAI

!pip install OpenAI

### 将API-KEY加入环境变量

### 安装tiktoken，统计编码token数
import tiktoken

encoding = tiktoken.encoding_for_model("gpt-3.5-turbo")

encoding

<Encoding 'cl100k_base'>

encoding.encode("黄河之水天上来")

[30868, 226, 31106, 111, 55030, 53610, 36827, 17905, 37507]

len(encoding.encode("黄河之水天上来"))

9

max_token

temperature [0 - 2]，值越大随机性越大

frequency_penalty [-2 - 2]，影响的是生成文本里词重复出现的频率，值越大生成越多样性，一般设置0 - 1之间

presence-penalty [-2 - 2]，影响的是生成内容是否包含更多的新词

### 提示词工程
#### 1.使用最新的模型
#### 2.把指令放在提示的开头，并且用###或"""来分隔指令和上下文
#### 3.尽可能对上下文和输出的长度、格式、风格等给出具体、描述性、详细的要求
以{某著名诗人}的风格，写一首关于Open AI的鼓舞人心的短诗，主题要集中在最近推出的DALL-E产品上（DALL-E是一种根据文本生成图形的机器学习模型）
#### 4.通过一些例子来阐明想要的输出格式
提取下面文本中提到的重要实体。首先提取所有公司名称，然后提取所有人名，然后提取适合内容的特定主题，最后提取一般总体主题

所需格式:

公司名称: <逗号分隔公司名称列表>

人名: -||-

特定主题: -||-

一般主题: -||-

文本: {文本内容}

#### 5.先从零样本提示开始，效果不好，则用小样本提示
从下面相应的文本中提取关键词。

文本1：Stripe提供API，WEB开发人员可以使用这些API将支付处理集成到他们的网站和移动应用程序中。

关键词1：Stripe、支付处理、API、Web开发人员、网站、移动应用程序

文本2：{text}

关键词2：

#### 6.减少空洞和不严谨的描述
用3到5句话来组成一个段落来描述此产品

#### 7.与其告诉不应该做什么，不如告知应该做什么
以下是客服与客人之间的对话。客服将尝试了解问题并提出解决方案，同时避免询问任何隐私相关的问题。不要询问用户名或密码等隐私，而是让用户参阅帮助文章

www.samplewebsite.com/help/faq

客户：我无法登录我的账户。

客服：

#### 给AI的理想示范：
response = client.chat.completions.create(

  model="gpt-3.5-turbo",
  
  messages=[
  
    {"role": "user", "content": "格式化以下信息： \n姓名 -> 张三\n年龄 -> 27\n客户ID -> 001"},
    
    {"role": "assistant", "content": "##客户信息\n- 客户姓名: 张三\n- 客户年龄：27岁\n- 客户ID：001"}
    
  ]
  
)

##### 思维链：提供给AI解题步骤和示例，或者懒人方法"let's think step by step","让我们来分步骤思考"

#### API用法实例
from openai import OpenAI

clent = OpenAI()

def get_openai_response(client, system_prompt, user_prompt, model="gpt-3.5-turbo"):

  response = client.chat.completion.create(
    
    model=model,

    messages=[

      {"role": "system", "content": system_prompt},

      {"role": "user", "content": user_prompt}
    
    ],
    
  )

  return response.choices[0].message.content
#### LangChain
LangChain是一个用于支持AI大模型应用开发的框架，不仅是调用模型API，而且还可以感知模型连接外部数据，并且借助外部工具与环境进行互动，来生成更好的回答。

提供的组件，包括：模型（Model），提示模板，文件加载器，记忆（Memory），检索器（Retriever），agent和链（Chain）

模型提供语言理解和生成能力，是AI应用的核心，它们可以是来自各种AI服务提供方的模型；

记忆用来存储和管理对话历史或相关的上下文信息，这个组件是对话型AI应用中，保持连贯性和上下文感知的关键；

链是把不同组件串联起来的结构，能让我们创建出复杂的流程，流程里的每个组件可以处理特定的任务；

检索器负责从外部信息源检索信息，检索器对增加模型的知识面和回答准确性很总要；

Agent代表一个大模型的，能执行一系列动作的智能体；它的核心理念是利用AI模型的能力进行推理；根据任务，动态评估和确定行动路径。

#### LangChain Model I/O
主要有两种模型，LLM语言模型和Chat Model聊天模型。LLM模型接收一个字符串，返回一个字符串。Chat Model接收一个消息列表作为输入，返回一个消息列表输出，gpt-3.5这些都属于Chat Model模型。

#### 安装LangChain库
!pip install langchain_openai

pip install langchain_openai

from langchain_openai import ChatOpenAI

model = ChatOpenAI(model = "gpt-3.5-turbo")

有3种消息类型，SystemMessage对AI系统的指令, HumanMessage, AIMessage表示来自AI的系统消息

from langchain.schema.messages import (SystemMessage, HumanMessage)

messages = [

system_message = SystemMessage(content="请你作为我的物理课助教，用通俗易懂的语言解释物理概念。")

human = HumanMessage(content="什么是波粒二象性")

]

response = model.invoke(messages)

// chat_models.baidu_qianfan_endpoint.QianfanChatEndpoint 百度千帆平台模型

// chat_models.hunyuan.ChatHunyuan 腾讯混元模型

// chat_models.tongyi.ChatTongyi 阿里通义千问模型
