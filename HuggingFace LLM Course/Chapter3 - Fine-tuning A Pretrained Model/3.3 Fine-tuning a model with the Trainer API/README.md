# 3.3 Fine-tuning a model with the Trainer API

[📖 Chapter link](https://huggingface.co/learn/llm-course/en/chapter3/3)

[💻 Colab Notebook link](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/en/chapter3/section3.ipynb)

▶ Video link: [The Trainer API](https://youtu.be/nvBXf7s7vTI)


## 🗒 Section Notes

This chapter recommends to go through the following articles before diving into the training:

1. https://huggingface.co/docs/transformers/main/en/trainer <br><br>
Details on training arguments required for the trainer: https://huggingface.co/docs/transformers/main/en/main_classes/trainer#transformers.TrainingArguments


2. https://huggingface.co/learn/cookbook/en/fine_tuning_code_llm_on_single_gpu

---

The following section talks about some specific settings in the trainer:

### Advanced Training Features

The `Trainer` comes with many built-in features that make modern deep learning best practices accessible:

#### 1. Mixed Precision Training: 
Use `fp16=True` in training arguments for faster training and reduced memory usage:

```Python
training_args = TrainingArguments(
    "test-trainer",
    eval_strategy="epoch",
    fp16=True,  # Enable mixed precision
)
```

#### 2. Gradient Accumulation: 
For effective larger batch sizes when GPU memory is limited:

```Python
training_args = TrainingArguments(
    "test-trainer",
    eval_strategy="epoch",
    per_device_train_batch_size=4,
    gradient_accumulation_steps=4,  # Effective batch size = 4 * 4 = 16
)
```

#### 3. Learning Rate Scheduling: 
The Trainer uses linear decay by default, but we can customize this:

```Python
training_args = TrainingArguments(
    "test-trainer",
    eval_strategy="epoch",
    learning_rate=2e-5,
    lr_scheduler_type="cosine",  # Try different schedulers
)
```
