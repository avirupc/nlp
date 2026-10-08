# 7.3 Fine-tuning a Masked Language Model

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter7/3)

▶ Video links:
1. [Tasks: Masked Language Modeling](https://youtu.be/mqElG5QJWUg)
2. [Data Processing for Masked Language Modeling](https://youtu.be/8PmhEIXhBvI)
3. [What is Perplexity?](https://youtu.be/NURcDHhYe98)
4. [What is Domain Adaptation](https://youtu.be/0Oxphw4Q9fo)


## 🗒 Section Notes


# Context

- For many **NLP applications using Transformer models**, you can:
  - Take a **pretrained model** from the Hugging Face Hub.
  - Fine-tune it directly on your own dataset.
  - Train it for a specific task, such as classification or question answering.

- **Transfer learning** usually works well when:
  - The data used to pretrain the model is **similar to your data**.
  - For example, a model pretrained on general English text may work well on another general English dataset.

## Why Do We Need Domain Adaptation?

Sometimes your dataset is very different from the data the model was originally trained on.

Examples:

- **Legal documents** → contracts, laws, court documents
- **Scientific documents** → research papers, technical articles
- **Medical documents** → medical reports and research

In these cases:

- A general Transformer model such as **BERT** may not understand domain-specific vocabulary very well.
- Words that are common in your domain may be treated as **rare or unfamiliar tokens**.
- This can reduce the model's performance on your specific task.

## What Is Domain Adaptation?

**Domain adaptation** means:

> **Fine-tuning a pretrained language model on data from your specific domain before using it for a particular task.**

The process looks like this:

1. Start with a **pretrained language model** such as BERT.
2. Fine-tune it using **in-domain data**.
   - For example, train BERT on a large collection of legal documents.
3. Use this adapted model for your **specific downstream task**.
   - For example, classifying legal documents.

## Why Is It Useful?

- Domain adaptation can **improve performance** on downstream tasks.
- You generally only need to perform the **domain adaptation step once** for a particular domain.
- After that, the adapted model can potentially be reused for **multiple tasks** within that domain.

## ULMFiT

- **ULMFiT (2018)** helped popularize the idea of domain adaptation in NLP.
- It was one of the early neural architectures that demonstrated that **transfer learning could work effectively for NLP**.
- ULMFiT was based on **LSTMs**.
- The same general idea can be applied today using **Transformer models** instead of LSTMs.
