#  Transformers, what can they do?

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter1/3)

[▶️ Video link](https://youtu.be/tiZFewofSLM)

[💻 Colab Notebook link](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/en/chapter1/section3.ipynb)

(Oops! Looks like I need to install Torch first for running this locally!☹️ Was being too optimistic actually🥲. Will update soon!)

**Update:** I have installed Pytorch and the whole process with it is described here (yeah, totally unnecessary, but I thought I just should record it). 

## Torch Setup

### Part 1: The CUDA 

⚠️**Caution:** The steps mentioned here are **NOT** a set of instructions to follow  (you'll understand why as you go through it). These are simply the steps I took (and the challenges I faced) during the process.

🔧 **Background:** I have an NVIDIA 4060 GPU in my laptop, and my operating system is Windows 11.

#### Step 1: Check CUDA Version
As advised by ChatGPT, I initially ran the following command in Command Prompt:

```bash
nvcc --version
```
However, this didn’t work because I hadn’t installed the CUDA Toolkit yet.

Then to check the CUDA version, I ran

```bash
nvidia-smi
```

This command showed me several things, including that my CUDA version was **12.9**.

#### Step 2: Download the CUDA Tookit
I searched for the CUDA Toolkit 12.9 and found the official download link [here](https://developer.nvidia.com/cuda-12-9-0-download-archive).

I selected:

- **OS:** Windows
- **Architecture:** x86_64
- **Version:** 11

I then downloaded the installer, which was about 3 GB in size.

#### Step 3: Install CUDA 12.9 Toolkit
After running the installer, it prompted me to proceed with the installation. However, it showed an warning indicating that I didn’t have the **compatible version of Visual Studio** required for this operation.

#### Step 4: Install Visual Studio
Without much research (I know, my mistake 🥲), I downloaded and installed the latest coommunity (free) version of Visual Studios, which is Visual Studios **2026**. 

The installation size was around 10 GB.

❗ **Important:** During installation, I had to select the **Desktop Development with C++ Workload**, which is essential for CUDA.

#### Step 5: Back to installing CUDA 12.9 Toolkit
As advised, after rebooting my system, I retired the CUDA Toolkit installation, but **AGAIN** it showed that the Visual Studio version wasn’t compatible.

After some research, I found that Visual Studio 2026 (which had been recently released) is **not yet supported by CUDA**. To resolve this, I needed to install Visual Studio 2022, which is supported by CUDA.

#### Step 6: Install Visual Studio 2022
Here comes the most frustrating part of the process. I couldn't find the **Community version** of Visual Studio 2022 on the official website. The only options available were Professional and Enterprise versions, which are free for a trial but require a paid license afterward.

After a lot of digging, I finally found a Reddit link to download the Community version of Visual Studio 2022.

I then uninstalled Visual Studio 2026, installed Visual Studio 2022, and selected the **Desktop Development with C++ Workload** during installation. (The installation size was smaller than the 2026 version, but I don't recall the exact size.)


#### Step 7: Back to installing CUDA 12.9 Toolkit, again!

After a system reboot, I retried the CUDA toolkit installation. The process completed successfully, but the newly installed NVIDIA app didn’t launch properly. I rebooted my system again, but that did noty help either.

#### Step 8: Reinstalling NVIDIA app

After consulting some online resources, I uninstalled the NVIDIA app, downloaded the latest version from [here](https://www.nvidia.com/en-eu/software/nvidia-app/) and reinstalled it.

Upon reinstallation, I noticed that the GeForce Experience app is not there anymore on my system. However, after launching the NVIDIA app, I found an option to install GeForce Experience. 

After the installation, I checked my system with 
```bash
nvidia-smi
``` 
And Oto my surprise, the CUDA version was now shown as **13.1!!😐😐😐**


#### Step 9: Install CUDA 13.1 
Now that my system was showing CUDA 13.1, I needed to **install the CUDA Toolkit for version 13.1**. 

I followed the similar path as mentioned here in [Step 2](#step-2-download-the-cuda-tookit), but this time for CUDA 13.1, which can be found [here](https://developer.nvidia.com/cuda-downloads) (⚠️ Note: This link always directs you to the latest version, so make sure to check the version before installing to ensure compatibility with your system.).

The installation ran smoothly, and once it finished, I checked everything:

✅ Ran ```nvidia-smi```

✅ Ran ```nvcc --version```

✅ Checked if the newly installed CUDA is set as an environment variable (in System Properties > Environment Variables)

And...Everything was good! 🎉


### Part 2: The Torch

Now, it was time to install PyTorch that is compatible with my system (Windows and CUDA 13.1).

#### Step 1: Install PyTorch

I checked the official PyTorch installation page to find the correct version for my system. You can find it [here](https://pytorch.org/).

I selected the following options as per my setup:

- **PyTorch Build:** Stable (2.9.1)

- **Your OS:** Windows
- **Package:** Pip
- **Language:** Python
**Compute Platform:** CUDA 13.0 

Note: CUDA 13.1 was not listed as an option at the time. I chose CUDA 13.0, as it's said to be backward compatible with CUDA 13.1. 🤞


Upon selecting the options, the provided command to install PyTorch was:

```bash
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
```

#### Verify the installation
Then I verified if everything is running okay through following code:

```Python
import torch
import torchvision

print(f"PyTorch Version: {torch.__version__}")
print(f"TorchVision Version: {torchvision.__version__}")

# Check if CUDA is available
cuda_available = torch.cuda.is_available()
print(f"CUDA Available: {cuda_available}")
```
Output:
```bash
PyTorch Version: 2.9.1+cu130
TorchVision Version: 0.24.1+cu130
CUDA Available: True
CUDA Device Name: NVIDIA GeForce RTX 4060 Laptop GPU
```
```python
if cuda_available:
    # Get GPU device name
    print(f"CUDA Device Name: {torch.cuda.get_device_name(0)}")
else:
    print("CUDA is not available. Running on CPU.")

if cuda_available:
    # Create a random tensor and move it to GPU
    x = torch.rand(10000, 10000).cuda()

    # Check if the tensor is on the GPU
    print(f"Tensor is on GPU: {x.is_cuda}")

    # Perform a computation on the GPU
    y = torch.rand(10000, 10000).cuda()
    result = torch.matmul(x, y)
    print("Matrix multiplication completed on GPU!")
else:
    print("CUDA is not available. Running on CPU.")
```
Output:
```bash
Tensor is on GPU: True
Matrix multiplication completed on GPU!
```
```python
if cuda_available:
    # Check GPU memory stats
    total_memory = torch.cuda.get_device_properties(0).total_memory / 1024**3  # GB
    allocated_memory = torch.cuda.memory_allocated(0) / 1024**3  # GB
    reserved_memory = torch.cuda.memory_reserved(0) / 1024**3  # GB

    print(f"Total GPU Memory: {total_memory:.2f} GB")
    print(f"Memory Allocated: {allocated_memory:.2f} GB")
    print(f"Memory Reserved: {reserved_memory:.2f} GB")
```

Output:
```bash
Total GPU Memory: 8.00 GB
Memory Allocated: 1.13 GB
Memory Reserved: 1.14 GB
```


Everything ran successfully, and I received the expected results! 🎉 
With PyTorch and CUDA now properly set up, I’m all set to resume my exploration and dive deeper into learning NLP. 🚀





