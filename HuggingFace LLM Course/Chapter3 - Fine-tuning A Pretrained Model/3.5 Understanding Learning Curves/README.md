# 3.5 Understanding Learning Curves

[📖 Chapter link](https://huggingface.co/learn/llm-course/en/chapter3/5)



## 🗒 Section Notes

> _The [Hugging Face chapter](https://huggingface.co/learn/llm-course/en/chapter3/5) for this topic is quite comprehensive and provides a detailed understanding of the concepts. I recommend going through it for an in-depth explanation._
> 
> 
> 
> _The content below has been extracted directly from the Hugging Face chapter._
> 
> 
> 
> _For this chapter, I have not modified or run the accompanying notebook, as I encountered memory issues on my device while trying to do so. I plan to revisit and run it later if necessary. For now, I have included the original, unmodified notebook as-is._


Below, we will break down the different patterns that can be observed in the learning curves:

#### During Training

During the training process (after you've hit `trainer.train()`), you can monitor these key indicators:

1. **Loss convergence**: Is the loss still decreasing or has it plateaued?
2. **Overfitting signs**: Is validation loss starting to increase while training loss decreases?
3. **Learning rate**: Are the curves too erratic (LR too high) or too flat (LR too low)?
4. **Stability**: Are there sudden spikes or drops that indicate problems?

#### After Training

After the training process is complete, you can analyze the complete curves to understand the model's performance.

1. **Final performance**: Did the model reach acceptable performance levels?
2. **Efficiency**: Could the same performance be achieved with fewer epochs?
3. **Generalization**: How close are training and validation performance?
4. **Trends**: Would additional training likely improve performance?


> 🔍 **W&B Dashboard Features**: Weights & Biases automatically creates beautiful, interactive plots of your learning curves. You can:
> - Compare multiple runs side by side
> - Add custom metrics and visualizations  
> - Set up alerts for anomalous behavior
> - Share results with your team
>
> Learn more in the [Weights & Biases documentation](https://docs.wandb.ai/).

#### Overfitting

Overfitting occurs when the model learns too much from the training data and is unable to generalize to different data (represented by the validation set).

<img src="https://huggingface.co/datasets/huggingface-course/documentation-images/resolve/main/en/chapter3/10.png" alt="Overfitting" width="250">

**Symptoms:**

- Training loss continues to decrease while validation loss increases or plateaus
- Large gap between training and validation accuracy
- Training accuracy much higher than validation accuracy

**Solutions for overfitting:**
- **Regularization**: Add dropout, weight decay, or other regularization techniques
- **Early stopping**: Stop training when validation performance stops improving
- **Data augmentation**: Increase training data diversity
- **Reduce model complexity**: Use a smaller model or fewer parameters

In the sample below, we use early stopping to prevent overfitting. We set the `early_stopping_patience` to 3, which means that if the validation loss does not improve for 3 consecutive epochs, the training will be stopped.

```python
# Example of detecting overfitting with early stopping
from transformers import EarlyStoppingCallback

training_args = TrainingArguments(
    output_dir="./results",
    eval_strategy="steps",
    eval_steps=100,
    save_strategy="steps",
    save_steps=100,
    load_best_model_at_end=True,
    metric_for_best_model="eval_loss",
    greater_is_better=False,
    num_train_epochs=10,  # Set high, but we'll stop early
)

# Add early stopping to prevent overfitting
trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=tokenized_datasets["train"],
    eval_dataset=tokenized_datasets["validation"],
    data_collator=data_collator,
    processing_class=tokenizer,
    compute_metrics=compute_metrics,
    callbacks=[EarlyStoppingCallback(early_stopping_patience=3)],
)
```

#### 2. Underfitting

Underfitting occurs when the model is too simple to capture the underlying patterns in the data. This can happen for several reasons:

- The model is too small or lacks capacity to learn the patterns
- The learning rate is too low, causing slow learning
- The dataset is too small or not representative of the problem
- The model is not properly regularized

<img src="https://huggingface.co/datasets/huggingface-course/documentation-images/resolve/main/en/chapter3/7.png" alt="Underfitting" width="250">

**Symptoms:**
- Both training and validation loss remain high
- Model performance plateaus early in training
- Training accuracy is lower than expected

**Solutions for underfitting:**
- **Increase model capacity**: Use a larger model or more parameters
- **Train longer**: Increase the number of epochs
- **Adjust learning rate**: Try different learning rates
- **Check data quality**: Ensure your data is properly preprocessed

In the sample below, we train for more epochs to see if the model can learn the patterns in the data.

```python
from transformers import TrainingArguments

training_args = TrainingArguments(
    output_dir="./results",
    -num_train_epochs=5,
    +num_train_epochs=10,
)
```

#### 3. Erratic Learning Curves

Erratic learning curves occur when the model is not learning effectively. This can happen for several reasons:

- The learning rate is too high, causing the model to overshoot the optimal parameters
- The batch size is too small, causing the model to learn slowly
- The model is not properly regularized, causing it to overfit to the training data
- The dataset is not properly preprocessed, causing the model to learn from noise

<img src="https://huggingface.co/datasets/huggingface-course/documentation-images/resolve/main/en/chapter3/3.png" alt="Erratic Learning Curves" width="250">

**Symptoms:**
- Frequent fluctuations in loss or accuracy
- Curves show high variance or instability
- Performance oscillates without clear trend

Both training and validation curves show erratic behavior.


<img src="https://huggingface.co/datasets/huggingface-course/documentation-images/resolve/main/en/chapter3/9.png" alt="Erratic Learning Curves" width="250">

**Solutions for erratic curves:**
- **Lower learning rate**: Reduce step size for more stable training
- **Increase batch size**: Larger batches provide more stable gradients
- **Gradient clipping**: Prevent exploding gradients
- **Better data preprocessing**: Ensure consistent data quality

In the sample below, we lower the learning rate and increase the batch size.

```python
from transformers import TrainingArguments

training_args = TrainingArguments(
    output_dir="./results",
    -learning_rate=1e-5,
    +learning_rate=1e-4,
    -per_device_train_batch_size=16,
    +per_device_train_batch_size=32,
)
```
