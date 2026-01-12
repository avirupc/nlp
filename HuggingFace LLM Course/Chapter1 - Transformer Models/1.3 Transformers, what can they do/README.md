#  Transformers, what can they do?

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter1/3)

[▶️ Video link](https://youtu.be/tiZFewofSLM)

[💻 Colab Notebook link](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/en/chapter1/section3.ipynb)

## 🗒️Section Notes
Pipeline function (imported from `transformers`)

When we pass some text to pipeline:

1. The text is preprocessed into a format the model can understand.
2. The preprocessed inputs are passed to the model.
3. The predictions of the model are post-processed, so you can make sense of them.

Pipelines can be used for different modalities like text, images, audio and even for multimodal tasks. 

We can specify which model to use in the pipeline for a particular task. We can select the model from HuggingFace's model hub using the pipeline function by passing the model name/address as an argument.<br>
Example:
```python
generator = pipeline("text-generation", model="distilgpt2")
```

We used pipeline to perform different text-based tasks, such as:
1. Sentiment analysis
2. Zero-shot classification <br>
(allows  to specify which labels to use for the classification, so one doesn’t have to rely on the labels of the pretrained model)
3. Text generation
4. Mask filling (fill in the blanks in a given text)
5.  Named entity recognition (NER) <br> (the model has to find which parts of the input text correspond to entities such as persons, locations, or organizations)
6. Question answering
7. Summarization
8. Translation




