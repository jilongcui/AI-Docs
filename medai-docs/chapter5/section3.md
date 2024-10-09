# 5.3 创建智能助手应用

### 定义

智能助手（Agent Assistant），利用大语言模型的推理能力，能够自主对复杂的人类任务进行目标规划、任务拆解、工具调用、过程迭代，并在没有人类干预的情况下完成任务。

### 如何使用智能助手

为了方便快速上手使用，您可以在“探索”中找到智能助手的应用模板，添加到自己的工作区，或者在此基础上进行自定义。在全新的 Dify 工作室中，你也可以从零编排一个专属于你自己的智能助手，帮助你完成财务报表分析、撰写报告、Logo 设计、旅程规划等任务。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252F1rDfIo6VdoyC8Cruo2Qs%252Fimage.png%3Falt%3Dmedia%26token%3D89d2f0b2-5996-4016-9626-e399229e5a22&width=768&dpr=4&quality=100&sign=474c2e8c&sv=1)

探索-智能助手应用模板

选择智能助手的推理模型，智能助手的任务完成能力取决于模型推理能力，我们建议在使用智能助手时选择推理能力更强的模型系列如 gpt-4 以获得更稳定的任务完成效果。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FHx96Ap6wSOGXY8yLDQtb%252Fimage.png%3Falt%3Dmedia%26token%3D1055aeec-88ac-4911-933a-5555cf466d5b&width=768&dpr=4&quality=100&sign=3e32aafe&sv=1)

选择智能助手的推理模型

你可以在“提示词”中编写智能助手的指令，为了能够达到更优的预期效果，你可以在指令中明确它的任务目标、工作流程、资源和限制等。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FnFw6N5xnyzvTJqErgwop%252Fimage.png%3Falt%3Dmedia%26token%3D657ba5b0-4be7-40c7-97e4-c81cdd75ed0e&width=768&dpr=4&quality=100&sign=3f6ddca3&sv=1)

编排智能助手的指令提示词

### 添加助手需要的工具

在“上下文”中，你可以添加智能助手可以用于查询的知识库工具，这将帮助它获取外部背景知识。

在“工具”中，你可以添加需要使用的工具。工具可以扩展 LLM 的能力，比如联网搜索、科学计算或绘制图片，赋予并增强了 LLM 连接外部世界的能力。Dify 提供了两种工具类型：**第一方工具**和**自定义工具**。

你可以直接使用 Dify 生态提供的第一方内置工具，或者轻松导入自定义的 API 工具（目前支持 OpenAPI / Swagger 和 OpenAI Plugin 规范）。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FHlv3pfrJnt0E8kIZDvl1%252Fimage.png%3Falt%3Dmedia%26token%3D46208b01-948a-4fe8-898f-79d919db8017&width=768&dpr=4&quality=100&sign=532e0c6e&sv=1)

添加助手需要的工具

“工具”功能允许用户借助外部能力，在 Dify 上创建出更加强大的 AI 应用。例如你可以为智能助理型应用（Agent）编排合适的工具，它可以通过任务推理、步骤拆解、调用工具完成复杂任务。

另外工具也可以方便将你的应用与其他系统或服务连接，与外部环境交互。例如代码执行、对专属信息源的访问等。你只需要在对话框中谈及需要调用的某个工具的名字，即可自动调用该工具。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fgit-blob-11402a369e05b048134e03310bdb103f51433ead%252Fzh-agent-dalle3.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=d241ac5c&sv=1)

### 配置 Agent

在 Dify 上为智能助手提供了 Function calling（函数调用）和 ReAct 两种推理模式。已支持 Function Call 的模型系列如 gpt-3.5/gpt-4 拥有效果更佳、更稳定的表现，尚未支持 Function calling 的模型系列，我们支持了 ReAct 推理框架实现类似的效果。

在 Agent 配置中，你可以修改助手的迭代次数限制。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FYlPt8WaLb6ZegMURbYJt%252Fimage.png%3Falt%3Dmedia%26token%3Dbec14b7d-6927-4072-a241-3dbed22be048&width=768&dpr=4&quality=100&sign=d52b1d93&sv=1)

Function Calling 模式

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FAGnLezaffjvXtSjNnU3V%252Fimage.png%3Falt%3Dmedia%26token%3Dde9d487b-cca4-4a1e-a52b-4a718ae81802&width=768&dpr=4&quality=100&sign=9eaba6d1&sv=1)

ReAct 模式

### 配置对话开场白

您可以为智能助手配置一套会话开场白和开场问题，配置的对话开场白将在每次用户初次对话中展示助手可以完成什么样的任务，以及可以提出的问题示例。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252F9ZvSQoM1ZOfmVkLLU7hO%252Fimage.png%3Falt%3Dmedia%26token%3D31fc2005-4e11-4017-9992-e6940fe7644d&width=768&dpr=4&quality=100&sign=14c1a5c3&sv=1)

配置会话开场白和开场问题

### 调试与预览

编排完智能助手之后，你可以在发布成应用之前进行调试与预览，查看助手的任务完成效果。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FBKxh49kE9hpSW09vjpFG%252Fimage.png%3Falt%3Dmedia%26token%3Db28612b3-004d-4375-9e89-76e300fc6f6c&width=768&dpr=4&quality=100&sign=df365547&sv=1)

调试与预览

### 应用发布

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252F7TZnwLs0MLxluuUYoqlC%252Fimage.png%3Falt%3Dmedia%26token%3D5a8efe0d-d5fc-4f81-8344-41b8c452f92a&width=768&dpr=4&quality=100&sign=f71fabf3&sv=1)

应用发布为 Webapp
