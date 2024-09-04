# 2.3 处理多段文本

在实际应用中，我们往往需要同时处理大量长度各异的文本。而且所有的神经网络模型都只接受批 (batch) 数据作为输入，即使只输入一段文本，也需要先将它组成只包含一个样本的 batch，然后才能送入模型，例如：

```
import torch
from transformers import AutoTokenizer, AutoModelForSequenceClassification

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)

sequence = "I've been waiting for a HuggingFace course my whole life."

tokens = tokenizer.tokenize(sequence)
ids = tokenizer.convert_tokens_to_ids(tokens)
# input_ids = torch.tensor(ids), This line will fail.
input_ids = torch.tensor([ids])
print("Input IDs:\n", input_ids)

output = model(input_ids)
print("Logits:\n", output.logits)
Input IDs: 
tensor([[ 1045,  1005,  2310,  2042,  3403,  2005,  1037, 17662, 12172,  2607,
          2026,  2878,  2166,  1012]])
Logits: 
tensor([[-2.7276,  2.8789]], grad_fn=<AddmmBackward0>)
```

这里我们通过 `[ids]` 手工为输入增加了一个 batch 维（这个 batch 只包含一段文本），更多情况下送入的是包含多段文本的 batch：

```
batched_ids = [ids, ids, ids, ...]
```

**再次强调：**上面的演示只是为了便于我们更好地理解分词背后的原理。实际应用中，我们应该直接使用分词器对文本进行处理，例如对于上面的例子：

```
import torch
from transformers import AutoTokenizer, AutoModelForSequenceClassification

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)

sequence = "I've been waiting for a HuggingFace course my whole life."

tokenized_inputs = tokenizer(sequence, return_tensors="pt")
print("Input IDs:\n", tokenized_inputs["input_ids"])

output = model(**tokenized_inputs)
print("Logits:\n", output.logits)
Input IDs:
tensor([[  101,  1045,  1005,  2310,  2042,  3403,  2005,  1037, 17662, 12172,
          2607,  2026,  2878,  2166,  1012,   102]])
Logits:
tensor([[-1.5607,  1.6123]], grad_fn=<AddmmBackward0>)
```

可以看到，分词器输出的结果字典中，token IDs 只是其中的一项（`input_ids`），字典中还会包含其他的输入项。前面我们之所以只输入 token IDs 模型也能正常运行，是因为它自动地补全了其他的输入项，例如 `attention_mask` 等，后面我们会具体介绍。

> 小提示：下面的例子中，分词器自动在 token 序列的首尾添加了 `[CLS]` 和 `[SEP]` token，所以上面两个例子中模型的输出是有差异的。因为 DistilBERT 在预训练时的输入中就包含 `[CLS]` 和 `[SEP]`，所以下面例子才是正确的使用方法。

### Padding 操作

将多段文本按批 (batch) 输入会产生的一个直接问题就是：batch 中的文本有长有短，而输入张量 (tensor) 必须是严格的二维矩形，维度为 `(batch size, token IDs sequence length)`，换句话说每一个文本编码后的 token IDs 的数量必须一样多。例如下面的 ID 列表是无法转换为张量的：

```
batched_ids = [
    [200, 200, 200],
    [200, 200]
]
```

我们需要通过 Padding 操作，在短序列的最后填充特殊的 *padding token*，使得 batch 中所有的序列都具有相同的长度，例如：

```
padding_id = 100

batched_ids = [
    [200, 200, 200],
    [200, 200, padding_id],
]
```

每个预训练模型使用的 padding token 的 ID 可能有所不同，可以通过其对应分词器的 `pad_token_id` 属性获得。下面我们尝试将两段文本分别以独立以及组成 batch 的形式送入到模型中：

```
import torch
from transformers import AutoTokenizer, AutoModelForSequenceClassification

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)

sequence1_ids = [[200, 200, 200]]
sequence2_ids = [[200, 200]]
batched_ids = [
    [200, 200, 200],
    [200, 200, tokenizer.pad_token_id],
]

print(model(torch.tensor(sequence1_ids)).logits)
print(model(torch.tensor(sequence2_ids)).logits)
print(model(torch.tensor(batched_ids)).logits)
tensor([[ 1.5694, -1.3895]], grad_fn=<AddmmBackward0>)
tensor([[ 0.5803, -0.4125]], grad_fn=<AddmmBackward0>)
tensor([[ 1.5694, -1.3895],
        [ 1.3374, -1.2163]], grad_fn=<AddmmBackward0>)
```

`问题出现了！` 在组成 batch 后，使用 padding token 填充的序列的结果出现了问题，与单独送入模型时的预测结果不同。这是因为 Transformer 模型会编码输入序列中的每一个 token 以建模完整的上下文，因此这里会将填充的 padding token 也当成是普通 token 一起编码，从而生成了不同的上下文语义表示。

因此，在进行 Padding 操作的同时，我们必须明确地告诉模型哪些 token 是我们填充的，它们不应该参与编码，这就需要使用到 attention mask。

> 在前面例子中，除了 token IDs 之外，我们还经常能看到一个 `attention_mask` 项，这就是下面要介绍的 Attention Mask。

### Attention masks

Attention masks 是一个与 input IDs 尺寸完全相同的仅由 0 和 1 组成的张量，其中 0 表示对应位置的 token 是填充符，不应该参与 attention 层的计算，而应该只基于 1 对应位置的 token 来建模上下文。

> 除了标记填充字符位置以外，许多特定的模型结构也会使用 Attention masks 来遮蔽掉一些 tokens。

对于上面的例子，如果我们通过 `attention_mask` 标出填充的 padding token 的位置，计算结果就不会有问题了：

```
import torch
from transformers import AutoTokenizer, AutoModelForSequenceClassification

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)

sequence1_ids = [[200, 200, 200]]
sequence2_ids = [[200, 200]]
batched_ids = [
    [200, 200, 200],
    [200, 200, tokenizer.pad_token_id],
]
batched_attention_masks = [
    [1, 1, 1],
    [1, 1, 0],
]

print(model(torch.tensor(sequence1_ids)).logits)
print(model(torch.tensor(sequence2_ids)).logits)
outputs = model(
    torch.tensor(batched_ids), 
    attention_mask=torch.tensor(batched_attention_masks))
print(outputs.logits)
tensor([[ 1.5694, -1.3895]], grad_fn=<AddmmBackward0>)
tensor([[ 0.5803, -0.4125]], grad_fn=<AddmmBackward0>)
tensor([[ 1.5694, -1.3895],
        [ 0.5803, -0.4125]], grad_fn=<AddmmBackward0>)
```

**再次提醒：**这里只是为了演示。实际使用时，应该直接使用分词器对文本进行处理，它不仅会向 token 序列中添加 `[CLS]`、`[SEP]` 等特殊字符，还会自动地生成对应的 Attention masks。

目前大部分 Transformer 模型只能处理长度为 512 或 1024 的 token 序列，如果你需要处理的序列长度大于 1024，有以下两种处理方法：

- 使用一个支持长文的 Transformer 模型，例如 [Longformer](https://huggingface.co/transformers/model_doc/longformer.html)和 [LED](https://huggingface.co/transformers/model_doc/led.html)（最大长度 4096）；
- 设定一个最大长度 `max_sequence_length` 以**截断**输入序列：`sequence = sequence[:max_sequence_length]`。

### 直接使用分词器

前面我们介绍了分词、转换 token IDs、padding、构建 attention masks 以及截断等操作。实际上，直接使用分词器就能实现所有的这些操作。

```
from transformers import AutoTokenizer

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)

sequences = [
    "I've been waiting for a HuggingFace course my whole life.", 
    "So have I!"
]

model_inputs = tokenizer(sequences)
print(model_inputs)
{'input_ids': [
    [101, 1045, 1005, 2310, 2042, 3403, 2005, 1037, 17662, 12172, 2607, 2026, 2878, 2166, 1012, 102], 
    [101, 2061, 2031, 1045, 999, 102]], 
 'attention_mask': [
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1], 
    [1, 1, 1, 1, 1, 1]]
}
```

可以看到，分词器的输出包含了模型需要的所有输入项。例如对于 DistilBERT 模型，就是 input IDs（`input_ids`）和 Attention mask（`attention_mask`）。

**padding 操作**通过 `padding` 参数来控制：

- `padding="longest"`： 将 batch 内的序列填充到当前 batch 中最长序列的长度；
- `padding="max_length"`：将所有序列填充到模型能够接受的最大长度，例如 BERT 模型就是 512。

```
from transformers import AutoTokenizer

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)

sequences = [
    "I've been waiting for a HuggingFace course my whole life.", 
    "So have I!"
]

model_inputs = tokenizer(sequences, padding="longest")
print(model_inputs)

model_inputs = tokenizer(sequences, padding="max_length")
print(model_inputs)
{'input_ids': [
    [101, 1045, 1005, 2310, 2042, 3403, 2005, 1037, 17662, 12172, 2607, 2026, 2878, 2166, 1012, 102], 
    [101, 2061, 2031, 1045, 999, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]], 
 'attention_mask': [
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1], 
    [1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]
}

{'input_ids': [
    [101, 1045, 1005, 2310, 2042, 3403, 2005, 1037, 17662, 12172, 2607, 2026, 2878, 2166, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, ...], 
    [101, 2061, 2031, 1045, 999, 102, 0, 0, 0, ...]], 
 'attention_mask': [
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, ...], 
    [1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ...]]
}
```

**截断操作**通过 `truncation` 参数来控制，如果 `truncation=True`，那么大于模型最大接受长度的序列都会被截断，例如对于 BERT 模型就会截断长度超过 512 的序列。此外，也可以通过 `max_length` 参数来控制截断长度：

```
from transformers import AutoTokenizer

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)

sequences = [
    "I've been waiting for a HuggingFace course my whole life.", 
    "So have I!"
]

model_inputs = tokenizer(sequences, max_length=8, truncation=True)
print(model_inputs)
{'input_ids': [
    [101, 1045, 1005, 2310, 2042, 3403, 2005, 102], 
    [101, 2061, 2031, 1045, 999, 102]], 
 'attention_mask': [
    [1, 1, 1, 1, 1, 1, 1, 1], 
    [1, 1, 1, 1, 1, 1]]
}
```

分词器还可以通过 `return_tensors` 参数指定返回的张量格式：设为 `pt` 则返回 PyTorch 张量；`tf` 则返回 TensorFlow 张量，`np` 则返回 NumPy 数组。例如：

```
from transformers import AutoTokenizer

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)

sequences = [
    "I've been waiting for a HuggingFace course my whole life.", 
    "So have I!"
]

model_inputs = tokenizer(sequences, padding=True, return_tensors="pt")
print(model_inputs)

model_inputs = tokenizer(sequences, padding=True, return_tensors="np")
print(model_inputs)
{'input_ids': tensor([
    [  101,  1045,  1005,  2310,  2042,  3403,  2005,  1037, 17662, 12172,
      2607,  2026,  2878,  2166,  1012,   102],
    [  101,  2061,  2031,  1045,   999,   102,     0,     0,     0,     0,
         0,     0,     0,     0,     0,     0]]), 
 'attention_mask': tensor([
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
    [1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]])
}

{'input_ids': array([
    [  101,  1045,  1005,  2310,  2042,  3403,  2005,  1037, 17662,
     12172,  2607,  2026,  2878,  2166,  1012,   102],
    [  101,  2061,  2031,  1045,   999,   102,     0,     0,     0,
         0,     0,     0,     0,     0,     0,     0]]), 
 'attention_mask': array([
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
    [1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]])
}
```

实际使用分词器时，我们通常会同时进行 padding 操作和截断操作，并设置返回格式为 Pytorch 张量，这样就可以直接将分词结果送入模型：

```
from transformers import AutoTokenizer, AutoModelForSequenceClassification

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)

sequences = [
    "I've been waiting for a HuggingFace course my whole life.", 
    "So have I!"
]

tokens = tokenizer(sequences, padding=True, truncation=True, return_tensors="pt")
print(tokens)
output = model(**tokens)
print(output.logits)
{'input_ids': tensor([
    [  101,  1045,  1005,  2310,  2042,  3403,  2005,  1037, 17662, 12172,
      2607,  2026,  2878,  2166,  1012,   102],
    [  101,  2061,  2031,  1045,   999,   102,     0,     0,     0,     0,
         0,     0,     0,     0,     0,     0]]), 
 'attention_mask': tensor([
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
    [1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]])}

tensor([[-1.5607,  1.6123],
        [-3.6183,  3.9137]], grad_fn=<AddmmBackward0>)
```

可以看到在 `padding=True, truncation=True` 这样的设置下，同一个 batch 中的序列都会 pad 到相同的长度，并且大于模型最大接受长度的序列会被自动截断。

### 编码句子对

在上面的例子中，我们都是对单个序列进行编码（即使通过 batch 处理多段文本，也是并行地编码单个序列），而实际上对于 BERT 等包含“句子对”分类预训练任务的模型来说，都支持对“句子对”进行编码，例如：

```
from transformers import AutoTokenizer

checkpoint = "bert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)

inputs = tokenizer("This is the first sentence.", "This is the second one.")
print(inputs)

tokens = tokenizer.convert_ids_to_tokens(inputs["input_ids"])
print(tokens)
{'input_ids': [101, 2023, 2003, 1996, 2034, 6251, 1012, 102, 2023, 2003, 1996, 2117, 2028, 1012, 102], 
 'token_type_ids': [0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1], 
 'attention_mask': [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]}

['[CLS]', 'this', 'is', 'the', 'first', 'sentence', '.', '[SEP]', 'this', 'is', 'the', 'second', 'one', '.', '[SEP]']
```

可以看到分词器自动使用 `[SEP]` token 拼接了两个句子，输出形式为“[CLS] sentence1 [SEP] sentence2 [SEP][CLS] sentence1 [SEP] sentence2 [SEP]”的 token 序列，这也是 BERT 模型预期的输入格式。返回结果中除了前面我们介绍过的 `input_ids` 和 `attention_mask` 之外，还包含了一个 `token_type_ids` 项，用于标记输入序列中哪些 token 属于第一个句子，哪些属于第二个句子。对于上面的例子，如果我们将 `token_type_ids` 项与 token 序列对齐：

```
['[CLS]', 'this', 'is', 'the', 'first', 'sentence', '.', '[SEP]', 'this', 'is', 'the', 'second', 'one', '.', '[SEP]']
[      0,      0,    0,     0,       0,          0,   0,       0,      1,    1,     1,        1,     1,   1,       1]
```

可以看到第一个句子“[CLS] sentence1 [SEP][CLS] sentence1 [SEP]”片段所有 tokens 的 token type ID 都为 0，而第二个句子“sentence2 [SEP]sentence2 [SEP]”片段对应的 token type ID 都是 1。

> 如果我们选择其他的预训练模型，分词器的输出不一定会包含 `token_type_ids` 项（例如 DistilBERT 模型）。分词器的输出格式只需保证与模型在预训练时的输入格式保持一致即可。

实际使用时，我们不需要去关注编码结果中是否包含 `token_type_ids` 项，分词器会根据 checkpoint 自动调整适用于对应模型的格式，例如：

```
from transformers import AutoTokenizer

checkpoint = "bert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)

sentence1_list = ["This is the first sentence 1.", "second sentence 1."]
sentence2_list = ["This is the first sentence 2.", "second sentence 2."]

tokens = tokenizer(
    sentence1_list,
    sentence2_list,
    padding=True,
    truncation=True,
    return_tensors="pt"
)
print(tokens)
print(tokens['input_ids'].shape)
{'input_ids': tensor([
    [ 101, 2023, 2003, 1996, 2034, 6251, 1015, 1012,  102, 2023, 2003, 1996,
     2034, 6251, 1016, 1012,  102],
    [ 101, 2117, 6251, 1015, 1012,  102, 2117, 6251, 1016, 1012,  102,    0,
        0,    0,    0,    0,    0]]), 
 'token_type_ids': tensor([
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1],
    [0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0]]), 
 'attention_mask': tensor([
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0]])}

torch.Size([2, 17])
```

可以看到分词器成功地输出了形式为“[CLS] sentence1 [SEP] sentence2 [SEP][CLS] sentence1 [SEP] sentence2 [SEP]”的 token 序列，并且将两个 token 序列都 pad 到了相同的长度。
