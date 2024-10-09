# 5.11 知识库及文档维护



### 1 查看文本分段

知识库内已上传的每个文档都会以文本分段（Chunks）的形式进行存储，你可以在分段列表内查看每一个分段的具体文本内容。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FDR3zzBBnJW24zRNTGijK%252Fimage.png%3Falt%3Dmedia%26token%3D3fd4f26b-2120-4391-abb0-362c8f14b7d7&width=768&dpr=4&quality=100&sign=b0765643&sv=1)

查看已上传的文档分段

------

### 2 检查分段质量

文档分段对于知识库应用的问答效果有明显影响，在将知识库与应用关联之前，建议人工检查分段质量。

通过字符长度、标识符或者 NLP 语义分段等机器自动化的分段方式虽然能够显著减少大规模文本分段的工作量，但分段质量与不同文档格式的文本结构、前后文的语义联系都有关系，通过人工检查和订正可以有效弥补机器分段在语义识别方面的缺点。

检查分段质量时，一般需要关注以下几种情况：

- **过短的文本分段**，导致语义缺失；

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FngD50QqUwnFi7aSUWqxG%252Fimage.png%3Falt%3Dmedia%26token%3D9c72fbb6-4f40-4951-adae-104245d249dd&width=768&dpr=4&quality=100&sign=126594d7&sv=1)

过短的文本分段

- **过长的文本分段**，导致语义噪音影响匹配准确性；

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FLGzuDVIKcOdwBO4gWcHa%252Fimage.png%3Falt%3Dmedia%26token%3D0be00a02-2670-4924-98ae-1f896e5523f4&width=768&dpr=4&quality=100&sign=be1f4ea4&sv=1)

过长的文本分段

- **明显的语义截断**，在使用最大分段长度限制时会出现强制性的语义截断，导致召回时缺失内容；

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252F1fNAKKl3MkLd5vbRkZUG%252Fimage.png%3Falt%3Dmedia%26token%3Da2ff9c05-df49-4725-a78f-b2e0d3f82d5e&width=768&dpr=4&quality=100&sign=f0862ec3&sv=1)

明显的语义截断

------

### 3 添加文本分段

在分段列表内点击 「 添加分段 」 ，可以在文档内自行添加一个或批量添加多个自定义分段。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FUvTIyUHzo4LYZ1BM17km%252Fimage.png%3Falt%3Dmedia%26token%3D37fba198-d048-4227-98aa-da16cc22fe09&width=768&dpr=4&quality=100&sign=ce0d28de&sv=1)

批量添加分段时，你需要先下载 CSV 格式的分段上传模板，并按照模板格式在 Excel 内编辑所有的分段内容，再将 CSV 文件保存后上传。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252F2NNn9s2Qd7m9qWwNYoim%252Fimage.png%3Falt%3Dmedia%26token%3D3436908e-88cf-430d-95e1-d06070b2fa05&width=768&dpr=4&quality=100&sign=d5ab9fef&sv=1)

批量添加自定义分段

------

### 4 编辑文本分段

在分段列表内，你可以对已添加的分段内容直接进行编辑修改。包括分段的文本内容和关键词。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252F4BKKIm8bXuLfJUZyvy3J%252Fimage.png%3Falt%3Dmedia%26token%3D5b861ce5-7ac5-4a40-9622-07f7a343a3eb&width=768&dpr=4&quality=100&sign=bc27768b&sv=1)

编辑文档分段

------

### 5 元数据管理

除了用于标记不同来源文档的元数据信息，例如网页数据的标题、网址、关键词、描述等。元数据将被用于知识库的分段召回过程中，作为结构化字段参与召回过滤或者显示引用来源。



元数据过滤及引用来源功能当前版本尚未支持。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FwLf1SNvfAQQNNAYOirw0%252Fimage.png%3Falt%3Dmedia%26token%3Db3aa6181-2d80-4111-88a9-64a877737985&width=768&dpr=4&quality=100&sign=1388f9f8&sv=1)

元数据管理

------

### 6 添加文档

在「 知识库 > 文档列表 」 点击 「 添加文件 」，可以在已创建的知识库内上传新的文档或者 [Notion 页面](https://docs.dify.ai/v/zh-hans/guides/knowledge-base/sync-from-notion)。

知识库（Knowledge）是一些文档（Documents）的集合。文档可以由开发者或运营人员上传，或由其它数据源同步（通常对应数据源中的一个文件单位）。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FPqxdtJYXYkeBIqQCH00b%252Fimage.png%3Falt%3Dmedia%26token%3D6a382d79-23c3-4667-931f-b8eda8d9ccd8&width=768&dpr=4&quality=100&sign=1df4ceba&sv=1)

知识库上传新文档

------

### 7 文档禁用和归档

**禁用**：数据集支持将暂时不想被索引的文档或分段进行禁用，在数据集文档列表，点击禁用按钮，则文档被禁用；也可以在文档详情，点击禁用按钮，禁用整个文档或某个分段，禁用的文档将不会被索引。禁用的文档点击启用，可以取消禁用。

**归档**：一些不再使用的旧文档数据，如果不想删除可以将它进行归档，归档后的数据就只能查看或删除，不可以进行编辑。在数据集文档列表，点击归档按钮，则文档被归档，也可以在文档详情，归档文档。归档的文档将不会被索引。归档的文档也可以点击撤销归档。

------

### 8 知识库设置

在知识库的左侧导航中点击**设置**，你可以改变知识库的以下设置项：

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fu8Sousmlogi7eFBznh69%252Fimage.png%3Falt%3Dmedia%26token%3Dda0ed6be-5d93-46bd-95aa-aff046a851bc&width=768&dpr=4&quality=100&sign=b37d5910&sv=1)

知识库设置

**知识库名称**，定义一个名称用于识别一个知识库。

**知识库描述**，用于描述知识库内文档代表的信息



在知识库召回模式为 N 选 1 时，知识库作为工具提供给 LLM 进行推理调用，推理依据是知识库的描述，如果描述为空则会使用 Dify 的自动索引策略

**可见权限**，可选择 「 只有我 」 或 「 所有团队成员 」，不具有权限的人将无法查阅和编辑数据集。

**索引模式**，[参考文档](https://docs.dify.ai/v/zh-hans/guides/knowledge-base/create-knowledge-and-upload-documents#id-5-suo-yin-fang-shi)

**Embedding 模型，**修改知识库的嵌入模型，修改 Embedding 模型将对知识库内的所有文档重新嵌入，原先的嵌入将会被删除。

**检索设置**，[参考文档](https://docs.dify.ai/v/zh-hans/guides/knowledge-base/create-knowledge-and-upload-documents#id-6-jian-suo-she-zhi)

------

### 9 知识库 API 管理

Dify 知识库提供整套标准 API ，开发者通过 API 调用对知识库内的文档、分段进行增删改查等日常管理维护操作，请参考[知识库 API 文档](https://docs.dify.ai/v/zh-hans/guides/knowledge-base/maintain-dataset-via-api)。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FqqERjUP6VxL7DjsBbpks%252Fimage.png%3Falt%3Dmedia%26token%3D2ae1af89-6620-4cc6-8bb6-20bf47e879de&width=768&dpr=4&quality=100&sign=ee9f2be1&sv=1)

知识库 API 管理
