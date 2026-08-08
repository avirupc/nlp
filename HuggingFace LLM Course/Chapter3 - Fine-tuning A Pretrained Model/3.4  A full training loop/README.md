# 3.4  A full training loop

[📖 Chapter link](https://huggingface.co/learn/llm-course/en/chapter3/4)

[Accelerate Documentation](https://huggingface.co/docs/accelerate/)

▶ Video links:

1. [Write Your Training Loop in Pytorch](https://youtu.be/Dh9CL8fyG80)
2. [Hugging Face Accelerate](https://youtu.be/s7dy8QRgjJ0)
## 🗒 Section Notes

💡 **Key Takeaways:**
- Manual training loops give you complete control but require understanding of the proper sequence: forward → backward → optimizer step → scheduler step → zero gradients
- AdamW with weight decay is the recommended optimizer for transformer models
- Always use `model.eval()` and `torch.no_grad()` during evaluation for correct behavior and efficiency
- 🤗 Accelerate makes distributed training accessible with minimal code changes
- Device management (moving tensors to GPU/CPU) is crucial for PyTorch operations
- Modern techniques like mixed precision, gradient accumulation, and gradient clipping can significantly improve training efficiency

### 🚀 Modern Optimization Tips: 
 
 For better performance:
 - **AdamW with weight decay**: `AdamW(model.parameters(), lr=5e-5, weight_decay=0.01)`
 - **8-bit Adam**: Use `bitsandbytes` for memory-efficient optimization
 - **Different learning rates**: Lower learning rates (1e-5 to 3e-5) often work better for large models

 🚀 **Optimization Resources**: Learn more about optimizers and training strategies in the [🤗 Transformers optimization guide](https://huggingface.co/docs/transformers/main/en/erformance#optimizer).
<br><br>
### 💡 Modern Training Optimizations: 
 
 To make training loop more efficient, consider:

 - **Gradient Clipping**: Add `torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)` before `optimizer.step()`
 - **Mixed Precision**: Use `torch.cuda.amp.autocast()` and `GradScaler` for faster training
 - **Gradient Accumulation**: Accumulate gradients over multiple batches to simulate larger batch sizes
 - **Checkpointing**: Save model checkpoints periodically to resume training if interrupted

 🔧 **Implementation Guide**: For detailed examples of these optimizations, see the [🤗 Transformers efficient training guide](https://huggingface.co/docs/transformers/main/en/perf_train_gpu_one) and the [range of optimizers](https://huggingface.co/docs/transformers/main/en/optimizers).
 <br><br>

 ### 📊 Evaluation Best Practices: 
 For more sophisticated evaluation strategies and metrics, explore the [🤗 Evaluate documentation](https://huggingface.co/docs/evaluate/) and the [comprehensive evaluation cookbook](https://github.com/huggingface/evaluation-guidebook).