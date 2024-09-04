# 3.1 什么是RAG

说了这么多，下面我们来介绍一下什么是 RAG 。

RAG 是检索增强生成（Retrieval Augmented Generation ）的简称，它为大语言模型 (LLMs) 提供了从数据源检索信息的能力，并以此为基础生成回答。简而言之，RAG 结合了信息检索技术和大语言模型的提示功能，即模型根据搜索算法找到的信息作为上下文来查询回答问题。无论是查询还是检索的上下文，都会被整合到发给大语言模型的提示中。

![02a43de8239e3d7cdcf63585493e9364a5185c](https://img-blog.csdnimg.cn/img_convert/8a15bd10980833720ce40761c32f979d.webp?x-oss-process=image/format,png)

RAG 的架构如图中所示。它既不是一个特定的开源代码库，也不是某个特定的应用，是一个开发框架。

完整的 RAG 应用流程主要包含两个阶段：

数据准备阶段：（A）数据提取–> （B）分块（Chunking）–> （C）向量化（embedding）–> （D）数据入库

检索生成阶段：（1）问题向量化–> （2）根据问题查询匹配数据–> （3）获取索引数据 --> （4）将数据注入Prompt–> （5）LLM生成答案

下面让我们展开介绍一下这两个阶段的关键环节。

数据准备阶段
数据准备一般是一个离线的过程，主要是将私有数据向量化后构建索引并存入数据库的过程。主要包括：数据提取、文本分割、向量化、数据入库等环节。

1、数据提取：将 PDF、word、markdown、数据库和API等多种格式的数据，进行过滤、压缩、格式化等处理为同一个范式。

2、分块（Chunking）：将初始文档分割成一定大小的块，尽量不要失去语义含义。将文本分割成句子或段落，而不是将单个句子分成多部分。有多种文本分割器实现能够完成此任务。比如根据换行、句号、问号、感叹号等切分文本，或者以其他的合适大小的 chunk 为原则进行分割。最终将语料分割成 chunk 块，在检索时会取相关性最高的 top_n。

3、向量化(embedding)：将文本数据转化为向量矩阵的过程，该过程会直接影响到后续检索的效果。常用的 embedding 模型：moka-ai/m3e-base、GanymedeNil/text2vec-large-chinese，也可以参考 Hugging Face 推出的嵌入模型排行榜 MTEB Leaderboard。

4、数据入库：数据向量化后构建索引，并写入向量数据库的过程可以概述为数据入库，适用于 RAG 场景的向量数据库包括：facebookresearch/faiss（本地）、Chroma、Elasticsearch、Milvus 等。一般可以根据业务场景、硬件、性能需求等多因素综合考虑，选择合适的数据库。

应用阶段
在应用阶段，根据用户的提问，将提问问题向量化处理，然后通过高效的检索方法，从向量数据库中召回与提问最相关的知识，并融入 Prompt；大模型参考当前提问和相关知识，生成相应的答案。关键环节包括：数据检索、注入 Prompt 等。

1、数据检索

常见的数据检索方法包括：相似性检索、全文检索等。以及可以结合多种检索方式，提升召回率。

相似性检索：即计算查询向量与所有存储向量的相似性得分，返回得分高的记录。常见的相似性计算方法包括：余弦相似性、欧氏距离、曼哈顿距离等。

全文检索：全文检索是一种比较经典的检索方式，在数据存入时，通过关键词构建倒排索引；在检索时，通过关键词进行全文检索，找到对应的记录。

RAG 文本检索环节中的主流方法是相似性检索（向量检索），即语义相关度匹配的方式。想了解更多检索方式和检索的优化请查看文章，综述等文章。

2、注入 Prompt

Prompt 作为大模型的直接输入，是影响模型输出准确率的关键因素之一。在 RAG 场景中，Prompt 一般包括任务描述、背景知识（检索得到）、任务指令（一般是用户提问）等，根据任务场景和大模型性能，也可以在 Prompt 中适当加入其他指令优化大模型的输出。一个简单知识问答场景的 Prompt 如下所示：

```
prompt = f"""
Give the answer to the user query delimited by triple backticks ```{query}```\
using the information given in context delimited by triple backticks ```{context}```.\
If there is no relevant information in the provided context, try to answer yourself,
but tell user that you did not have any relevant context to base your answer on.
Be concise and output the answer of size less than 80 tokens.
"""   
```

Prompt 的设计只有方法、没有语法，比较依赖于个人经验，在实际应用过程中，往往需要根据大模型的实际输出进行针对性的 Prompt 调优。
