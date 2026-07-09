# 2.4 Tokenizers

[📖 Chapter link](https://huggingface.co/learn/llm-course/en/chapter2/4)

▶ Video links:

1. [Tokenizers Overview](https://youtu.be/VFp38yj8h3A)
2. [Word-based Tokenizers](https://youtu.be/nhJxYji1aho)
3. [Character-based Tokenizer](https://youtu.be/ssLq_EK2jLE)
4. [Subword-based Tokenizer](https://youtu.be/zHvTiHr506c)
5. [The Tokenization Pipeline](https://youtu.be/Yffk5aydLzg)
## 🗒 Section Notes

### Word-based tokenizers
```Python
tokenized_text = "Jim Henson was a puppeteer".split()
print(tokenized_text)
```
```
Output:
['Jim', 'Henson', 'was', 'a', 'puppeteer']
```

### Character-based tokenizers

Character-based tokenizers split the text into characters, rather than words. This has two primary benefits:

1. The vocabulary is much smaller.
2. There are much fewer out-of-vocabulary (unknown) tokens, since every word can be built from characters.

But:
- less meaningful - each character doesn’t mean a lot on its own.
<br> However, this again differs according to the language; in Chinese, for example, each character carries more information than a character in a Latin language.
- We will end up with a very large amount of tokens to be processed by our model: whereas a word would only be a single token with a word-based tokenizer, it can easily turn into 10 or more tokens when converted into characters.

### Subword-based tokenizers

Subword tokenization algorithms rely on the principle that frequently used words should not be split into smaller subwords, but rare words should be decomposed into meaningful subwords.

For instance, “annoyingly” might be considered a rare word and could be decomposed into “annoying” and “ly”. These are both likely to appear more frequently as standalone subwords, while at the same time the meaning of “annoyingly” is kept by the composite meaning of “annoying” and “ly”.

### Encoding

Translating text to numbers is known as encoding. Encoding is done in a two-step process: the **tokenization**, followed by the **conversion to input IDs**.

```Python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")

sequence = "Using a Transformer network is simple"
tokens = tokenizer.tokenize(sequence)

print(tokens)
```
```
Output:
['Using', 'a', 'transform', '##er', 'network', 'is', 'simple']
```

```Python
ids = tokenizer.convert_tokens_to_ids(tokens)

print(ids)
```

```
Output:
[7993, 170, 11303, 1200, 2443, 1110, 3014]
```

### Decoding

Decoding is going the other way around: from vocabulary indices, we want to get a string. This can be done with the decode() method as follows:

```Python
decoded_string = tokenizer.decode([7993, 170, 11303, 1200, 2443, 1110, 3014])
print(decoded_string)
```

```
Output:
'Using a Transformer network is simple'
```

Note that the decode method not only converts the indices back to tokens, but also groups together the tokens that were part of the same words to produce a readable sentence. This behavior is extremely useful when we use models that predict new text (either text generated from a prompt, or for sequence-to-sequence problems like translation or summarization).