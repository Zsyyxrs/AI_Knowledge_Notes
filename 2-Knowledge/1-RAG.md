
RAG（Retrieval Augmented Generation）顾名思义，通过**检索**的方法来增强**生成模型**的能力。

pdfminer.six或者pdfplumber解析pdf提取文字等

不同环境下可以放不同的 .env 文件：
.env.dev
.env.prod
load_dotenv(".env.dev")
find_dotenv()在当前路径和父路径找，返回完整路径

不要上传 .env 到 GitHub ，在 .gitignore 中加一行 .env

嵌入模型怎么拆分、训练的
找项目相关的语料库用LLM进行评估
text-embedding-ada-002 openAI的闭源模型
大多数开源的需要微调