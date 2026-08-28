# 6.3 Fast tokenizers' special powers

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter6/3)

▶ Video links:

1. [Why are fast tokeizers called fast?](https://youtu.be/g8quOxoqhHQ)
2. [Fast tokenizer superpowers](https://youtu.be/3umI3tm27Vw)

## 🗒 Section Notes

_(The screenshots attached below are taken from the video [Fast tokenizer superpowers](https://youtu.be/3umI3tm27Vw) mentioned above)_

Slow tokenizers are those written in Python inside the 🤗 Transformers library, while the fast versions are the ones provided by 🤗 Tokenizers, which are written in Rust. If you remember the table from [Chapter 5.3](../../Chapter5%20-%20%20The%20HuggingFace%20Datasets%20library/5.3%20Time%20to%20slice%20and%20dice/Time_to_slice_and_dice.ipynb) that reported how long it took a fast and a slow tokenizer to tokenize the Drug Review Dataset, you should have an idea of why we call them fast and slow:

Options         | Fast tokenizer | Slow tokenizer
:--------------:|:--------------:|:-------------:
`batched=True`  | 29.7s          | 5min16s
`batched=False` | 2min16s        | 6min18s

⚠️ **WARNING:** When tokenizing a single sentence, you won't always see a difference in speed between the slow and fast versions of the same tokenizer. In fact, the fast version might actually be slower! It's only when tokenizing lots of texts in parallel at the same time that you will be able to clearly see the difference.

### Other benefits of Fast Tokenizers

Besides their parallelization capabilities, the key functionality of fast tokenizers is that they always keep track of the original span of texts the final tokens come from — a feature called **offset mapping**.

<img src="6_3_ss1.png" width="70%"/>

For instance, here 👆 the tokenization is the same for the two sentences, even if one has several more spaces than the other.<br>
Just having the input IDs is thus not enough if we want to match some tokens with a span of text (something we will need to do when tackling question answering for instance).

<img src="6_3_ss2.png" width="70%"/>

It's also difficult to know when two tokens belong to the same word or not: it looks easy when you just look at the output of a BERT tokenizer, we just need to look for the ##. But other tokenizers have different ways to tokenize parts of words.For instance, RoBERTa adds this special G symbol to mark the tokens at the beginning of a word,and T5 uses this special underscore symbol for the same purpose.<br>
Thankfully, the fast tokenizers keep track of the word each token comes from, with a word_ids method you can use on their outputs.

<img src="6_3_ss3.png" width="70%"/>

The output is not necessarily clear, but assembled together in a nice table like this, we can look at the word position for each token.

<img src="6_3_ss4.png" width="70%"/>

Even better, the fast tokenizers keep track of the span of characters each token comes from, and we can get them when calling it on one (or several) text by adding the `return_offsets_mapping=True` argument.<br>
In this instance, we can see how we jump positions between the ##s token and the super token, because of the multiple spaces in the initial sentence. To enable this, the fast tokenizers store additional information at each step of their internal pipeline.

<img src="6_3_ss5.png" width="70%"/>

That internal pipeline consists of normalization, where we apply some cleaning to the text, like 
1. lowercasing or removing the accents
2. pre-tokenization, which is where we split the texts into words
3. applying the model of the tokenizer, which is where the words are splits into tokens
4. doing the post-processing, where special tokens are added.

<img src="6_3_ss6.png" width="70%"/>

From the beginning to the end of the pipeline, the tokenizer keeps track of each span of text that corresponds to each word, then each token.

We will see how useful it is when we tackle the following tasks: 
1. When doing **masked language modeling**, one variation that gets state-of-the-art results is to mask all the tokens of a given word instead of randomly chosen tokens. This will require us to use the word IDs we saw.

2. When doing **token classification**, we'll need to convert the labels we have on words, to labels on each tokens.

3. As for the offset mappings, it will be super useful when we need to convert token positions in a sentence into a span of text, which we will need to know when looking at **question answering** or when grouping the tokens corresponding to the same entity in **token classification**.

We have some examples of this in the [notebook](./Fast_tokenizers'_special_powers_(PyTorch).ipynb) also.
