#  How HuggingFace Transformers solve tasks

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter1/5)

▶️ Video links:
1. [How 🤗 Transformers solve tasks](https://youtu.be/zsfR7eY9Uho)


## 🗒️Section Notes

 ### Types of Language Models:
 1. **Encoder-only models:**
    - bidirectional approach to understand context from both directions. 
    - best suited for tasks that require deep **understanding of text**, such as classification, named entity recognition, and question answering.

    Example: **BERT**

2. **Decoder-only models:** 
    - process text from left to right.
    - good at text generation tasks. can complete sentences, write essays, or even generate code based on a prompt.

    Example: **GPT, Llama**

3. **Encoder-decoder models:** 
    - combine both approaches, using an encoder to understand the input and a decoder to generate output. 
    - Good at sequence-to-sequence tasks like translation, summarization, and question answering.

    Example: **T5, BART**

### Model Training Approaches:
There are two main approaches for training a transformer model:

1. **Masked language modeling (MLM):** 
Used by encoder models like BERT, this approach randomly masks some tokens in the input and trains the model to predict the original tokens based on the surrounding context. This allows the model to learn bidirectional context (looking at words both before and after the masked word).

2. **Causal language modeling (CLM):** 
Used by decoder models like GPT, this approach predicts the next token based on all previous tokens in the sequence. The model can only use context from the left (previous tokens) to predict the next token.