# Transformer Architectures

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter1/6)

▶ Video links:
1. [Encoders](https://www.youtube.com/watch?v=MUqNwgPjJvQ)
2. [Decoders](https://youtu.be/d_ixlCubqQw?si=IGqdm17vYNxaldUM)
3. [Encoders-Decoders](https://www.youtube.com/watch?v=0_4KEb08xrE)

## 🗒 Section Notes

### Overview of different NLP Tasks, Use Cases, and Model Architectures
(combined three tables into one from the article)

| Task / Capability                     | Description & Example Use Case                                                                 | Suggested Architecture              | Example Models        |
|----------------------------------------|------------------------------------------------------------------------------------------------|------------------------------------|-----------------------|
| Text generation                        | Creating coherent and contextually relevant text (e.g., writing essays, stories, or emails) | Decoder                            | GPT, LLaMA            |
| Text classification (sentiment, topic) | Categorizing text into predefined labels (e.g., sentiment analysis, topic labeling)         | Encoder                            | BERT, RoBERTa         |
| Summarization                          | Condensing long documents into shorter versions      | Encoder-Decoder                    | BART, T5              |
| Translation      | Converting text between languages (e.g., English to Spanish)                    | Encoder-Decoder                    | Marian, T5, BART      |
| Question answering (extractive)        | Extracting answers directly from provided context (e.g., answering factual questions from passages) | Encoder                    | BERT, RoBERTa         |
| Question answering (generative)        | Generating answers based on context (e.g., open-ended question answering)                   | Encoder-Decoder or Decoder         | T5, GPT, BART         |
| Code generation                        | Writing or completing code snippets (e.g., creating a function from a description)          | Decoder                            | GPT                   |
| Reasoning                              | Solving problems step by step using logical reasoning (e.g., solving math or logic problems) | Decoder or Encoder-Decoder        | GPT, T5               |
| Few-shot learning                      | Learning from a few examples in the prompt (e.g., classifying text after 2–3 examples)      | Decoder or Encoder-Decoder         | GPT, T5               |
| Data-to-text generation                | Converting structured data into natural language (e.g., generating reports from datasets)   | Encoder-Decoder                    | T5                    |
| Grammar correction                     | Fixing grammatical errors in text                      | Encoder-Decoder                    | T5                    |
| Named entity recognition               | Identifying entities such as names, places, organizations | Encoder                    | BERT, RoBERTa         |
| Conversational AI                      | Engaging in interactive dialogue (e.g., chatbots and virtual assistants)                    | Decoder                            | GPT, LLaMA            |
