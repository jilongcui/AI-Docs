# 3.2 实践示例

那具体 RAG 怎么做呢？我们用一个简单的 LangChain 代码示例来展示 RAG 的使用。

**环境准备**

安装相关依赖

```
# 环境准备，安装相关依赖
pip install langchain sentence_transformers chromadb 
```

**本地数据加载**
这个例子使用了保罗·格雷厄姆（Paul Graham）的文章"What I Worked On"的文本。下载文本后，放置到"./data"目录下。Langchain 提供了很多文件加载器，包括 word、csv、PDF、GoogleDrive、Youtube等，使用方法也很简单。这里直接使用 TextLoader 加载txt文本。

```
from langchain.document_loaders import TextLoader
loader = TextLoader("./data/paul_graham_essay.txt")
documents = loader.load() 
```

**文档分割(split_documents)**

文档分割，借助 langchain 的字符分割器。代码中我们指定 chunk_size=500, chunk_overlap=10， 这样的意思就是我们每块的文档中是 500 个字符，chunk_overlap 表示字符重复的个数，这样可以避免语义被拆分后不完整。

```
# 文档分割
from langchain.text_splitter import CharacterTextSplitter
# 创建拆分器
text_splitter = CharacterTextSplitter(chunk_size=500, chunk_overlap=10)
# 拆分文档
documents = text_splitter.split_documents(documents)   
```

**向量化(embedding)**
接下来对分割后的数据进行 embedding，并写入数据库。LangChain 提供了许多嵌入模型的接口，例如 OpenAI、Cohere、Hugging Face、Weaviate等，请参考 LangChain 官网。这里选用 m3e-base 作为 embedding 模型，向量数据库选用 Chroma。

```
from langchain.embeddings import HuggingFaceBgeEmbeddings
from langchain.vectorstores import Chroma
# embedding model: m3e-base
model_name = "moka-ai/m3e-base"
model_kwargs = {'device': 'cpu'}
encode_kwargs = {'normalize_embeddings': True}
embedding = HuggingFaceBgeEmbeddings(
		model_name=model_name,    
    model_kwargs=model_kwargs,   
    encode_kwargs=encode_kwargs
) 
```

**数据入库**

将嵌入后的结果存储在 VectorDB 中，常见的 VectorDB包括 Chroma、weaviate 和 FAISS 等，这里使用 Chroma 来实现。Chroma 与 LangChain 整合得很好，可以直接使用 LangChain 的接口进行操作。

```
# 指定 persist_directory 将会把嵌入存储到磁盘上。
persist_directory = 'db'
db = Chroma.from_documents(documents, embedding, persist_directory=persist_directory) 
```

**检索(Retrieve)**
向量数据库被填充后，可以将其定义为检索器组件，该组件根据用户查询与嵌入式块之间的语义相似性获取附加上下文。

```
retriever = db.as_retriever() 
```

**增强(Augment)**
接下来，为了将附加上下文与提示一起使用，需要准备一个提示模板。如下所示，可以轻松地从提示模板自定义提示。

```
from langchain.prompts import ChatPromptTemplate

template = """You are an assistant for question-answering tasks.
Use the following pieces of retrieved context to answer the question.
If you don't know the answer, just say that you don't know.
Use three sentences maximum and keep the answer concise.
Question: {question}
Context: {context}
Answer:   """
prompt = ChatPromptTemplate.from_template(template) 
```

**生成(Generate)**
最后，可以构建一个 RAG 流水线的链，将检索器、提示模板和LLM连接在一起。一旦定义了 RAG 链，就可以调用它。本地通过 ollama 运行的 llama3 来作为 LLM 使用。如果不了解本地ollama部署模型的流程，可以参考这篇文章。

```
from langchain_community.chat_models import ChatOllama
from langchain.schema.runnable import RunnablePassthrough
from langchain.schema.output_parser import StrOutputParser
llm = ChatOllama(model='llama3')
rag_chain = ( 
		{"context": retriever, "question": RunnablePassthrough()}
		| prompt
		| llm
		| StrOutputParser()
)

query = "What did the author do growing up?"
response = rag_chain.invoke(query)
print(response) 
```


我这里的本地llama3环境下，输出为：

```
Before college, Paul Graham worked on writing and programming outside of school. He didn't write essays, but instead focused on writing short stories. His stories were not very good, having little plot and just characters with strong feelings. 
```

从这个输出中，可以看到已经将我们提供的文本中的相关信息检索出来，并由 LLM 总结回答我们的问题了。
