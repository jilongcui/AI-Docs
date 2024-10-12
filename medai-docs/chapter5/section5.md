# 5.5 工作流的节点

**节点是工作流中的关键构成**，通过连接不同功能的节点，执行工作流的一系列操作。

### 核心节点

[**开始（Start）**](https://docs.dify.ai/v/zh-hans/guides/workflow/node/start)

定义一个 workflow 流程启动的初始参数。

[**结束（End）**](https://docs.dify.ai/v/zh-hans/guides/workflow/node/end)

定义一个 workflow 流程结束的最终输出内容。

[**回复（Answer）**](https://docs.dify.ai/v/zh-hans/guides/workflow/node/answer)

定义一个 Chatflow 流程中的回复内容。

[**大语言模型（LLM）**](https://docs.dify.ai/v/zh-hans/guides/workflow/node/llm)

调用大语言模型回答问题或者对自然语言进行处理。

[**知识检索（Knowledge Retrieval）**](https://docs.dify.ai/v/zh-hans/guides/workflow/node/knowledge-retrieval)

从知识库中检索与用户问题相关的文本内容，可作为下游 LLM 节点的上下文。

[**问题分类（Question Classifier）**](https://docs.dify.ai/v/zh-hans/guides/workflow/node/question-classifier)

通过定义分类描述，LLM 能够根据用户输入选择与之相匹配的分类。

[**条件分支（IF/ELSE）**](https://docs.dify.ai/v/zh-hans/guides/workflow/node/ifelse)

允许你根据 if/else 条件将 workflow 拆分成两个分支。

[**代码执行（Code）**](https://docs.dify.ai/v/zh-hans/guides/workflow/node/code)

运行 Python / NodeJS 代码以在工作流程中执行数据转换等自定义逻辑。

[**模板转换（Template）**](https://docs.dify.ai/v/zh-hans/guides/workflow/node/template)

允许借助 Jinja2 的 Python 模板语言灵活地进行数据转换、文本处理等。

[**变量聚合（Variable Aggregator）**](https://docs.dify.ai/v/zh-hans/guides/workflow/node/variable-assigner)

将多路分支的变量聚合为一个变量，以实现下游节点统一配置。

[**参数提取器（Parameter Extractor）**](https://docs.dify.ai/v/zh-hans/guides/workflow/node/parameter-extractor)

利用 LLM 从自然语言推理并提取结构化参数，用于后置的工具调用或 HTTP 请求。

[**迭代（Iteration）**](https://docs.dify.ai/v/zh-hans/guides/workflow/node/iteration)

对列表对象执行多次步骤直至输出所有结果。

[**HTTP 请求（HTTP Request）**](https://docs.dify.ai/v/zh-hans/guides/workflow/node/http-request)

允许通过 HTTP 协议发送服务器请求，适用于获取外部检索结果、webhook、生成图片等情景。

[**工具（Tools）**](https://docs.dify.ai/v/zh-hans/guides/workflow/node/tools)

允许在工作流内调用 MedAI 内置工具、自定义工具、子工作流等。

[**变量赋值（Variable Assigner）**](https://docs.dify.ai/v/zh-hans/guides/workflow/node/variable-assignment)

变量赋值节点用于向可写入变量（例如会话变量）进行变量赋值。

