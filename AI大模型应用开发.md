# OpenAI大模型基础

### 安装OpenAI库
pip install OpenAI
!pip install OpenAI

### 将API-KEY加入环境变量

### 安装tiktoken，统计编码token数
import tiktoken
encoding = tiktoken.encoding_for_model("gpt-3.5-turbo")
encoding

encoding.encode("黄河之水天上来")
