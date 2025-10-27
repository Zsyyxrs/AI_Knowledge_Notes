
RAG（Retrieval Augmented Generation）顾名思义，通过**检索**的方法来增强**生成模型**的能力。

![](https://cdn.jsdelivr.net/gh/Zsyyxrs/picgo-images/img/rag.png)

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

向量检索

文本向量
1. 将文本转成一组*N*维浮点数，即**文本向量**又叫 Embeddings
2. 向量之间可以计算距离，距离远近对应**语义相似度**大小
文本向量训练：
3. 构建相关（正例）与不相关（负例）的句子对样本
4. 训练双塔式模型，让正例间的距离小，负例间的距离大

![](https://cdn.jsdelivr.net/gh/Zsyyxrs/picgo-images/img/sbert.png)


测试图片
![image.png](https://cdn.jsdelivr.net/gh/Zsyyxrs/picgo-images/img/20251027225616935.png)
