# 5.5.3 工作流的节点3

## 迭代

### 定义

对数组执行多次步骤直至输出所有结果。

迭代步骤在列表中的每个条目（item）上执行相同的步骤。使用迭代的条件是确保输入值已经格式化为列表对象。迭代节点允许 AI 工作流处理更复杂的处理逻辑，迭代节点是循环节点的友好版本，它在自定义程度上做出了一些妥协，以便非技术用户能够快速入门。

### 场景

#### **示例1：长文章迭代生成器**

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fgit-blob-5cfc9b6665f71a31713ff432a4499ae8eb92252f%252Fiteration-story-generator.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=61b9259a&sv=1)

长故事生成器

1. 在 **开始节点** 内输入故事标题和大纲
2. 使用 **LLM 节点=** 基于用户输入的故事标题和大纲，让 LLM 开始编写内容
3. 使用 **参数提取节点** 将 LLM 输出的完整内容转换成数组格式
4. 通过 **迭代节点** 包裹的 **LLM 节点** 循环多次生成各章节内容
5. 将 **直接回复** 节点添加在迭代节点内部，实现在每轮迭代生成之后流式输出

**具体配置步骤**

1. 在 **开始节点** 配置故事标题（title）和大纲（outline）；

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FZyUmRrrXA51pBBR2f9BD%252Fimage.png%3Falt%3Dmedia%26token%3Dfeced52a-be84-46f4-a422-353c2a6ef901&width=768&dpr=4&quality=100&sign=d4d29d11&sv=1)

开始节点配置

1. 选择 **LLM 节点** 基于用户输入的故事标题和大纲，让 LLM 开始编写文本；

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fgit-blob-f1da18cc64e75cfa47cecea06be82d732fac14a9%252Fiteration-llm-node.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=8d88594f&sv=1)

模板节点

1. 选择 **参数提取节点**，将故事文本转换成为数组（Array）结构。提取参数为 `sections` ，参数类型为 `Array[Object]`

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fgit-blob-1196e7548a1eae3080f991e713f5205238872091%252Fzh-iteration-extract-node.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=814cd6fb&sv=1)

参数提取

参数提取效果受模型推理能力和指令影响，使用推理能力更强的模型，在**指令**内增加示例可以提高参数提取的效果。

1. 将数组格式的故事大纲作为迭代节点的输入，在迭代节点内部使用 **LLM 节点** 进行处理

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FP5laDJ3dY8MhaDfMR5Bl%252Fimage.png%3Falt%3Dmedia%26token%3D4e7f6cda-df30-477d-87b7-b87d8f002cf6&width=768&dpr=4&quality=100&sign=d8698d2c&sv=1)

配置迭代节点

在 LLM 节点内配置输入变量 `GenerateOverallOutline/output` 和 `Iteration/item`

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Ff6nwbE9V9yxIyAWyRsOu%252Fimage.png%3Falt%3Dmedia%26token%3D64ece23c-4165-41c1-aebf-a55f4ab56857&width=768&dpr=4&quality=100&sign=d5d41d6b&sv=1)

配置 LLM 节点

迭代的内置变量：`items[object]` 和 `index[number]`

```
items[object] 代表以每轮迭代的输入条目；
index[number] 代表当前迭代的轮次；
```

1. 在迭代节点内部配置 **直接回复节点** ，可以实现在每轮迭代生成之后流式输出。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FGe8grfAWj817KLAdXmxX%252Fimage.png%3Falt%3Dmedia%26token%3D5b2e2baa-92b0-4d91-b28a-a4510377176d&width=768&dpr=4&quality=100&sign=7be9f2d1&sv=1)

配置 Answer 节点

1. 完整调试和预览

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252F8g9SrjsxFUisxQEXfB1K%252Fimage.png%3Falt%3Dmedia%26token%3D47d4ecac-3a83-4c1d-bd77-4f281def437a&width=768&dpr=4&quality=100&sign=526f4147&sv=1)

按故事章节多轮迭代生成

#### **示例 2：长文章迭代生成器（另一种编排方式）**

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252F43HQqmncRrD9JEX4MTaD%252Fimage.png%3Falt%3Dmedia%26token%3D59e1881c-fea3-4da1-a320-63bd65bc8d7b&width=768&dpr=4&quality=100&sign=a654bfd3&sv=1)

- 在 **开始节点** 内输入故事标题和大纲
- 使用 **LLM 节点** 生成文章小标题，以及小标题对应的内容
- 使用 **代码节点** 将完整内容转换成数组格式
- 通过 **迭代节点** 包裹的 **LLM 节点** 循环多次生成各章节内容
- 使用 **模板转换** 节点将迭代节点输出的字符串数组转换为字符串
- 在最后添加 **直接回复节点** 将转换后的字符串直接输出

### 什么是数组内容

列表是一种特定的数据类型，其中的元素用逗号分隔，以 `[` 开头，以 `]` 结尾。例如：

**数字型：**

Copy

```
[0,1,2,3,4,5]
```

**字符串型：**

Copy

```
["monday", "Tuesday", "Wednesday", "Thursday"]
```

**JSON 对象：**

Copy

```
[
    {
        "name": "Alice",
        "age": 30,
        "email": "alice@example.com"
    },
    {
        "name": "Bob",
        "age": 25,
        "email": "bob@example.com"
    },
    {
        "name": "Charlie",
        "age": 35,
        "email": "charlie@example.com"
    }
]
```

### 支持返回数组的节点

- 代码节点
- 参数提取
- 知识库检索
- 迭代
- 工具
- HTTP 请求

### 如何获取数组格式的内容

**使用 CODE 节点返回**

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FkaQXOvisQeDYWSIOM5I0%252Fimage.png%3Falt%3Dmedia%26token%3D5408f962-6b2d-4369-af87-e4ebe38a69c0&width=768&dpr=4&quality=100&sign=6a3ff090&sv=1)

code 节点输出 array

**使用 参数提取 节点返回**

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FgSM6G00R5FqVYG3pziA6%252Fimage.png%3Falt%3Dmedia%26token%3Da2843fe1-3ada-44f4-a7aa-e5815b613306&width=768&dpr=4&quality=100&sign=b0c0c3ea&sv=1)

参数提取节点输出 array

### 如何将数组转换为文本

迭代节点的输出变量为数组格式，无法直接输出。你可以使用一个简单的步骤将数组转换回文本。

**使用代码节点转换**

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FZ7T100WWSRCDLQqvPMrz%252Fimage.png%3Falt%3Dmedia%26token%3Df66ca349-3774-4ae2-8e02-62a36d1d7034&width=768&dpr=4&quality=100&sign=70f5d1cb&sv=1)

代码节点转换

代码示例：

Copy

```
def main(articleSections: list):
    data = articleSections
    return {
        "result": "\n".join(data)
    }
```

**使用模板节点转换**

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FjiEMiq8G5N2Xh0dihnaV%252Fimage.png%3Falt%3Dmedia%26token%3Dd2d57628-8a50-4a99-95ed-c9468a966278&width=768&dpr=4&quality=100&sign=f427d221&sv=1)

模板节点转换

代码示例：

Copy

```
{{ articleSections | join("\n") }}
```

## 参数提取

### 1 定义

利用 LLM 从自然语言推理并提取结构化参数，用于后置的工具调用或 HTTP 请求。

Dify 工作流内提供了丰富的[工具](https://docs.dify.ai/v/zh-hans/guides/tools)选择，其中大多数工具的输入为结构化参数，参数提取器可以将用户的自然语言转换为工具可识别的参数，方便工具调用。

工作流内的部分节点有特定的数据格式传入要求，如[迭代](https://docs.dify.ai/v/zh-hans/guides/workflow/node/iteration#id-1-ding-yi)节点的输入要求为数组格式，参数提取器可以方便的实现[结构化参数的转换](https://docs.dify.ai/v/zh-hans/guides/workflow/node/iteration#id-2-chang-jing)。

### 2 场景

1. **从自然语言中提供工具所需的关键参数提取**，如构建一个简单的对话式 Arxiv 论文检索应用。

在该示例中：Arxiv 论文检索工具的输入参数要求为 **论文作者** 或 **论文编号**，参数提取器从问题“这篇论文中讲了什么内容：2405.10739”中提取出论文编号 **2405.10739**，并作为工具参数进行精确查询。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FOxVJeE4qdFKJLAYrF6ie%252Fimage.png%3Falt%3Dmedia%26token%3Dd3db72bc-9f77-4acd-a657-fcc09fe212e7&width=768&dpr=4&quality=100&sign=850facfa&sv=1)

Arxiv 论文检索工具

1. **将文本转换为结构化数据**，如长故事迭代生成应用中，作为[迭代节点](https://docs.dify.ai/v/zh-hans/guides/workflow/node/iteration)的前置步骤，将文本格式的章节内容转换为数组格式，方便迭代节点进行多轮生成处理。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FNItl3xv0JekSRpLq2WIk%252Fimage.png%3Falt%3Dmedia%26token%3D94962801-f74a-47e8-9792-fba193394e3a&width=768&dpr=4&quality=100&sign=7032fb19&sv=1)

1. **提取结构化数据并使用** [**HTTP 请求**](https://docs.dify.ai/v/zh-hans/guides/workflow/node/http-request) ，可请求任意可访问的 URL ，适用于获取外部检索结果、webhook、生成图片等情景。

### 3 如何配置

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fe5CftZx5dBlFp2utRN5e%252Fimage.png%3Falt%3Dmedia%26token%3D0fc38dc5-9c9c-4676-aae0-fdfbadf715f2&width=768&dpr=4&quality=100&sign=432d9aae&sv=1)

**配置步骤**

1. 选择输入变量，一般为用于提取参数的变量输入
2. 选择模型，参数提取器的提取依靠的是 LLM 的推理和结构化生成能力
3. 定义提取参数，可以手动添加需要提取的参数，也可以**从已有工具中快捷导入**
4. 编写指令，在提取复杂的参数时，编写示例可以帮助 LLM 提升生成的效果和稳定性

**高级设置**

**推理模式**

部分模型同时支持两种推理模式，通过函数/工具调用或是纯提示词的方式实现参数提取，在指令遵循能力上有所差别。例如某些模型在函数调用效果欠佳的情况下可以切换成提示词推理。

- Function Call/Tool Call
- Prompt

**记忆**

开启记忆后问题分类器的每次输入将包含对话中的聊天历史，以帮助 LLM 理解上文，提高对话交互中的问题理解能力。

**输出变量**

- 提取定义的变量
- 节点内置变量

`__is_success Number 提取是否成功` 成功时值为 1，失败时值为 0。

`__reason String` 提取错误原因

## HTTP 请求

### 1 定义

允许通过 HTTP 协议发送服务器请求，适用于获取外部数据、webhook、生成图片、下载文件等情景。它让你能够向指定的网络地址发送定制化的 HTTP 请求，实现与各种外部服务的互联互通。

该节点支持常见的 HTTP 请求方法：

- **GET**，用于请求服务器发送某个资源。
- **POST**，用于向服务器提交数据，通常用于提交表单或上传文件。
- **HEAD**，类似于 GET 请求，但服务器不返回请求的资源主体，只返回响应头。
- **PATCH**，用于在请求-响应链上的每个节点获取传输路径。
- **PUT**，用于向服务器上传资源，通常用于更新已存在的资源或创建新的资源。
- **DELETE**，用于请求服务器删除指定的资源。

你可以通过配置 HTTP 请求的包括 URL、请求头、查询参数、请求体内容以及认证信息等。 

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FgYWdjhLQihIN0cBg1EkP%252Fimage.png%3Falt%3Dmedia%26token%3D4a00a4d5-f4ee-4352-976f-789fe26edcce&width=768&dpr=4&quality=100&sign=658b72f6&sv=1)

HTTP 请求配置

### 2 场景

这个节点的一个实用特性是能够根据场景需要，在请求的不同部分动态插入变量。比如在处理客户评价请求时，你可以将用户名或客户ID、评价内容等变量嵌入到请求中，以定制化自动回复信息或获取特定客户信息并发送相关资源至特定的服务器。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FxkXV7FkvCK6EfLv6NU35%252Fimage.png%3Falt%3Dmedia%26token%3Dc3daad6c-3de8-4842-aaa7-be73d79d4d60&width=768&dpr=4&quality=100&sign=554bed3a&sv=1)

客户评价分类

HTTP 请求的返回值包括响应体、状态码、响应头和文件。值得注意的是，如果响应中包含了文件（目前仅支持图片类型），这个节点能够自动将文件保存下来，供流程后续步骤使用。这样的设计不仅提高了处理效率，也使得处理带有文件的响应变得简单直接。

# 工具

“工具”节点可以为工作流提供强大的第三方能力支持，分为以下三种类型：

- **内置工具**，Dify 第一方提供的工具，使用该工具前可能需要先给工具进行 **授权**。
- **自定义工具**，通过 [OpenAPI/Swagger 标准格式](https://swagger.io/specification/)导入或配置的工具。如果内置工具无法满足使用需求，你可以在 **Dify 菜单导航 --工具** 内创建自定义工具。
- **工作流**，你可以编排一个更复杂的工作流，并将其发布为工具。详细说明请参考[工具配置说明](https://docs.dify.ai/v/zh-hans/guides/tools)。

### 添加工具节点

添加节点时，选择右侧的 “工具” tab 页。配置工具节点一般分为两个步骤：

1. 对工具授权/创建自定义工具/将工作流发布为工具
2. 配置工具输入和参数

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FOYXq3l1wDq1r8zCO7A6F%252Fimage.png%3Falt%3Dmedia%26token%3Dd3437513-1da2-447b-ba5e-07e12a7a76a0&width=768&dpr=4&quality=100&sign=75c8c741&sv=1)

工具选择

工具节点可以连接其它节点，通过[变量](https://docs.dify.ai/v/zh-hans/guides/workflow/variables)处理和传递数据。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252F5FB6LcrIwJY2rCTK3ja3%252Fimage.png%3Falt%3Dmedia%26token%3D2faaf899-ee56-413d-a06d-7cd4a2d1e305&width=768&dpr=4&quality=100&sign=4c260508&sv=1)

配置 Google 搜索工具检索外部知识

### 将工作流应用发布为工具

工作流应用可以被发布为工具，并被其它工作流内的节点所应用。关于如何创建自定义工具和配置工具，请参考[工具配置说明](https://docs.dify.ai/v/zh-hans/guides/tools)。
