# 5.13 召回测试/引用归属

### 1 召回测试

Dify 知识库内提供了文本召回测试的功能，用于调试不同检索方式及参数配置下的召回效果。你可以在 **源文本** 输入框输入常见的用户问题，点击 **测试** 并在右侧的 **召回段落** 查看召回结果。在 **最近查询** 内可以查看到历史的查询记录；若知识库已关联至应用内，由应用内触发的知识库查询也可以在此查看记录。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FHLHhqhv4IbNGrAIWVQgC%252Fimage.png%3Falt%3Dmedia%26token%3Deb119d48-e564-468b-a66c-a617075abca9&width=768&dpr=4&quality=100&sign=9665f1f&sv=1)

召回测试

点击源文本输入框右上角的图标可以更换当前知识库的检索方式和具体参数，**保存之后仅在召回测试的调试过程中生效**。在召回测试完成调试并确认更改知识库的检索参数时，需要在 [知识库设置 > 检索设置](https://github.com/langgenius/dify-docs/blob/main/zh_CN/guides/knowledge-base/create-knowledge-and-upload-documents/README.md#id-6-jian-suo-she-zhi) 中进行更改。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FvV72wufZeztyiob6O6IA%252Fimage.png%3Falt%3Dmedia%26token%3D77f683f3-8474-4ff1-903e-69f9a42932fd&width=768&dpr=4&quality=100&sign=a5524bb9&sv=1)

召回测试-检索设置

**召回测试建议步骤：**

1. 

   设计和整理覆盖常见用户问题的测试用例/测试问题集；

2. 

   选择合适的检索策略：向量检索/全文检索/混合检索，不同检索方式的优缺点，请参考扩展阅读[检索增强生成（RAG）](https://docs.dify.ai/v/zh-hans/learn-more/extended-reading/retrieval-augment)；

3. 

   调试召回分段数量（TopK）和召回分数阈值（Score），需根据应用场景、包括文档本身的质量来选择合适的参数组合。

**TopK 值和召回阈值（Score ）如何配置**

- 

  **TopK 代表按相似分数倒排时召回分段的最大个数**。TopK 值调小，将会召回更少分段，可能导致召回的相关文本不全；TopK 值调大，将召回更多分段，可能导致召回语义相关性较低的分段使得 LLM 回复质量降低。

- 

  **召回阈值（Score）代表允许召回分段的最低相似分数。**召回分数调小，将会召回更多分段，可能导致召回相关度较低的分段；召回分数阈值调大，将会召回更少分段，过大时将会导致丢失相关分段。

------

### 2 引用与归属

在应用内测试知识库效果时，你可以进入 **工作室 -- 添加功能 -- 引用归属**，打开引用归属功能。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fgit-blob-7b0bcb4b6b6a56edc4d6ec874937a0879257f1c7%252Fcitation-and-attribution.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=94ad940a&sv=1)

打开引用与归属功能

开启功能后，当 LLM 引用知识库内容来回答问题时，可以在回复内容下面查看到具体的引用段落信息，包括**原始分段文本、分段序号、匹配度**等。点击引用分段上方的 **跳转至知识库 ，**可以快捷访问该分段所在的知识库分段列表，方便开发者进行调试编辑。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FNOoFoYBE8uPRlryImi0D%252Fimage.png%3Falt%3Dmedia%26token%3Deffb6115-4cfa-45f9-ad5b-0fd784192541&width=768&dpr=4&quality=100&sign=195d9d20&sv=1)

查看回复内容的引用信息
