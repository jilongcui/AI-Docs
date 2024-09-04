# 2.2 分词器

因为神经网络模型不能直接处理文本，我们需要先将文本转换为模型能够处理的数字，这个过程被称为**编码 (Encoding)**：先使用分词器 (Tokenizers) 将文本按词、子词、符号切分为 tokens；然后将 tokens 映射到对应的 token 编号（token IDs）。

### 分词策略

根据切分粒度的不同，分词策略大概可以分为以下几种：

- **按词切分 (Word-based)** 规则简单，而且能产生不错的结果。

  ![word_based_tokenization](https://xiaosheng.blog/img/article/transformers-note-2/word_based_tokenization.png)

  例如直接利用 Python 自带的 `split()` 函数按空格进行分词：

  ```
  tokenized_text = "Jim Henson was a puppeteer".split()
  print(tokenized_text)
  ```

  ```
  ['Jim', 'Henson', 'was', 'a', 'puppeteer']
  ```

  这种策略的问题是会将文本中所有出现过的独立片段都作为不同的 token，从而产生巨大的词表。而实际上词表中很多词是相关的，例如 “dog” 和 “dogs”、“run” 和 “running”，如果给它们赋不同的编号就无法表示出这种关联性。

  > 词表就是一个映射字典，负责将每个 token 映射到对应的编号 (IDs)，编号从 0 开始，一直到词表中所有 token 的数量，神经网络模型就是通过这些 token IDs 来区分每一个 token。

  当遇到不在词表中的词时，分词器会使用一个专门的 `[UNK]` token 来表示它是 unknown 的。显然，如果分词结果中包含很多 `[UNK]` token 就意味着丢掉了很多文本信息，因此**一个好的分词策略，应该尽可能不出现 unknown tokens**。

- **按字符切分 (Character-based)** 按更细的粒度进行分词，比如按字符切分。

  ![character_based_tokenization](https://xiaosheng.blog/img/article/transformers-note-2/character_based_tokenization.png)

  这种策略把文本切分为字符而不是词语，这样就只会产生一个非常小的词表，并且很少会出现词表外的 tokens。

  但是从直觉上来看，字符本身并没有太大的意义，因此将文本切分为字符之后就会变得不容易理解。这也与语言有关，例如中文字符会比拉丁字符包含更多的信息，相对影响较小。此外，这种方式切分出的 tokens 会很多，例如一个由 10 个字符组成的单词就会输出 10 个 tokens，而实际上它们只是一个词。

  因此现在广泛采用的是一种同时结合了按词切分和按字符切分的方式——按子词切分 (Subword tokenization)。

- **按子词 (Subword) 切分** 高频词直接保留，低频词被切分为更有意义的子词。

- 例如 “annoyingly” 就是一个低频词，可以切分为 “annoying” 和 “ly”，这两个子词不仅出现频率更高，而且词义也得以保留。下图就是对文本 “Let’s do tokenization!“ 按子词切分的例子：

  ![bpe_subword](https://xiaosheng.blog/img/article/transformers-note-2/bpe_subword.png)

  可以看到，“tokenization” 被切分为了 “token” 和 “ization”，不仅保留了语义，而且只用两个 token 就表示了一个长词。这中策略只用一个较小的词表就可以覆盖绝大部分的文本，基本不会产生 unknown tokens。尤其对于土耳其语等黏着语言，可以通过串联多个子词构成几乎任意长度的复杂长词。

### 加载与保存分词器

分词器的加载与保存与模型非常相似，也是使用 `from_pretrained()` 和 `save_pretrained()` 函数。例如，使用 `BertTokenizer` 类加载并保存 BERT 模型的分词器：

```
from transformers import BertTokenizer

tokenizer = BertTokenizer.from_pretrained("bert-base-cased")
tokenizer.save_pretrained("./models/bert-base-cased/")
```

与 `AutoModel` 类似，在大部分情况下，我们都应该使用 `AutoTokenizer` 类来加载分词器，它会根据 checkpoint 来自动选择对应的分词器：

```
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")
tokenizer.save_pretrained("./models/bert-base-cased/")
```

调用 `Tokenizer.save_pretrained()` 函数会在保存路径下创建三个文件：

- special_tokens_map.json：配置文件，里面包含 unknown tokens 等特殊字符的映射关系；
- tokenizer_config.json：配置文件，里面包含构建分词器需要的参数；
- vocab.txt：词表，每一个 token 占一行，行号就是对应的 token ID（从 0 开始）。

### 编码与解码文本

完整的文本编码 (Encoding) 过程实际上包含两个步骤：

1. **分词：**使用分词器按某种策略将文本切分为 tokens；
2. **映射：**将 tokens 转化为对应的 token IDs。

> 因为不同预训练模型采用的分词策略并不相同，因此我们需要通过向 `Tokenizer.from_pretrained()` 函数传递模型 checkpoint 的名称来加载对应的分词器和词表。

下面，我们尝试使用 BERT 分词器来对文本进行分词：

```
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")

sequence = "Using a Transformer network is simple"
tokens = tokenizer.tokenize(sequence)

print(tokens)
['Using', 'a', 'Trans', '##former', 'network', 'is', 'simple']
```

可以看到，BERT 分词器采用的是子词 (subword) 切分策略：它会不断切分词语直到获得词表中的 token，例如 “transformer” 会被切分为 “transform” 和 “##er”。

然后，我们通过 `convert_tokens_to_ids()` 将切分出的 tokens 转换为对应的 token IDs：

```
ids = tokenizer.convert_tokens_to_ids(tokens)

print(ids)
[7993, 170, 13809, 23763, 2443, 1110, 3014]
```

还可以通过 `encode()` 函数将这两个步骤合并，并且 `encode()` 会自动添加模型需要的特殊字符。例如对于 BERT 会自动在 token 序列的首尾分别添加 `[CLS]` 和 `[SEP]` token：

```
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")

sequence = "Using a Transformer network is simple"
sequence_ids = tokenizer.encode(sequence)

print(sequence_ids)
[101, 7993, 170, 13809, 23763, 2443, 1110, 3014, 102]
```

其中 101 和 102 分别是 `[CLS]` 和 `[SEP]` 对应的 token IDs。

**注意：**实际编码文本时，更为常见的是直接使用分词器进行处理。这样返回的结果中不仅包含处理后的 token IDs，还包含模型需要的其他辅助输入。例如对于 BERT 模型还会自动在输入中添加 `token_type_ids` 和 `attention_mask`：

```
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")
tokenized_text = tokenizer("Using a Transformer network is simple")
print(tokenized_text)
{'input_ids': [101, 7993, 170, 13809, 23763, 2443, 1110, 3014, 102], 
 'token_type_ids': [0, 0, 0, 0, 0, 0, 0, 0, 0], 
 'attention_mask': [1, 1, 1, 1, 1, 1, 1, 1, 1]}
```

文本解码 (Decoding) 与编码相反，负责将 token IDs 转化为原来的字符串。注意，解码过程不是简单地将 token IDs 映射回 tokens，还需要合并那些被分词器分为多个 token 的单词。下面我们尝试通过 `decode()` 函数解码前面生成的 token IDs：

```
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")

decoded_string = tokenizer.decode([7993, 170, 11303, 1200, 2443, 1110, 3014])
print(decoded_string)

decoded_string = tokenizer.decode([101, 7993, 170, 13809, 23763, 2443, 1110, 3014, 102])
print(decoded_string)
Using a transformer network is simple
[CLS] Using a Transformer network is simple [SEP]
```

解码文本是一个重要的步骤，当我们运用模型来预测新的文本时，都会调用这一函数。例如根据模板 (prompt) 生成文本、翻译或者摘要等 seq2seq 问题等等。
