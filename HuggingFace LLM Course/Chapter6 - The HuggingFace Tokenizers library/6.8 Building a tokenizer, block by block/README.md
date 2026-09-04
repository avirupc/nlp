# 6.8 Building a tokenizer, block by block

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter6/8)

▶ Video links:
[Building a New Tokenizer](https://youtu.be/MR8tZm5ViWU)

## 🗒 Section Notes

We have seen in previous chapters that tokenization comprises several steps:

- Normalization (any cleanup of the text that is deemed necessary, such as removing spaces or accents, Unicode normalization, etc.)
- Pre-tokenization (splitting the input into words)
- Running the input through the model (using the pre-tokenized words to produce a sequence of tokens)
- Post-processing (adding the special tokens of the tokenizer, generating the attention mask and token type IDs)

 In this section we'll see how we can build a tokenizer from scratch using the 🤗 Tokenizers library, as opposed to training a new tokenizer from an old one as we did in [chapter 6.2](../6.2%20Training%20a%20new%20tokenizer%20from%20an%20old%20one/).

 More precisely, the library is built around a central `Tokenizer` class with the building blocks regrouped in submodules:

- `normalizers` contains all the possible types of `Normalizer` we can use (complete list [here](https://huggingface.co/docs/tokenizers/api/normalizers)).
- `pre_tokenizers` contains all the possible types of `PreTokenizer` we can use (complete list [here](https://huggingface.co/docs/tokenizers/api/pre-tokenizers)).
- `models` contains the various types of `Model` we can use, like `BPE`, `WordPiece`, and `Unigram` (complete list [here](https://huggingface.co/docs/tokenizers/api/models)).
- `trainers` contains all the different types of `Trainer` we can use to train your model on a corpus (one per type of model; complete list [here](https://huggingface.co/docs/tokenizers/api/trainers)).
- `post_processors` contains the various types of `PostProcessor` we can use (complete list [here](https://huggingface.co/docs/tokenizers/api/post-processors)).
- `decoders` contains the various types of `Decoder` we can use to decode the outputs of tokenization (complete list [here](https://huggingface.co/docs/tokenizers/components#decoders)).

The whole list of building blocks is [here](https://huggingface.co/docs/tokenizers/components).

In this chapter, we have built tokenizers step-by-step using three main tokenization algorithms -  WordPiece, BPE and Unigram. Check the [notebook](./Building_a_tokenizer,_block_by_block.ipynb)!