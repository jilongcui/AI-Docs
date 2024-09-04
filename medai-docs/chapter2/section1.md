# 2.1 模型

在之前介绍 pipeline 模型时，我们使用 `AutoModel` 类根据 checkpoint 名称自动加载模型。当然，我们也可以直接使用对应的 `Model` 类。例如加载 BERT 模型（包括采用 BERT 结构的其他[模型](https://huggingface.co/models?filter=bert)）：

```
from transformers import BertModel

model = BertModel.from_pretrained("bert-base-cased")
```

这里可以直接将 `BertModel` 替换成 `AutoModel`。

在大部分情况下，我们都应该使用 `AutoModel`，编写的代码应该与 checkpoint 无关。

这样如果我们想要换一个预训练模型（例如把 BERT 换成 RoBERTa），只需要切换 checkpoint，其他代码可以保持不变。

### 加载模型

通过调用 `Model.from_pretrained()` 函数可以自动加载 checkpoint 对应的模型权重 (weights)。然后，我们可以直接使用模型完成它的预训练任务，或者在新的任务上对模型权重进行微调。

> `Model.from_pretrained()` 会自动缓存下载的模型权重，默认保存到 *~/.cache/huggingface/transformers*，我们也可以通过 HF_HOME 环境变量自定义缓存目录。

所有存储在 [Model Hub](https://huggingface.co/models) 上的模型都能够通过 `Model.from_pretrained()` 加载，只需要传递对应 checkpoint 的名称。当然了，我们也可以先将模型下载下来，然后将本地路径传给 `Model.from_pretrained()`，比如加载下载好的 [Bert-base 模型](https://huggingface.co/bert-base-cased)：

```
from transformers import BertModel

model = BertModel.from_pretrained("./models/bert/")
```

部分模型的 Hub 页面中会包含很多文件，我们通常只需要下载模型对应的 *config.json* 和 *pytorch_model.bin*，以及分词器对应的 *tokenizer.json*、*tokenizer_config.json* 和 *vocab.txt*。我们在后面会详细介绍这些文件。

### 保存模型

保存模型与加载模型类似，只需要调用 `Model.save_pretrained()` 函数。例如保存加载的 BERT 模型：

```
from transformers import AutoModel

model = AutoModel.from_pretrained("bert-base-cased")
model.save_pretrained("./models/bert-base-cased/")
```

这会在保存路径下创建两个文件：

- config.json：模型配置文件，里面包含构建模型结构的必要参数；
- pytorch_model.bin：又称为 state dictionary，包含模型的所有权重。

这两个文件缺一不可，配置文件负责记录模型的**结构**，模型权重记录模型的**参数**。我们自己保存的模型同样可以通过 `Model.from_pretrained()` 加载，只需要传递保存目录的路径。
