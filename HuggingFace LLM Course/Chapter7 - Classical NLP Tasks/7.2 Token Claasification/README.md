# 7.2 Token Claasification

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter7/2)

▶ Video links:
1. [Token Classification](https://youtu.be/wVHdVlPScxA)
2. [Data Processing for Token Classification](https://youtu.be/iY2AZYdZAr0)

## 🗒 Section Notes

### What is token classification?

Token Classification encompasses any problem that can be formulated as "attributing a label to each token in a sentence," such as:

- **Named entity recognition (NER)**: Find the entities (such as persons, locations, or organizations) in a sentence. This can be formulated as attributing a label to each token by having one class per entity and one class for "no entity."
- **Part-of-speech tagging (POS)**: Mark each word in a sentence as corresponding to a particular part of speech (such as noun, verb, adjective, etc.).
- **Chunking**: Find the tokens that belong to the same entity. This task (which can be combined with POS or NER) can be formulated as attributing one label (usually `B-`) to any tokens that are at the beginning of a chunk, another label (usually `I-`) to tokens that are inside a chunk, and a third label (usually `O`) to tokens that don't belong to any chunk.

Token classification models are evaluated by metrics like accuracy, precision, recall, F1 score.<br> 
These metrics are calculated for each of the classes (through calculating True Positive (TP), True Negative (TN), False Positive (FP) for each class) and then averaged over all classes to evaluate the model.