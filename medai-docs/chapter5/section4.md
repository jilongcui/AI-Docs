# 5.4 工作流应用

### 基本介绍

工作流通过将复杂的任务分解成较小的步骤（节点）降低系统复杂度，减少了对提示词技术和模型推理能力的依赖，提高了 LLM 应用面向复杂任务的性能，提升了系统的可解释性、稳定性和容错性。

Dify 工作流分为两种类型：

- **Chatflow**：面向对话类情景，包括客户服务、语义搜索、以及其他需要在构建响应时进行多步逻辑的对话式应用程序。
- **Workflow**：面向自动化和批处理情景，适合高质量翻译、数据分析、内容生成、电子邮件自动化等应用程序。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FvdBlcbguy4O2jYQQdYH1%252Fimage.png%3Falt%3Dmedia%26token%3D4450ffa8-5c2c-476b-a3b9-7e424d2c0e35&width=768&dpr=4&quality=100&sign=679e53a0&sv=1)

为解决自然语言输入中用户意图识别的复杂性，Chatflow 提供了问题理解类节点。相对于 Workflow 增加了 Chatbot 特性的支持，如：对话历史（Memory）、标注回复、Answer 节点等。

为解决自动化和批处理情景中复杂业务逻辑，工作流提供了丰富的逻辑节点，如代码节点、IF/ELSE 节点、模板转换、迭代节点等，除此之外也将提供定时和事件触发的能力，方便构建自动化流程。

### 常见案例

- 客户服务

通过将 LLM 集成到您的客户服务系统中，您可以自动化回答常见问题，减轻支持团队的工作负担。 LLM 可以理解客户查询的上下文和意图，并实时生成有帮助且准确的回答。

- 内容生成

无论您需要创建博客文章、产品描述还是营销材料，LLM 都可以通过生成高质量内容来帮助您。只需提供一个大纲或主题，LLM将利用其广泛的知识库来制作引人入胜、信息丰富且结构良好的内容。

- 任务自动化

可以与各种任务管理系统集成，如 Trello、Slack、Lark、以自动化项目和任务管理。通过使用自然语言处理，LLM 可以理解和解释用户输入，创建任务，更新状态和分配优先级，无需手动干预。

- 数据分析和报告

可以用于分析大型数据集并生成报告或摘要。通过提供相关信息给 LLM，它可以识别趋势、模式和洞察力，将原始数据转化为可操作的智能。对于希望做出数据驱动决策的企业来说，这尤其有价值。

- 邮件自动化处理

LLM 可以用于起草电子邮件、社交媒体更新和其他形式的沟通。通过提供简要的大纲或关键要点，LLM 可以生成一个结构良好、连贯且与上下文相关的信息。这样可以节省大量时间，并确保您的回复清晰和专业。

### 如何开始

- 从一个空白的工作流开始构建或者使用系统模版帮助你开始；
- 熟悉基础操作，包括在画布上创建节点、连接和配置节点、调试工作流、查看运行历史等；
- 保存并发布一个工作流；
- 在已发布应用中运行或者通过 API 调用工作流；

# 关键概念

### 节点

**节点是工作流的关键构成**，通过连接不同功能的节点，执行工作流的一系列操作。

------

### 变量

**变量用于串联工作流内前后节点的输入与输出**，实现流程中的复杂处理逻辑，包含环境变量、系统变量和会话变量。

------

### Chatflow 和 Workflow

**应用场景**

- **Chatflow**：面向对话类情景，包括客户服务、语义搜索、以及其他需要在构建响应时进行多步逻辑的对话式应用程序。
- **Workflow**：面向自动化和批处理情景，适合高质量翻译、数据分析、内容生成、电子邮件自动化等应用程序。

**使用入口**

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FYGACKz6rbGIvWSOWNSfQ%252Foutput.png%3Falt%3Dmedia%26token%3Df5520e78-04f6-4120-b656-7b91003a7a52&width=768&dpr=4&quality=100&sign=badbcc2&sv=1)

Chatflow 入口

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FI9BmggTqTrxgRHenkimu%252Foutput.png%3Falt%3Dmedia%26token%3D6331d8ce-6474-4758-b4b9-fa20a9c47448&width=768&dpr=4&quality=100&sign=c9388ebb&sv=1)

Workflow 入口

**可用节点差异**

1. End 节点属于 Workflow 的结束节点，仅可在流程结束时选择。
2. Answer 节点属于 Chatflow ，用于流式输出文本内容，并支持在流程中间步骤输出。
3. Chatflow 内置聊天记忆（Memory），用于存储和传递多轮对话的历史消息，可在 LLM 、问题分类等节点内开启，Workflow 无 Memory 相关配置，无法开启。
4. Chatflow 的开始节点内置变量包括：`sys.query`，`sys.files`，`sys.conversation_id`，`sys.user_id`。Workflow 的开始节点内置变量包括：`sys.files`，`sys.user_id`

# 变量

Workflow 和 Chatflow 类型应用由独立节点相构成。大部分节点设有输入和输出项，但每个节点的输入信息不一致，各个节点所输出的答复也不尽相同。

如何用一种固定的符号**指代动态变化的内容？** 变量作为一种动态数据容器，能够存储和传递不固定的内容，在不同的节点内被相互引用，实现信息在节点间的灵活通信。

### **系统变量**

系统变量指的是在 Chatflow / Workflow 应用内预设的系统级参数，可以被其它节点全局读取。系统级变量均以 `sys` 开头。

#### Workflow

Workflow 类型应用提供以下系统变量：



| 变量名称      | 数据类型    | 说明                                                         | 备注                                             |
| :------------ | :---------- | :----------------------------------------------------------- | :----------------------------------------------- |
| `sys.files`   | Array[File] | 文件参数，存储用户初始使用应用时上传的图片                   | 图片上传功能需在应用编排页右上角的 “功能” 处开启 |
| `sys.user_id` | String      | 用户 ID，每个用户在使用工作流应用时，系统会自动向用户分配唯一标识符，用以区分不同的对话用户 |                                                  |

![workflow-system-variable](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FDkdGG7Es4A1pFcGwu31y%252Fimage.png%3Falt%3Dmedia%26token%3D1ffebfa1-b022-4a47-90a8-b14f6a79aa47&width=768&dpr=4&quality=100&sign=4887076d&sv=1)

Workflow 类型应用系统变量

#### Chatflow

Chatflow 类型应用提供以下系统变量：



| 变量名称              | 数据类型    | 说明                                                         | 备注                                             |
| :-------------------- | :---------- | :----------------------------------------------------------- | :----------------------------------------------- |
| `sys.query`           | String      | 用户在对话框中初始输入的内容                                 |                                                  |
| `sys.files`           | Array[File] | 用户在对话框内上传的图片                                     | 图片上传功能需在应用编排页右上角的 “功能” 处开启 |
| `sys.dialogue_count`  | Number      | 用户在与 Chatflow 类型应用交互时的对话轮数。每轮对话后自动计数增加 1，可以和 if-else 节点搭配出丰富的分支逻辑。例如到第 X 轮对话时，回顾历史对话并给出分析 |                                                  |
| `sys.conversation_id` | String      | 对话框交互会话的唯一标识符，将所有相关的消息分组到同一个对话中，确保 LLM 针对同一个主题和上下文持续对话 |                                                  |
| `sys.user_id`         | String      | 分配给每个应用用户的唯一标识符，用以区分不同的对话用户       |                                                  |

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FnEabPflR315QnAEeLhLN%252Fimage.png%3Falt%3Dmedia%26token%3D1f475f55-7d2f-4a79-afa0-ca4b1b803e49&width=768&dpr=4&quality=100&sign=c46697af&sv=1)

Chatflow 类型应用系统变量

### 环境变量

**环境变量用于保护工作流内所涉及的敏感信息**，例如运行工作流时所涉及的 API 密钥、数据库密码等。它们被存储在工作流程中，而不是代码中，以便在不同环境中共享。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252F5NyEyTNH8sKcgyRMhbYv%252F%25E7%258E%25AF%25E5%25A2%2583%25E5%258F%2598%25E9%2587%258F.jpeg%3Falt%3Dmedia%26token%3D0b4a9003-1dbc-41a3-ac13-975611c316ed&width=768&dpr=4&quality=100&sign=cb18a8df&sv=1)

环境变量

支持以下三种数据类型：

- String 字符串
- Number 数字
- Secret 密钥

环境变量拥有以下特性：

- 环境变量可在大部分节点内全局引用；
- 环境变量命名不可重复；
- 环境变量为只读变量，不可写入；

### 会话变量

> 会话变量面向多轮对话场景，而 Workflow 类型应用的交互是线性而独立的，不存在多次对话交互的情况，因此会话变量仅适用于 Chatflow 类型（聊天助手 → 工作流编排）应用。

**会话变量允许应用开发者在同一个 Chatflow 会话内，指定需要被临时存储的特定信息，并确保在当前工作流内的多轮对话内都能够引用该信息**，如上下文、上传至对话框的文件（即将上线）、 用户在对话过程中所输入的偏好信息等。好比为 LLM 提供一个可以被随时查看的“备忘录”，避免因 LLM 记忆出错而导致的信息偏差。

例如你可以将用户在首轮对话时输入的语言偏好存储至会话变量中，LLM 在回答时将参考会话变量中的信息，并在后续的对话中使用指定的语言回复用户。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fr6UT23sf1PyUmpgdbRNX%252F%25E4%25BC%259A%25E8%25AF%259D%25E5%258F%2598%25E9%2587%258F.jpeg%3Falt%3Dmedia%26token%3D01533cea-84f8-4e09-845e-d879d2b87924&width=768&dpr=4&quality=100&sign=7f357c9b&sv=1)

会话变量

**会话变量**支持以下六种数据类型：

- String 字符串
- Number 数值
- Object 对象
- Array[string] 字符串数组
- Array[number] 数值数组
- Array[object] 对象数组

**会话变量**具有以下特性：

- 会话变量可在大部分节点内全局引用；
- 会话变量的写入需要使用[变量赋值](https://docs.dify.ai/v/zh-hans/guides/workflow/node/variable-assignment)节点；
- 会话变量为可读写变量；

关于如何将会话变量与变量赋值节点配合使用，请参考[变量赋值](https://docs.dify.ai/v/zh-hans/guides/workflow/node/variable-assignment)节点说明。

### 注意事项

- 为避免变量名重复，节点命名不可重复
- 节点的输出变量一般为固定变量，不可编辑

