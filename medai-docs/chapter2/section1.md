# 2.1 快速上手

快来使用 🤗 Transformers 吧！无论你是开发人员还是日常用户，这篇快速上手教程都将帮助你入门并且向你展示如何使用 [pipeline()](https://huggingface.co/docs/transformers/v4.45.2/zh/main_classes/pipelines#transformers.pipeline) 进行推理，使用 [AutoClass](https://huggingface.co/docs/transformers/v4.45.2/zh/model_doc/auto) 加载一个预训练模型和预处理器，以及使用 PyTorch 或 TensorFlow 快速训练一个模型。如果你是一个初学者，我们建议你接下来查看我们的教程或者[课程](https://huggingface.co/course/chapter1/1)，来更深入地了解在这里介绍到的概念。

在开始之前，确保你已经安装了所有必要的库：

```
!pip install transformers datasets evaluate accelerate
```

你还需要安装喜欢的机器学习框架：

Pytorch

Hide Pytorch content

```
pip install torch
```

TensorFlow

Hide TensorFlow content

```
pip install tensorflow
```

### Pipeline

使用 [pipeline()](https://huggingface.co/docs/transformers/v4.45.2/zh/main_classes/pipelines#transformers.pipeline) 是利用预训练模型进行推理的最简单的方式。你能够将 [pipeline()](https://huggingface.co/docs/transformers/v4.45.2/zh/main_classes/pipelines#transformers.pipeline) 开箱即用地用于跨不同模态的多种任务。

创建一个 [pipeline()](https://huggingface.co/docs/transformers/v4.45.2/zh/main_classes/pipelines#transformers.pipeline) 实例并且指定你想要将它用于的任务，就可以开始了。你可以将 [pipeline()](https://huggingface.co/docs/transformers/v4.45.2/zh/main_classes/pipelines#transformers.pipeline) 用于任何一个上面提到的任务，如果想知道支持的任务的完整列表，可以查阅 [pipeline API 参考](https://huggingface.co/docs/transformers/v4.45.2/zh/main_classes/pipelines)。不过, 在这篇教程中，你将把 [pipeline()](https://huggingface.co/docs/transformers/v4.45.2/zh/main_classes/pipelines#transformers.pipeline) 用在一个情感分析示例上：

```
>>> from transformers import pipeline

>>> classifier = pipeline("sentiment-analysis")
```

[pipeline()](https://huggingface.co/docs/transformers/v4.45.2/zh/main_classes/pipelines#transformers.pipeline) 会下载并缓存一个用于情感分析的默认的[预训练模型](https://huggingface.co/distilbert/distilbert-base-uncased-finetuned-sst-2-english)和分词器。现在你可以在目标文本上使用 `classifier` 了：

```
>>> classifier("We are very happy to show you the 🤗 Transformers library.")
[{'label': 'POSITIVE', 'score': 0.9998}]
```

如果你有不止一个输入，可以把所有输入放入一个列表然后传给[pipeline()](https://huggingface.co/docs/transformers/v4.45.2/zh/main_classes/pipelines#transformers.pipeline)，它将会返回一个字典列表：

```
>>> results = classifier(["We are very happy to show you the 🤗 Transformers library.", "We hope you don't hate it."])
>>> for result in results:
...     print(f"label: {result['label']}, with score: {round(result['score'], 4)}")
label: POSITIVE, with score: 0.9998
label: NEGATIVE, with score: 0.5309
```

### AutoTokenizer

分词器负责预处理文本，将文本转换为用于输入模型的数字数组。有多个用来管理分词过程的规则，包括如何拆分单词和在什么样的级别上拆分单词（在 [分词器总结](https://huggingface.co/docs/transformers/v4.45.2/zh/tokenizer_summary) 学习更多关于分词的信息）。要记住最重要的是你需要实例化的分词器要与模型的名称相同, 来确保和模型训练时使用相同的分词规则。

使用 `AutoTokenizer` 加载一个分词器:

```
>>> from transformers import AutoTokenizer

>>> model_name = "nlptown/bert-base-multilingual-uncased-sentiment"
>>> tokenizer = AutoTokenizer.from_pretrained(model_name)
```

将文本传入分词器：

```
>>> encoding = tokenizer("We are very happy to show you the 🤗 Transformers library.")
>>> print(encoding)
{'input_ids': [101, 11312, 10320, 12495, 19308, 10114, 11391, 10855, 10103, 100, 58263, 13299, 119, 102],
 'token_type_ids': [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
 'attention_mask': [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]}
```

分词器返回了含有如下内容的字典:

- [input_ids](https://huggingface.co/docs/transformers/v4.45.2/zh/glossary#input-ids)：用数字表示的 token。
- [attention_mask](https://huggingface.co/docs/transformers/v4.45.2/zh/.glossary#attention-mask)：应该关注哪些 token 的指示。

分词器也可以接受列表作为输入，并填充和截断文本，返回具有统一长度的批次：

```
>>> pt_batch = tokenizer(
...     ["We are very happy to show you the 🤗 Transformers library.", "We hope you don't hate it."],
...     padding=True,
...     truncation=True,
...     max_length=512,
...     return_tensors="pt",
... )
```

查阅[预处理](https://huggingface.co/docs/transformers/v4.45.2/zh/preprocessing)教程来获得有关分词的更详细的信息，以及如何使用 `AutoFeatureExtractor` 和 `AutoProcessor` 来处理图像，音频，还有多模式输入。

### AutoModel

🤗 Transformers 提供了一种简单统一的方式来加载预训练的实例. 这表示你可以像加载 `AutoTokenizer` 一样加载 `AutoModel`。唯一不同的地方是为你的任务选择正确的`AutoModel`。对于文本（或序列）分类，你应该加载`AutoModelForSequenceClassification`：

```
>>> from transformers import AutoModelForSequenceClassification

>>> model_name = "nlptown/bert-base-multilingual-uncased-sentiment"
>>> pt_model = AutoModelForSequenceClassification.from_pretrained(model_name)
```

通过 [任务摘要](https://huggingface.co/docs/transformers/v4.45.2/zh/task_summary) 查找 `AutoModel` 支持的任务.

现在可以把预处理好的输入批次直接送进模型。你只需要通过 `**` 来解包字典:

```
>>> pt_outputs = pt_model(**pt_batch)
```

模型在 `logits` 属性输出最终的激活结果. 在 `logits` 上应用 softmax 函数来查询概率:

```
>>> from torch import nn

>>> pt_predictions = nn.functional.softmax(pt_outputs.logits, dim=-1)
>>> print(pt_predictions)
tensor([[0.0021, 0.0018, 0.0115, 0.2121, 0.7725],
        [0.2084, 0.1826, 0.1969, 0.1755, 0.2365]], grad_fn=<SoftmaxBackward0>)
```



所有 🤗 Transformers 模型（PyTorch 或 TensorFlow）在最终的激活函数（比如 softmax）*之前* 输出张量， 因为最终的激活函数常常与 loss 融合。模型的输出是特殊的数据类，所以它们的属性可以在 IDE 中被自动补全。模型的输出就像一个元组或字典（你可以通过整数、切片或字符串来索引它），在这种情况下，为 None 的属性会被忽略。

### 保存模型

当你的模型微调完成，你就可以使用 [PreTrainedModel.save_pretrained()](https://huggingface.co/docs/transformers/v4.45.2/zh/main_classes/model#transformers.PreTrainedModel.save_pretrained) 把它和它的分词器保存下来：

```
>>> pt_save_directory = "./pt_save_pretrained"
>>> tokenizer.save_pretrained(pt_save_directory)
>>> pt_model.save_pretrained(pt_save_directory)
```

当你准备再次使用这个模型时，就可以使用 [PreTrainedModel.from_pretrained()](https://huggingface.co/docs/transformers/v4.45.2/zh/main_classes/model#transformers.PreTrainedModel.from_pretrained) 加载它了：

```
>>> pt_model = AutoModelForSequenceClassification.from_pretrained("./pt_save_pretrained")
```

🤗 Transformers 有一个特别酷的功能，它能够保存一个模型，并且将它加载为 PyTorch 或 TensorFlow 模型。`from_pt` 或 `from_tf` 参数可以将模型从一个框架转换为另一个框架：

```
>>> from transformers import AutoModel

>>> tokenizer = AutoTokenizer.from_pretrained(tf_save_directory)
>>> pt_model = AutoModelForSequenceClassification.from_pretrained(tf_save_directory, from_tf=True)
```

 
