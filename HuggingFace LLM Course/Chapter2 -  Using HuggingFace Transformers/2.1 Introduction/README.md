# 2.1 Introduction

[📖 Chapter link](https://huggingface.co/learn/llm-course/en/chapter2/1)

## 🗒 Section Notes

1. **Transformer Model Complexity**:
   - Transformers are large, with millions to billions of parameters, making training and deployment challenging.

2. **🤗 Transformers Library**:
   - **Purpose**: A unified API to load, train, and save Transformer models easily.
   - **Key Features**:
     - **Ease of Use**: Minimal code to use state-of-the-art models.
     - **Flexibility**: Models are PyTorch `nn.Module` classes, allowing easy integration into existing ML workflows.
     - **Simplicity**: Models are self-contained, with their forward pass defined in a single file, making them more understandable and hackable.

3. **Model API Overview**:
   - The chapter will provide an end-to-end example using a model and tokenizer together, similar to the `pipeline()` function from Chapter 1.
   - A deep dive into the model and configuration classes, how models process numerical inputs, and how they output predictions.

4. **Tokenizer API**:
   - Tokenizers handle text preprocessing (converting text to numerical inputs) and post-processing (converting outputs back to text).

5. **Batch Processing**:
   - Explanation of handling multiple sentences in a batch for efficient model processing.

6. **Tokenizer Function**:
   - A high-level overview of the `tokenizer()` function.