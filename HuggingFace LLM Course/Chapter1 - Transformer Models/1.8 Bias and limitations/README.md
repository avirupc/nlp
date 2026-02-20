# Bias and limitations

[📖 Chapter link](https://huggingface.co/learn/llm-course/en/chapter1/9)

[💻 Colab Notebook link](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/en/chapter1/section8.ipynb)


## 🗒 Section Notes

* Pretrained models, including fine-tuned versions, come with limitations due to training on vast amounts of internet-scraped data, which includes both high-quality and biased content.
* Even seemingly neutral models like BERT, trained on Wikipedia and BookCorpus, can still exhibit gender and occupation biases (e.g., associating "woman" with occupations like "prostitute" - as shown in the example notebook).
* When using these models in production, be aware that they might generate biased, sexist, racist, or homophobic content.
* Fine-tuning on your own data does not necessarily eliminate these inherent biases.
