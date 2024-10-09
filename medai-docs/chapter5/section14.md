# 5.14 从网页导入知识库

Dify 知识库通过集成 Firecrawl ，支持网页抓取并解析为 Markdown 导入至知识库。



[Firecrawl ](https://www.firecrawl.dev/)是一个开源的网页解析工具，它能将网页将其转换为干净并且方便 LLM 识别的 Markdown 格式文本，它同时提供了易于使用的 API 服务。

### 如何配置

首先需要在 DataSource 页面内配置 Firecrawl 的凭据。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fojuk9LZjXLlRFteSqnnu%252Fimage.png%3Falt%3Dmedia%26token%3Dd01968bb-f671-49ac-8e69-2487f80404c0&width=768&dpr=4&quality=100&sign=dfa934a&sv=1)

登录 [Firecrawl 官网](https://www.firecrawl.dev/) 完成注册，获取 API Key 后填入并保存。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FhgPoniOW92PifCLhkc19%252Fimage.png%3Falt%3Dmedia%26token%3Dfa515aeb-1ee9-4305-aa20-492ea2b9716c&width=768&dpr=4&quality=100&sign=158bd228&sv=1)

在知识库创建页选择 **Sync from website**，**填入需要抓取的网页 URL**。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252FVki76RZulOg7TGZfHaP9%252Fimage.png%3Falt%3Dmedia%26token%3D2070074a-cb22-4ab0-8a27-27d507202fa0&width=768&dpr=4&quality=100&sign=10d5e730&sv=1)

网页抓取配置

设置中的配置项包括：是否抓取子页面、抓取页面数量上限、页面抓取深度、排除页面、仅抓取页面、提取内容。完成配置后点击 **Run**，预览已解析的页面。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252F9muc0l4bmZ5ioGlewzGj%252Fimage.png%3Falt%3Dmedia%26token%3D99965c35-95ab-4234-85fe-5a8d005f2d6b&width=768&dpr=4&quality=100&sign=48144e0f&sv=1)

执行抓取

导入网页解析的文本后存储至知识库的文档中，查看导入结果。点击 **Add URL** 可以继续导入新的网页。

![img](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fiz65aYoK1JL35oggl0Uk%252Fimage.png%3Falt%3Dmedia%26token%3D68d7004b-788a-4028-8fb2-b4c4a2c7ba17&width=768&dpr=4&quality=100&sign=606f6af7&sv=1)

导入网页解析文本至知识库内
