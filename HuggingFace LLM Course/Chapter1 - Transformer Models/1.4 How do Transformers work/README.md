#  Transformers, what can they do?

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter1/4)

▶️ Video links:
1. [The Carbon Footprint of Transformers](https://youtu.be/ftWlj4FBHTg)
2. [What is Transfer Learning](https://youtu.be/BqqfQnyjmgg)
3. [The Transformer Architecture](https://youtu.be/H39Z_720T5s)

Additional videos (these videos can be later found in chapter 1.6):
1. [Encoders](https://www.youtube.com/watch?v=MUqNwgPjJvQ)
2. [Decoders](https://youtu.be/d_ixlCubqQw?si=IGqdm17vYNxaldUM)
3. [Encoders-Decoders](https://www.youtube.com/watch?v=0_4KEb08xrE)

## 🗒️Section Notes

### Benefits of transfer learning:
- The pretrained model was already trained on a dataset that has some similarities with the fine-tuning dataset. The fine-tuning process is thus able to take advantage of knowledge acquired by the initial model during pretraining (for instance, with NLP problems, the pretrained model will have some kind of statistical understanding of the language you are using for your task).
- Since the pretrained model was already trained on lots of data, the fine-tuning requires way less data to get decent results.
- For the same reason, the amount of time and resources needed to get good results are much lower.

-----------------------------------------------------------------
### Transformer:

- A transformer model primarily consists of two components - Encoder and Decoder. Each of these parts can be used independently, depending on the task:

    - **Encoder-only models:** Good for tasks that require understanding of the input, such as sentence classification and named entity recognition.
    - **Decoder-only models:** Good for generative tasks such as text generation.
    - **Encoder-decoder models or sequence-to-sequence models:** Good for generative tasks that require an input, such as translation or summarization.

### Attention layer:
In simple words, this layer will tell the model to pay specific attention to certain words in the sentence you passed it (and more or less ignore the others) when dealing with the representation of each word.

To put this into context, consider the task of translating text from English to French. Given the input “You like this course”, a translation model will need to also attend to the adjacent word “You” to get the proper translation for the word “like”, because in French the verb “like” is conjugated differently depending on the subject. The rest of the sentence, however, is not useful for the translation of that word. In the same vein, when translating “this” the model will also need to pay attention to the word “course”, because “this” translates differently depending on whether the associated noun is masculine or feminine. Again, the other words in the sentence will not matter for the translation of “course”. With more complex sentences (and more complex grammar rules), the model would need to pay special attention to words that might appear farther away in the sentence to properly translate each word.

The same concept applies to any task associated with natural language: a word by itself has a meaning, but that meaning is deeply affected by the context, which can be any other word (or words) before or after the word being studied.

###  The original Transformer architecture

- Was originally built for translation
-  In the encoder, the attention layers can use all the words in a sentence.
- The decoder, however, works sequentially and can only pay attention to the words in the sentence that it has already translated (so, only the words before the word currently being generated).
- To speed things up during training (when the model has access to target sentences), the decoder is fed the whole target, but it is not allowed to use future words. For instance, when trying to predict the fourth word, the attention layer will only have access to the words in positions 1 to 3.

I had some struggle to understand the following part (mentioned after outlining the Transformer architecture diagram):<br>
_"Note that the first attention layer in a decoder block pays attention to all (past) inputs to the decoder, but the second attention layer uses the output of the encoder. It can thus access the whole input sentence to best predict the current word. This is very useful as different languages can have grammatical rules that put the words in different orders, or some context provided later in the sentence may be helpful to determine the best translation of a given word."_

So, I am providing some explanation I got online:

The famous "decoder only sees past" rule only applies to the target language (the sentence we're generating), because of Auto-regressive property – must generate sequentially like humans write.<br>
The source language (input sentence) → we want the decoder to see everything all the time, because Translation needs full context from source language.

![The Illustrated Transformer – Jay Alammar](https://jalammar.github.io/images/t/transformer_resideual_layer_norm_3.png)

![alt text](https://jalammar.github.io/images/t/transformer_decoding_2.gif)

Image source: https://jalammar.github.io/illustrated-transformer/


Still not very clear to me, need to spend some more time!😅