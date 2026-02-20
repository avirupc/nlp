# Inference with LLMs

[📖 Chapter link](https://huggingface.co/learn/llm-course/en/chapter1/8)

▶ Video links:
1. [Deep dive into Text Generation Inference with LLMs](https://youtu.be/Xp2w1_LKZN4)

## 🗒 Section Notes

🎮 **_This article/chapter includes several interactive tools that help you visualize key internal steps. Be sure to try them out!!_**

### What is inference? <br>
Inference is the process of using a trained LLM to generate human-like text from a given input prompt. 

### What is the role of attention in LLMs? <br>
The attention mechanism is what gives LLMs their ability to understand context and generate coherent responses. When predicting the next word, not every word in a sentence carries equal weight - for example, in the sentence “The capital of France is …”, the words “France” and “capital” are crucial for determining that “Paris” should come next. This ability to focus on relevant information is what we call attention.

### What is Context Length (or Attention Span)?<br> 
The context length refers to the maximum number of tokens (words or parts of words) that the LLM can process at once. Think of it as the size of the model’s working memory.

---

### The Two-Phase Inference Process
-  The Prefill Phase<br>
This phase involves three key steps:

    - **Tokenization:** Converting the input text into tokens
    - **Embedding Conversion:** Transforming these tokens into numerical representations that capture their meaning
    - **Initial Processing:** Running these embeddings through the model’s neural networks to create a rich understanding of the context

    This phase is computationally intensive because it needs to process all input tokens at once. Think of it as reading and understanding an entire paragraph before starting to write a response.


- The Decode Phase<br>
This is where the actual text generation happens. The model generates one token at a time in what we call an autoregressive process (where each new token depends on all previous tokens).
The decode phase involves several key steps that happen for each new token:<br>
    - **Probability Calculation:** Determining the likelihood of each possible next token
    - **Token Selection:** Choosing the next token based on these probabilities
    - **Continuation Check:** Deciding whether to continue or stop generation

    This phase is memory-intensive because the model needs to keep track of all previously generated tokens and their relationships.

---

### Understanding Token Selection: From Probabilities to Token Choices

- **Raw Logits**: Initial scores (unnormalized probabilities) for every possible next token.
- **Temperature**: Controls randomness.
  - \> 1.0 → More creative/random
  - < 1.0 → More deterministic/focused
- **Top-p (Nucleus) Sampling**: Select from the smallest set of tokens whose cumulative probability reaches a threshold (e.g., 0.9).
- **Top-k Filtering**: Select only from the top *k* most probable tokens.

> These methods convert model probabilities into actual token choices.

---

### Managing Repetition: Keeping Output Fresh

- **Problem**: Models may repeat words or phrases.
- **Presence Penalty**: Penalizes any token that has appeared before (regardless of frequency).
- **Frequency Penalty**: Penalizes tokens proportionally to how often they’ve appeared.

> Penalties adjust probabilities *before* sampling, encouraging vocabulary diversity.

---

### Controlling Generation Length: Setting Boundaries

- **Token Limits**: Set minimum and/or maximum token counts.
- **Stop Sequences**: Predefined patterns (e.g., `\n\n`) that stop generation.
- **End-of-Sequence (EOS)**: Let the model naturally stop.

> Ensures outputs match required length (e.g., tweet vs. blog post).

---

### Beam Search: Looking Ahead for Better Coherence

- Maintains multiple candidate sequences at each step (e.g., 5–10 beams).
- Expands each candidate with possible next tokens.
- Keeps only the most promising sequence combinations.
- Selects the sequence with the highest overall probability.

> Produces more coherent text but requires more computation.

---

### Practical Challenges and Optimization

#### Key Performance Metrics

1. **Time to First Token (TTFT)**: Delay before the first output token (affects responsiveness).
2. **Time Per Output Token (TPOT)**: Speed of generating subsequent tokens.
3. **Throughput**: Number of simultaneous requests handled.
4. **VRAM Usage**: GPU memory consumption (often the main constraint).

---

#### The Context Length Challenge

- Longer context provides more information but increases cost.
- **Memory usage**: Grows quadratically with context length.
- **Speed**: Decreases with longer contexts.
- Requires careful balancing of VRAM.
- Very large context windows (e.g., 1M tokens) are powerful but slower.

> Balance context size with performance requirements.

---

#### The KV Cache Optimization

- Stores intermediate Key-Value (KV) computations.
- Avoids recomputing previous tokens.
- Significantly improves generation speed, especially for long contexts.
- Trade-off: Increased memory usage.

> Essential optimization for efficient long-context inference.
