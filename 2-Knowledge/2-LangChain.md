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
			'''python
			trim_messages(
			    messages,
			    max_tokens=45,
			    strategy="last",
			    token_counter=ChatOpenAI(model="gpt-4o-mini"),
			    include_system=True,
			    allow_partial=True,
			)'''
2. filter_messages过滤消息

# 五、架构封装