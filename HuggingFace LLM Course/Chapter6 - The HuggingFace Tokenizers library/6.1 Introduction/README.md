# 6.1 Introduction

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter6/1)


## 🗒 Section Notes

This chapter explains how to train a tokenizer from scratch when building a language model. Unlike fine-tuning, where we reuse the model’s pretrained tokenizer, training from scratch often requires a tokenizer tailored to the new domain or language. For example, an English tokenizer may work poorly for Japanese text because of differences in spacing and punctuation.

This  chapter introduces the 🤗 Tokenizers library, which is used to create new tokenizers. This will all be done with the help of the 🤗 Tokenizers library, which provides the “fast” tokenizers in the 🤗 Transformers library. We’ll take a close look at the features that this library provides, and explore how the fast tokenizers differ from the “slow” versions.

We will see:

- How to train a new tokenizer similar to the one used by a given checkpoint on a new corpus of texts
- The special features of fast tokenizers
- The differences between the three main subword tokenization algorithms used in NLP today
- How to build a tokenizer from scratch with the 🤗 Tokenizers library and train it on some data