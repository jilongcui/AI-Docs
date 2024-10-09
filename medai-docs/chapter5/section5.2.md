# 5.5.2 工作流的节点2

## 代码执行

### 介绍

代码节点支持运行 Python / NodeJS 代码以在工作流程中执行数据转换。它可以简化您的工作流程，适用于Arithmetic、JSON transform、文本处理等情景。

该节点极大地增强了开发人员的灵活性，使他们能够在工作流程中嵌入自定义的 Python 或 Javascript 脚本，并以预设节点无法达到的方式操作变量。通过配置选项，你可以指明所需的输入和输出变量，并撰写相应的执行代码：

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252F4y0F1NmpRrBEbKRCoB0D%252Fimage.png%3Falt%3Dmedia%26token%3D3207abc5-c147-477d-ba66-0f35edf62bb2&width=768&dpr=4&quality=100&sign=bc5158b0&sv=1)

### 配置

如果您需要在代码节点中使用其他节点的变量，您需要在`输入变量`中定义变量名，并引用这些变量，可以参考[变量引用](https://docs.dify.ai/v/zh-hans/guides/workflow/key-concept#变量)。

### 使用场景

使用代码节点，您可以完成以下常见的操作：

#### 结构化数据处理

在工作流中，经常要面对非结构化的数据处理，如JSON字符串的解析、提取、转换等。最典型的例子就是HTTP节点的数据处理，在常见的API返回结构中，数据可能会被嵌套在多层JSON对象中，而我们需要提取其中的某些字段。代码节点可以帮助您完成这些操作，下面是一个简单的例子，它从HTTP节点返回的JSON字符串中提取了`data.name`字段：

Copy

```
def main(http_response: str) -> str:
    import json
    data = json.loads(http_response)
    return {
        # 注意在输出变量中声明result
        'result': data['data']['name'] 
    }
```

#### 数学计算

当工作流中需要进行一些复杂的数学计算时，也可以使用代码节点。例如，计算一个复杂的数学公式，或者对数据进行一些统计分析。下面是一个简单的例子，它计算了一个数组的平方差：

Copy

```
def main(x: list) -> float:
    return {
        # 注意在输出变量中声明result
        'result' : sum([(i - sum(x) / len(x)) ** 2 for i in x]) / len(x)
    }
```

#### 拼接数据

有时，也许您需要拼接多个数据源，如多个知识检索、数据搜索、API调用等，代码节点可以帮助您将这些数据源整合在一起。下面是一个简单的例子，它将两个知识库的数据合并在一起：

Copy

```
def main(knowledge1: list, knowledge2: list) -> list:
    return {
        # 注意在输出变量中声明result
        'result': knowledge1 + knowledge2
    }
```

### 本地部署

如果您是本地部署的用户，您需要启动一个沙盒服务，它会确保恶意代码不会被执行，同时，启动该服务需要使用Docker服务，您可以在[这里](https://github.com/langgenius/dify/tree/main/docker/docker-compose.middleware.yaml)找到Sandbox服务的具体信息，您也可以直接通过`docker-compose`启动服务：

Copy

```
docker-compose -f docker-compose.middleware.yaml up -d
```

> 如果您的系统安装了 Docker Compose V2 而不是 V1，请使用 `docker compose` 而不是 `docker-compose`。通过`$ docker compose version`检查这是否为情况。在[这里](https://docs.docker.com/compose/#compose-v2-and-the-new-docker-compose-command)阅读更多信息。

### 限制

无论是Python还是Javascript，它们的执行环境都被严格隔离（沙箱化），以确保安全性。这意味着开发者不能使用那些消耗大量系统资源或可能引发安全问题的功能，如直接访问文件系统、进行网络请求或执行操作系统级别的命令。这些限制保证了代码的安全执行，同时避免了对系统资源的过度消耗。

## 模板转换

### 定义

允许借助 Jinja2 的 Python 模板语言灵活地进行数据转换、文本处理等。

### 什么是 Jinja？

> Jinja is a fast, expressive, extensible templating engine.
>
> Jinja 是一个快速、表达力强、可扩展的模板引擎。

—— https://jinja.palletsprojects.com/en/3.1.x/

### 场景

模板节点允许你借助 Jinja2 这一强大的 Python 模板语言，在工作流内实现轻量、灵活的数据转换，适用于文本处理、JSON 转换等情景。例如灵活地格式化并合并来自前面步骤的变量，创建出单一的文本输出。这非常适合于将多个数据源的信息汇总成一个特定格式，满足后续步骤的需求。

**示例1：**将多个输入（文章标题、介绍、内容）拼接为完整文本

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FYy13dVflaqVEZzsfXVeX%252Fimage.png%3Falt%3Dmedia%26token%3Deac39909-565e-4a02-9a86-93a003eb2e4d&width=768&dpr=4&quality=100&sign=264d1131&sv=1)

拼接文本

**示例2：**将知识检索节点获取的信息及其相关的元数据，整理成一个结构化的 Markdown 格式

Copy

```
{% for item in chunks %}
### Chunk {{ loop.index }}. 
### Similarity: {{ item.metadata.score | default('N/A') }}

#### {{ item.title }}

##### Content
{{ item.content | replace('\n', '\n\n') }}

---
{% endfor %}
```

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FOtGkLaz38v0FSzSBNuV2%252Fimage.png%3Falt%3Dmedia%26token%3D122965f8-9d70-4e57-b0e2-1fdaf1320275&width=768&dpr=4&quality=100&sign=f289740a&sv=1)

知识检索节点输出转换为 Markdown

你可以参考 Jinja 的[官方文档](https://jinja.palletsprojects.com/en/3.1.x/templates/)，创建更为复杂的模板来执行各种任务。

## 变量聚合

### 1 定义

将多路分支的变量聚合为一个变量，以实现下游节点统一配置。

变量聚合节点（原变量赋值节点）是工作流程中的一个关键节点，它负责整合不同分支的输出结果，确保无论哪个分支被执行，其结果都能通过一个统一的变量来引用和访问。这在多分支的情况下非常有用，可将不同分支下相同作用的变量映射为一个输出变量，避免下游节点重复定义。


### 2 场景

通过变量聚合，可以将诸如问题分类或条件分支等多路输出聚合为单路，供流程下游的节点使用和操作，简化了数据流的管理。

**问题分类后的多路聚合**

未添加变量聚合，分类1 和 分类 2 分支经不同的知识库检索后需要重复定义下游的 LLM 和直接回复节点。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fp4i1zi4oWvbefh4WA2xC%252Fimage.png%3Falt%3Dmedia%26token%3D1fca7569-f185-4aa3-a369-410a4e03dc48&width=768&dpr=4&quality=100&sign=f6464c03&sv=1)

问题分类（无变量聚合）

添加变量聚合，可以将两个知识检索节点的输出聚合为一个变量。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FebTugkG7RyZ5WCDBBI0B%252Fimage.png%3Falt%3Dmedia%26token%3Dc38902d5-bddc-4711-8c6e-28bddd7ec328&width=768&dpr=4&quality=100&sign=1cd0e210&sv=1)

问题分类后的多路聚合

**IF/ELSE 条件分支后的多路聚合**

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FvTYB8AQbmX00iplr6Fx7%252Fimage.png%3Falt%3Dmedia%26token%3D4eba382e-772e-4af8-b448-cdcf14c034f6&width=768&dpr=4&quality=100&sign=7c15d770&sv=1)

问题分类后的多路聚合

### 3 格式要求

变量聚合器支持聚合多种数据类型，包括字符串（`String`）、数字（`Number`）、对象（`Object`）以及数组（`Array`）。

**变量聚合器只能聚合同一种数据类型的变量**。若第一个添加至变量聚合节点内的变量数据格式为 `String`，后续连线时会自动过滤可添加变量为 `String` 类型。

**聚合分组**

v0.6.10 版本之后已支持聚合分组。

开启聚合分组后，变量聚合器可以聚合多组变量，各组内聚合时要求同一种数据类型。

## 变量赋值

定义

变量赋值节点用于向可写入变量进行变量赋值，已支持以下可写入变量：

- [会话变量](https://docs.dify.ai/v/zh-hans/guides/workflow/key-concept#hui-hua-bian-liang)。

用法：通过变量赋值节点，你可以将工作流内的变量赋值到会话变量中用于临时存储，并可以在后续对话中持续引用。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fgit-blob-c615ff1be228baa4408a762fb2cbad84e340ecb6%252Fzh-conversation-variable.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=b695f703&sv=1)

### 使用场景示例

通过变量赋值节点，你可以将会话过程中的**上下文、上传至对话框的文件（即将上线）、用户所输入的偏好信息**等写入至会话变量，并在后续对话中引用已存储的信息导向不同的处理流程或者进行回复。

**场景 1**

**自动判断提取并存储对话中的信息**，在会话内通过会话变量数组记录用户输入的重要信息，并在后续对话中让 LLM 基于会话变量中存储的历史信息进行个性化回复。

示例：开始对话后，LLM 会自动判断用户输入是否包含需要记住的事实、偏好或历史记录。如果有，LLM 会先提取并存储这些信息，然后再用这些信息作为上下文来回答。如果没有新的信息需要保存，LLM 会直接使用自身的相关记忆知识来回答问题。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FNzWrDCtJx8VaQ7p0Nk0F%252F%25E4%25B8%25AD%25E6%2596%2587.jpeg%3Falt%3Dmedia%26token%3Dcf5e1b19-5a08-4fde-abc8-42f12e2faf7b&width=768&dpr=4&quality=100&sign=457ff3e9&sv=1)

**配置流程：**

1. **设置会话变量**：首先设置一个会话变量数组 `memories`，类型为 array[object]，用于存储用户的事实、偏好和历史记录。

2. **判断和提取记忆**：

   - 添加一个条件判断节点，使用 LLM 来判断用户输入是否包含需要记住的新信息。

   - 如果有新信息，走上分支，使用 LLM 节点提取这些信息。

   - 如果没有新信息，走下分支，直接使用现有记忆回答。

3. **变量赋值/写入**：

   - 在上分支中，使用变量赋值节点，将提取出的新信息追加（append）到 `memories` 数组中。

   - 使用转义功能将 LLM 输出的文本字符串转换为适合存储在 array[object] 中的格式。

4. **变量读取和使用**：

   - 在后续的 LLM 节点中，将 `memories` 数组中的内容转换为字符串，并插入到 LLM 的提示词 Prompts 中作为上下文。

   - LLM 使用这些历史信息来生成个性化回复。

图中的 code 节点代码如下：

1. 将字符串转义为 object

Copy

```
import json

def main(arg1: str) -> object:
    try:
        # Parse the input JSON string
        input_data = json.loads(arg1)
        
        # Extract the memory object
        memory = input_data.get("memory", {})
        
        # Construct the return object
        result = {
            "facts": memory.get("facts", []),
            "preferences": memory.get("preferences", []),
            "memories": memory.get("memories", [])
        }
        
        return {
            "mem": result
        }
    except json.JSONDecodeError:
        return {
            "result": "Error: Invalid JSON string"
        }
    except Exception as e:
        return {
            "result": f"Error: {str(e)}"
        }
```

1. 将 object 转义为字符串

Copy

```
import json

def main(arg1: list) -> str:
    try:
        # Assume arg1[0] is the dictionary we need to process
        context = arg1[0] if arg1 else {}
        
        # Construct the memory object
        memory = {"memory": context}
        
        # Convert the object to a JSON string
        json_str = json.dumps(memory, ensure_ascii=False, indent=2)
        
        # Wrap the JSON string in <answer> tags
        result = f"<answer>{json_str}</answer>"
        
        return {
            "result": result
        }
    except Exception as e:
        return {
            "result": f"<answer>Error: {str(e)}</answer>"
        }
```

**场景 2**

**记录用户的初始偏好信息**，在会话内记住用户输入的语言偏好，在后续对话中持续使用该语言类型进行回复。

示例：用户在对话开始前，在 `language` 输入框内指定了 “中文”，该语言将会被写入会话变量，LLM 在后续进行答复时会参考会话变量中的信息，在后续对话中持续使用“中文”进行回复。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fgit-blob-b6492a90d9b0e99e18ae228e0fb05b13d747e36d%252Fzh-conversation-var-scenario-1.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=68f4afab&sv=1)

**配置流程：**

**设置会话变量**：首先设置一个会话变量 `language`，在会话流程开始时添加一个条件判断节点，用来判断 `language` 变量的值是否为空。

**变量写入/赋值**：首轮对话开始时，若 `language` 变量值为空，则使用 LLM 节点来提取用户输入的语言，再通过变量赋值节点将该语言类型写入到会话变量 `language` 中。

**变量读取**：在后续对话轮次中 `language` 变量已存储用户语言偏好。在后续对话中，LLM 节点通过引用 language 变量，使用用户的偏好语言类型进行回复。

**场景 3**

**辅助 Checklist 检查**，在会话内通过会话变量记录用户的输入项，更新 Checklist 中的内容，并在后续对话中检查遗漏项。

示例：开始对话后，LLM 会要求用户在对话框内输入 Checklist 所涉及的事项，用户一旦提及了 Checklist 中的内容，将会更新并存储至会话变量内。LLM 会在每轮对话后提醒用户继续补充遗漏项。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fgit-blob-11a59fb7fd916ae28778be0193a4898ffa000d2f%252Fconversation-var-scenario-2-1.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=a5ae57eb&sv=1)

**配置流程：**

- **设置会话变量：** 首先设置一个会话变量 `ai_checklist`，在 LLM 内引用该变量作为上下文进行检查。
- **变量赋值/写入：** 每一轮对话时，在 LLM 节点内检查 `ai_checklist` 内的值并比对用户输入，若用户提供了新的信息，则更新 Checklist 并将输出内容通过变量赋值节点写入到 `ai_checklist` 内。
- **变量读取：** 每一轮对话读取 `ai_cheklist` 内的值并比对用户输入直至所有 checklist 完成。

### 使用变量赋值节点

点击节点右侧 ＋ 号，选择“变量赋值”节点，填写“赋值的变量”和“设置变量”。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fgit-blob-442888c252e9dbdf358ff5568a113d0ce6ff9eac%252Fzh-language-variable-assigner.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=6af12fcc&sv=1)

**设置变量：**

赋值的变量：选择被赋值变量，即指定需要被赋值的目标会话变量。

设置变量：选择需要赋值的变量，即指定需要被转换的源变量。

以上图赋值逻辑为例：将上一个节点的文本输出项 `Language Recognition/text` 赋值到会话变量 `language` 内。

**写入模式：**

- 覆盖，将源变量的内容覆盖至目标会话变量
- 追加，指定变量为 Array 类型时
- 清空，清空目标会话变量中的内容
