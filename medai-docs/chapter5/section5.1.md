# 5.5.1 工作流的节点1

## 开始

### 定义

为启动工作流设置初始参数。

在开始节点中，您可以自定义启动工作流的输入变量。每个工作流都需要一个开始节点。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fgit-blob-944f750dc564cef9df35307e8c429ee81d918936%252Fimage%2520%28236%29.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=9d211211&sv=1)

工作流开始节点

开始节点支持定义四种类型输入变量：

- 文本
- 段落
- 下拉选项
- 数字
- 文件（即将推出）

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fgit-blob-b408c8a4af094b5934b4df38b77e8d65e6d9435d%252Foutput%2520%282%29%2520%281%29.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=8d6df3b&sv=1)

配置开始节点的变量

配置完成后，工作流在执行时将提示您提供开始节点中定义的变量值。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FCXCgeW7CFIaA54tSQxd9%252Foutput%2520%283%29.png%3Falt%3Dmedia%26token%3Db1fe9654-ab6b-429b-9a91-5cf591f88d26&width=768&dpr=4&quality=100&sign=8a56e061&sv=1)



Tip: 在Chatflow中，开始节点提供了内置系统变量：`sys.query` 和 `sys.files`。

`sys.query` 用于对话应用中的用户输入问题。

`sys.files` 用于对话中的文件上传，如上传图片，这需要与图片理解模型配合使用。

## 结束

### 1 定义

定义一个工作流程结束的最终输出内容。每一个工作流在完整执行后都需要至少一个结束节点，用于输出完整执行的最终结果。

结束节点为流程终止节点，后面无法再添加其他节点，工作流应用中只有运行到结束节点才会输出执行结果。若流程中出现条件分叉，则需要定义多个结束节点。

结束节点需要声明一个或多个输出变量，声明时可以引用任意上游节点的输出变量。

Chatflow 内不支持结束节点

### 2 场景

在以下[长故事生成工作流](https://docs.dify.ai/v/zh-hans/guides/workflow/node/iteration#shi-li-2-chang-wen-zhang-die-dai-sheng-cheng-qi-ling-yi-zhong-bian-pai-fang-shi)中，结束节点声明的变量 `Output `为上游代码节点的输出，即该工作流会在 Code3 节点执行完成之后结束，并输出 Code3 的执行结果。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252F4f8N80WOjB6XxLIbMtXT%252Fimage.png%3Falt%3Dmedia%26token%3D8a072ee0-f703-4a6a-8c53-845e0425d77e&width=768&dpr=4&quality=100&sign=90041026&sv=1)

结束节点-长故事生成示例

**单路执行示例：**

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FXU2FP90nDFB3NZ068wNM%252Foutput.png%3Falt%3Dmedia%26token%3D8a3603e3-94c6-4c4f-8ae3-426965c70dc8&width=768&dpr=4&quality=100&sign=30559b2c&sv=1)

**多路执行示例：**

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FEhd0SOkQJDz5QwBHqaYR%252Foutput%2520%281%29.png%3Falt%3Dmedia%26token%3Db26f42ab-5211-41a6-bee2-36844c28a072&width=768&dpr=4&quality=100&sign=dac67deb&sv=1)

## 直接回复

### 定义

定义一个 Chatflow 流程中的回复内容。

你可以在文本编辑器中自由定义回复格式，包括自定义一段固定的文本内容、使用前置步骤中的输出变量作为回复内容、或者将自定义文本与变量组合后回复。

可随时加入节点将内容流式输出至对话回复，支持所见即所得配置模式并支持图文混排，如：

1. 输出 LLM 节点回复内容
2. 输出生成图片
3. 输出纯文本

**示例1：**输出纯文本

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FNXdxdUVmAVJeo1RgBe8h%252Foutput%2520%282%29.png%3Falt%3Dmedia%26token%3D698dee44-a0d9-48d0-bf1d-25a900623332&width=768&dpr=4&quality=100&sign=eb2cf06d&sv=1)

**示例2：**输出图片+LLM回复

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FfFd5pRCfr3w9AQgHelqB%252Fimage.png%3Falt%3Dmedia%26token%3D0fd9fabe-d002-46b3-ba49-6f3c28588fad&width=768&dpr=4&quality=100&sign=b5bb85b&sv=1)

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fng4yHIyEajPRZwlDyhxx%252Fimage.png%3Falt%3Dmedia%26token%3D4fb52765-66d2-4a82-b501-9da1ae66e679&width=768&dpr=4&quality=100&sign=e6b220b9&sv=1)



直接回复节点可以不作为最终的输出节点，作为流程过程节点时，可以在中间步骤流式输出结果。

## LLM

### 定义

调用大语言模型回答问题或者处理自然语言。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FyttBpZkssrPeYyXb8gNR%252Fimage.png%3Falt%3Dmedia%26token%3D5857f949-fada-47f3-b8aa-f82588a52b0d&width=768&dpr=4&quality=100&sign=1ce47ea3&sv=1)

LLM 节点

### 场景

LLM 是 Chatflow/Workflow 的核心节点，利用大语言模型的对话/生成/分类/处理等能力，根据给定的提示词处理广泛的任务类型，并能够在工作流的不同环节使用。

- **意图识别**，在客服对话情景中，对用户问题进行意图识别和分类，导向下游不同的流程。
- **文本生成**，在文章生成情景中，作为内容生成的节点，根据主题、关键词生成符合的文本内容。
- **内容分类**，在邮件批处理情景中，对邮件的类型进行自动化分类，如咨询/投诉/垃圾邮件。
- **文本转换**，在文本翻译情景中，将用户提供的文本内容翻译成指定语言。
- **代码生成**，在辅助编程情景中，根据用户的要求生成指定的业务代码，编写测试用例。
- **RAG**，在知识库问答情景中，将检索到的相关知识和用户问题重新组织回复问题。
- **图片理解**，使用 vision 能力的多模态模型，能对图像内的信息进行理解和问答。

选择合适的模型，编写提示词，你可以在 Chatflow/Workflow 中构建出强大、可靠的解决方案。


### 如何配置

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FWICBnO3xj7rnnDryr3mz%252Fimage.png%3Falt%3Dmedia%26token%3Db1146ff4-0505-4c10-bb9a-2b5238ee6ea9&width=768&dpr=4&quality=100&sign=1cc6354b&sv=1)

LLM 节点配置-选择模型

**配置步骤：**

1. **选择模型**，Dify 提供了全球主流模型的[支持](https://docs.dify.ai/v/zh-hans/getting-started/readme/model-providers)，包括 OpenAI 的 GPT 系列、Anthropic 的 Claude 系列、Google 的 Gemini 系列等，选择一个模型取决于其推理能力、成本、响应速度、上下文窗口等因素，你需要根据场景需求和任务类型选择合适的模型。

2. **配置模型参数**，模型参数用于控制模型的生成结果，例如温度、TopP，最大标记、回复格式等，为了方便选择系统同时提供了 3 套预设参数：创意，平衡和精确。

3. **编写提示词**，LLM 节点提供了一个易用的提示词编排页面，选择聊天模型或补全模型，会显示不同的提示词编排结构。

4. 

   **高级设置**，可以开关记忆，设置记忆窗口，使用 Jinja-2 模版语言来进行更复杂的提示词等。

如果你是初次使用 Dify ，在 LLM 节点选择模型之前，需要在 **系统设置—模型供应商** 内提前完成[模型配置](https://docs.dify.ai/v/zh-hans/guides/model-configuration)。

#### **编写提示词**

在 LLM 节点内，你可以自定义模型输入提示词。如果选择聊天模型（Chat model），你可以自定义系统提示词（SYSTEM）/用户（USER）/助手（ASSISTANT）三部分内容。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fgit-blob-8eaa71f92febfa89028c746c640c264eb922c893%252Fzh-node-llm.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=456db19c&sv=1)

**提示生成器**

如果在编写系统提示词（SYSTEM）时没有好的头绪，也可以使用提示生成器功能，借助 AI 能力快速生成适合实际业务场景的提示词。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fgit-blob-8733408a1c49119a44f1ab6f1dd19052b60e54ef%252Fzh-node-llm-prompt-generator.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=b90ddf77&sv=1)

在提示词编辑器中，你可以通过输入 **“/”** 或者 **“{”** 呼出 **变量插入菜单**，将 **特殊变量块** 或者 **上游节点变量** 插入到提示词中作为上下文内容。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fryjx7hQLPGZ3GsxDLKLW%252Fimage.png%3Falt%3Dmedia%26token%3D7317926c-d52f-44d5-aa94-53beca8debb1&width=768&dpr=4&quality=100&sign=f905862&sv=1)

呼出变量插入菜单


### 特殊变量说明

**上下文变量**

上下文变量是 LLM 节点内定义的特殊变量类型，用于在提示词内插入外部检索的文本内容。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fg33ASbKfu0J98l9tvrtU%252Fimage.png%3Falt%3Dmedia%26token%3Dff17bbfc-bdc6-4c98-9f4b-74a339a25ab9&width=768&dpr=4&quality=100&sign=9eac3e6f&sv=1)

上下文变量

在常见的知识库问答应用中，知识库检索的下游节点一般为 LLM 节点，知识检索的 **输出变量** `result` 需要配置在 LLM 节点中的 **上下文变量** 内关联赋值。关联后在提示词的合适位置插入 **上下文变量** ，可以将外部检索到的知识插入到提示词中。

该变量除了可以作为 LLM 回复问题时的提示词上下文作为外部知识引入，由于其数据结构中包含了分段引用信息，同时可以支持应用端的 [**引用与归属**](https://docs.dify.ai/v/zh-hans/guides/knowledge-base/retrieval-test-and-citation#id-2-yin-yong-yu-gui-shu) 功能。



若上下文变量关联赋值的是上游节点的普通变量，例如开始节点的字符串类型变量，则上下文的变量同样可以作为外部知识引入，但 **引用与归属** 功能将会失效。

**会话历史**

为了在文本补全类模型（例如 gpt-3.5-turbo-Instruct）内实现聊天型应用的对话记忆，Dify 在原[提示词专家模式（已下线）](https://github.com/langgenius/dify-docs/blob/main/zh_CN/learn-more/extended-reading/prompt-engineering/prompt-engineering-1/README.md)内设计了会话历史变量，该变量沿用至 Chatflow 的 LLM 节点内，用于在提示词中插入 AI 与用户之间的聊天历史，帮助 LLM 理解对话上文。



会话历史变量应用并不广泛，仅在 Chatflow 中选择文本补全类模型时可以插入使用。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252F7WZRPjtT9Zt3FFt03iWs%252Fimage.png%3Falt%3Dmedia%26token%3De95e4699-93d2-4948-bc3f-3eb6768bd9aa&width=768&dpr=4&quality=100&sign=d4fe6e6&sv=1)

插入会话历史变量

### 高级功能

**记忆：** 开启记忆后问题分类器的每次输入将包含对话中的聊天历史，以帮助 LLM 理解上文，提高对话交互中的问题理解能力。

**记忆窗口：** 记忆窗口关闭时，系统会根据模型上下文窗口动态过滤聊天历史的传递数量；打开时用户可以精确控制聊天历史的传递数量（对数）。

**对话角色名设置：** 由于模型在训练阶段的差异，不同模型对于角色名的指令遵循程度不同，如 Human/Assistant，Human/AI，人类/助手等等。为适配多模型的提示响应效果，系统提供了对话角色名的设置，修改对话角色名将会修改会话历史的角色前缀。

**Jinja-2 模板：** LLM 的提示词编辑器内支持 Jinja-2 模板语言，允许你借助 Jinja2 这一强大的 Python 模板语言，实现轻量级数据转换和逻辑处理，参考[官方文档](https://jinja.palletsprojects.com/en/3.1.x/templates/)。

## 知识检索

### 1 定义

从知识库中检索与用户问题相关的文本内容，可作为下游 LLM 节点的上下文来使用。

### 2 场景

常见情景：构建基于外部数据/知识的 AI 问答系统（RAG）。了解更多关于 RAG 的[基本概念](https://docs.dify.ai/v/zh-hans/learn-more/extended-reading/retrieval-augment)。

下图为一个最基础的知识库问答应用示例，该流程的执行逻辑为：知识库检索作为 LLM 节点的前置步骤，在用户问题传递至 LLM 节点之前，先在知识检索节点内将匹配用户问题最相关的文本内容并召回，随后在 LLM 节点内将用户问题与检索到的上下文一同作为输入，让 LLM 根据检索内容来回复问题。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fy2YRW5rl2NjIn9VFnPfo%252Fimage.png%3Falt%3Dmedia%26token%3D91c67785-0e38-457c-a4a4-3c3a4325f144&width=768&dpr=4&quality=100&sign=a520652a&sv=1)

知识库问答应用示例


### 3 如何配置

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FN3MJuWEfxCxIJDFXZE4y%252Fimage.png%3Falt%3Dmedia%26token%3Dcdf63108-8a18-447f-95f6-8e4c2b4bfd4f&width=768&dpr=4&quality=100&sign=794c2420&sv=1)

知识检索配置

**配置流程：**

1. 选择查询变量，用于作为输入来检索知识库中的相关文本分段，在常见的对话类应用中一般将开始节点的 `sys.query` 作为查询变量；
2. 选择需要查询的知识库，可选知识库需要在 Dify 知识库内预先[创建](https://github.com/langgenius/dify-docs/blob/main/zh_CN/guides/knowledge-base/create-knowledge-and-upload-documents/README.md#id-1-chuang-jian-zhi-shi-ku)；
3. 指定[召回模式](https://docs.dify.ai/v/zh-hans/learn-more/extended-reading/retrieval-augment/retrieval)。自 9 月 1 日后，知识库的召回模式将自动切换为多路召回，不再建议使用 N 选 1 召回模式；
4. 连接并配置下游节点，一般为 LLM 节点；

> 建议将知识库的召回模式切换为多路召回，详细说明请参考[《在应用内集成知识库》](https://docs.dify.ai/v/zh-hans/guides/knowledge-base/integrate-knowledge-within-application)。

**输出变量**

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FNAy83340FWTooMtC3FQh%252Fimage.png%3Falt%3Dmedia%26token%3D7251b1e0-e2be-49ed-8c03-2e34cd531784&width=768&dpr=4&quality=100&sign=bf455616&sv=1)

输出变量

知识检索的输出变量 `result` 为从知识库中检索到的相关文本分段。其变量数据结构中包含了分段内容、标题、链接、图标、元数据信息。

**配置下游节点**

在常见的对话类应用中，知识库检索的下游节点一般为 LLM 节点，知识检索的**输出变量** `result` 需要配置在 LLM 节点中的 **上下文变量** 内关联赋值。关联后你可以在提示词的合适位置插入 **上下文变量**。

上下文变量是 LLM 节点内定义的特殊变量类型，用于在提示词内插入外部检索的文本内容。

当用户提问时，若在知识检索中召回了相关文本，文本内容会作为上下文变量中的值填入提示词，提供 LLM 回复问题；若未在知识库检索中召回相关的文本，上下文变量值为空，LLM 则会直接回复用户问题。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FlRTJjSXjSN5XZBLqzr1r%252Fimage.png%3Falt%3Dmedia%26token%3D50091044-e33b-4123-bb81-50fd544e8767&width=768&dpr=4&quality=100&sign=832b9046&sv=1)

配置下游 LLM 节点

该变量除了可以作为 LLM 回复问题时的提示词上下文作为外部知识参考引用，另外由于其数据结构中包含了分段引用信息，同时可以支持应用端的 [**引用与归属**](https://docs.dify.ai/v/zh-hans/guides/knowledge-base/retrieval-test-and-citation#id-2-yin-yong-yu-gui-shu) 功能。



## 问题分类

### 1 **定义**

通过定义分类描述，问题分类器能够根据用户输入推理与之相匹配的分类并输出分类结果。

### **2 场景**

常见的使用情景包括**客服对话意图分类、产品评价分类、邮件批量分类**等。

在一个典型的产品客服问答场景中，问题分类器可以作为知识库检索的前置步骤，对用户输入问题意图进行分类处理，分类后导向下游不同的知识库查询相关的内容，以精确回复用户的问题。

下图为产品客服场景的示例工作流模板：

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252F6MBrOasr0hrrz8oAqxjD%252Fimage.png%3Falt%3Dmedia%26token%3De3c647f9-9ea2-4b1e-b75f-0f57ba133693&width=768&dpr=4&quality=100&sign=d45fdf78&sv=1)

在该场景中我们设置了 3 个分类标签/描述：

- 分类 1 ：**与售后相关的问题**

- 分类 2：**与产品操作使用相关的问题**

- 分类 3 ：**其他问题**

当用户输入不同的问题时，问题分类器会根据已设置的分类标签/描述自动完成分类：

- “**iPhone 14 如何设置通讯录联系人？**” —> “**与产品操作使用相关的问题**”

- “**保修期限是多久？**” —> “**与售后相关的问题**”

- “**今天天气怎么样？**” —> “**其他问题**”

### 3 如何配置

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FDTPZf8fZBIY0BcloeXfY%252Fimage.png%3Falt%3Dmedia%26token%3D69583f5e-6cea-443c-b9d6-2b90d467840c&width=768&dpr=4&quality=100&sign=67c0cfb&sv=1)

**配置步骤：**

1. **选择输入变量**，指用于分类的输入内容，客服问答场景下一般为用户输入的问题 `sys.query`;
2. **选择推理模型**，问题分类器基于大语言模型的自然语言分类和推理能力，选择合适的模型将有助于提升分类效果；
3. **编写分类标签/描述**，你可以手动添加多个分类，通过编写分类的关键词或者描述语句，让大语言模型更好的理解分类依据。
4. **选择分类对应的下游节点，**问题分类节点完成分类之后，可以根据分类与下游节点的关系选择后续的流程路径。

#### **高级设置：**

**指令：**你可以在 **高级设置-指令** 里补充附加指令，比如更丰富的分类依据，以增强问题分类器的分类能力。

**记忆：**开启记忆后问题分类器的每次输入将包含对话中的聊天历史，以帮助 LLM 理解上文，提高对话交互中的问题理解能力。

**记忆窗口：**记忆窗口关闭时，系统会根据模型上下文窗口动态过滤聊天历史的传递数量；打开时用户可以精确控制聊天历史的传递数量（对数）。

**输出变量：**

```
class_name
```

即分类之后输出的分类名。你可以在下游节点需要时使用分类结果变量。

## 条件分支

### 定义

根据 if/else/elif 条件将 workflow 拆分成多个分支。

条件分支节点有六个部分：

- IF 条件：选择变量，设置条件和满足条件的值；
- IF 条件判断为 `True`，执行 IF 路径；
- IF 条件判断为 `False`，执行 ELSE 路径；
- ELIF 条件判断为 `True`，执行 ELIF 路径；
- ELIF 条件判断为 `False`，继续判断下一个 ELIF 路径或执行最后的 ELSE 路径；

**条件类型**

- 包含（Contains）
- 不包含（Not contains）
- 开始是（Start with）
- 结束是（End with）
- 是（Is）
- 不是（Is not）
- 为空（Is empty）
- 不为空（Is not empty）

### 场景

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fgit-blob-aa142f8456d883aae695d682e0767aee83b56528%252Fzh-if-else-elif.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=5f9f405b&sv=1)

以**文本总结工作流**作为示例说明各个条件：

- IF 条件： 选择开始节点中的 `summarystyle` 变量，条件为**包含** `技术`；
- IF 条件判断为 `True`，执行 IF 路径，通过知识检索节点查询技术相关知识再到 LLM 节点回复（图中上半部分）；
- IF 条件判断为 `False`，但添加了 `ELIF` 条件，即 `summarystyle` 变量输入**不包含**`技术`，但 `ELIF` 条件内包含 `科技`，会检查 `ELIF` 内的条件是否为 `True`，然后执行路径内定义的步骤；
- `ELIF` 内的条件为 `False`，即输入变量既不不包含 `技术`，也不包含 `科技`，继续判断下一个 ELIF 路径或执行最后的 ELSE 路径；
- IF 条件判断为 `False`，即 `summarystyle` 变量输入**不包含** `技术`，执行 ELSE 路径，通过 LLM2 节点进行回复（图中下半部分）；

**多重条件判断**

涉及复杂的条件判断时，可以设置多重条件判断，在条件之间设置**AND**或者**OR**，即在条件之间取**交集**或者**并集**。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FLWL1vFLw4T02RB8DJJOQ%252Fimage.png%3Falt%3Dmedia%26token%3D86d368d3-0e96-491d-93c0-a92da16559c2&width=768&dpr=4&quality=100&sign=538b6b9d&sv=1)

多重条件判断
