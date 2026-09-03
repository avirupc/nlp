# 6.7 Unigram tokenization

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter6/7)

▶ Video links:
[Unigram Tokenization](https://youtu.be/TGZfZVuF9Yc)


## 🗒 Section Notes

*(I haven’t yet executed the code for this chapter as I usually do. However, I’ve added the [notebook](Unigram_tokenization.ipynb) along with the full chapter contents [here](6_7_Unigram_tokenization.md). I plan to revisit this section in the near future to study it thoroughly and make edits as needed.)*

- The overall training strategy of a Unigram LM tokenizer is to start with a very large vocabulary and then to remove tokens at each iteration until we reach the desired size.
- At each iteration, we will calculate a loss (Unigram loss) on our training corpus.
- We look at the evolution of the loss by removing in turn each token from the vocabulary.
- We will choose to remove the p percents which increase the loss the less.

### What is an Unigram model?
The Unigram LM model is a type of statistical Language Model. A statistical LM will assign a probability to a text considering that the text is in fact a sequence of tokens.

The simplest sequences of tokens to imagine are the words that compose the sentence or
the characters. The particularity of Unigram LM is that it assumes that the occurrence of each word is independent of its previous word.
This assumption allows us to write that the probability of a text is equal to the
p**roduct of the probabilities** of the tokens that compose it.

This is a very simple model which **would not be adapted to the generation of text** since this model **would always generate the same token, the one which has the greatest probability**.
Nevertheless, to do tokenization, this model is very useful to us because it can be used
to estimate the relative likelihood of different phrases.