# 2.2 Behind the piepline

[📖 Chapter link](https://huggingface.co/learn/llm-course/en/chapter2/2)

▶ Video links:<br>
https://youtu.be/1pedAIvTWXk


## 🗒 Section Notes

**_(The video included in this chapter is quite good and gives a complete overview of this chapter. Be sure to check that out!)_**

There are three main stages in a HuggingFace pipeline:<br>

**Tokenizer** ➡️ **Model** ➡️ **Postprocessing**

### Tokenizer

The tokenizer has following conversions:<br>

Raw text: "This course is amazing!" <br>
⬇️<br>
Tokens: ['this', 'course', 'is', 'amazing', '!']<br>
⬇️<br>
Special tokens: [[CLS], 'this', 'course', 'is', 'amazing', '!', [SEP]] ([CLS] and [SEP] denotes start and end of the sentences)<br>
⬇️<br>
Input IDs: [101, 2023, ......., 102]

The tokenizer returns two tensors for an input sentence:
1. input IDs: [101, 2023, ......., 102, 0, 0, 0,..., 0] (0s denote masking at the end of the sentence)
2. attention mask: [1, 1, 1, 1,..., 0, 0,..., 0]

### Model
* The base Transformer module takes tokenized inputs and produces **hidden states** (also called features), which are high-dimensional vectors representing the contextual meaning of each input token.

* These hidden states typically have three dimensions: **batch size**, **sequence length**, and **hidden size**.
    * Batch size is the number of sequences processed at a time.
    * Sequence length is the length of the numerical representation of the sequence.
    * Hidden size is the vector dimension of each model input. The hidden size makes the output “high-dimensional” (e.g., 768 or more).

* The hidden states are usually passed to a **model head**, which is task-specific and made of one or more linear layers that transform these vectors into outputs suited for a specific task.

* While the core Transformer architecture can remain the same across tasks, different tasks (like classification) use different heads to project the high-dimensional outputs into lower-dimensional results (e.g., one value per label).

### Postprocessing

For a classification task like sentiment analysis, the model produces logits which are just numbers that do not make sense themselves. Hence, they are converted to probabilities using softmax.

logits<br>

⬇️ <sub>(softmax)</sub> <br>

probabilities<br>

⬇️<br>

labels