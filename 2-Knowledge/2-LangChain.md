# 一、简介
**LangChain** 是一个开源框架，专门用于**构建基于大语言模型（LLM）的应用程序**。它把「大模型」和「外部世界」连接起来，让开发者能更轻松地实现像 ChatGPT、RAG（检索增强生成）、智能体（Agent）等复杂功能。
[LangChain官网](https://www.langchain.com/)
![](https://cdn.jsdelivr.net/gh/Zsyyxrs/picgo-images/img/langchain.png)
# 二、模型I/O封装
- LLMs：大语言模型
- Chat Models：一般基于 LLMs，但按对话结构重新封装
- PromptTemple：提示词模板
- OutputParser：解析输出

**接口位置**

| 原来的路径                 | 现在的位置                                              | 说明                           |
| --------------------- | -------------------------------------------------- | ---------------------------- |
| langchain.schema      | langchain_core.messages                            | 消息结构（AI/Human/SystemMessage） |
| langchain.chat_models | langchain_openai 或 langchain_community.chat_models | 模型实现部分分离                     |
| langchain.llms        | langchain_openai / langchain_anthropic 等           | 各厂商独立子包                      |
| langchain.prompts     | langchain_core.prompts                             | Prompt 模型化部分                 |

![](https://cdn.jsdelivr.net/gh/Zsyyxrs/picgo-images/img/model_io.jpg)

- PromptTemplate 可以在模板中自定义变量
- ChatPromptTemplate 用模板表示的对话上下文
- MessagesPlaceholder 把多轮对话变成模板
- outparser类将大模型的输出解析成结构化对象
- LangChain 提供了 Function Calling 的封装

# 三、数据连接封装
1. PyMuPDFLoader加载文档
2. RecursiveCharacterTextSplitter用于文本处理
3. vectorstores选取合适的向量数据库

# 四、对话历史管理
1. trim_messages裁剪消息
```python
trim_messages(
	messages,  # 当前对话历史，通常是一个列表
	max_tokens=45,  # 上限控制参数：最终裁剪后的对话，总token数不能超过 45 个
	strategy="last",  # 裁剪策略：保留最后的几条消息
	token_counter=ChatOpenAI(model="gpt-4o-mini"), # token计数器对象
	include_system=True,  # 是否保留system消息
	allow_partial=True,  # 是否允许部分消息被截断（即只保留一部分内容）
)
```
2. filter_messages过滤消息
```python
filter_messages(
	messages,  # 原始聊天记录，带id(唯一编号) name(可选的发送者标识) role(system/user/assistant) content(消息内容)
	exclude_names=["example_user", "example_assistant"],  # 排除掉某些特定角色或名字的消息
	exclude_ids=["3"],  # 手动删除特定id的消息
)
```
3. 存储历史消息到数据库
```python
from langchain_community.chat_message_histories import SQLChatMessageHistory
def get_session_history(session_id):
    # 通过 session_id 区分对话历史，并存储在 sqlite 数据库中
    return SQLChatMessageHistory(session_id, "sqlite:///memory.db")
```
# 五、架构封装
LangChain Expression Language（LCEL）是一种声明式语言，可轻松组合不同的调用顺序构成 Chain。
```python
# LCEL 表达式
runnable = (
{"text": RunnablePassthrough()} | prompt | structured_llm
)
# 直接运行
ret = runnable.invoke("不超过100元的流量大的套餐有哪些")
```
通过langserver实现client-server服务
LangChain 侧重与 LLM 本身交互的封装
-  Prompt、LLM、Message、OutputParser 等工具丰富
- 在数据处理和 RAG 方面提供的工具相对粗糙
- 主打 LCEL 流程封装
- 配套 Agent、LangGraph 等智能体与工作流工具
- 另有 LangServe 部署工具和 LangSmith 监控调试工具
LlamaIndex 侧重与数据交互的封装
- 数据加载、切割、索引、检索、排序等相关工具丰富
- Prompt、LLM 等底层封装相对单薄
- 配套实现 RAG 相关工具
- 有 Agent 相关工具，不突出
