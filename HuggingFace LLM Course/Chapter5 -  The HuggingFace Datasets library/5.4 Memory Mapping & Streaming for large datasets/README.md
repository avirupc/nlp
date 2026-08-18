# 5.4 Memory Mapping & Streaming for large datasets

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter5/4)

▶ Video links: 
[Memory Mapping & Streaming](https://youtu.be/JwISwTCPPWo)


## 🗒 Section Notes

🤗 Datasets helps handle very large datasets that can’t fit comfortably into RAM or on a local hard drive.

- It uses memory-mapped files, allowing datasets to be accessed without loading everything into RAM.
- It supports streaming, so data can be read piece-by-piece rather than storing the entire dataset locally.
- This makes it practical to work with multi-gigabyte corpora, such as the 40 GB WebText dataset used to pretrain GPT-2.

In short: 🤗 Datasets makes huge datasets manageable by reducing both memory usage and storage requirements.

Rest of the content is illustrated in the [notebook](./Big_data__🤗_Datasets_to_the_rescue!.ipynb).

---

### A Note on Cleaning Up Downloaded Data and Cache Files:

This chapter (and some of the others) requires downloading data files, some of which can be quite large. Since I am almost always running out of disk space 😅, I would like to keep a little note here on how to clean up the files we no longer need once we are done with the experiments.

First, if you loaded the dataset using 🤗 Datasets, you can remove the cached files associated with the dataset with the following helper function:

```Python
import shutil
from pathlib import Path


def clear_dataset_cache(dataset):
    """Remove the cached files associated with a Hugging Face dataset."""
    
    cache_files = dataset.cache_files

    # Get the unique directories containing the cached files
    cache_dirs = {
        Path(file["filename"]).parent
        for file in cache_files
        if "filename" in file
    }

    for cache_dir in cache_dirs:
        if cache_dir.exists():
            shutil.rmtree(cache_dir)
            print(f"Deleted: {cache_dir}")


# Example:
clear_dataset_cache(pubmed_dataset)
```
>Note: Run this only when you are completely done with the dataset. The function removes the cached files associated with the dataset object you pass to it.


Also check the directories like: 
- "C:\Users\<user_name>\.cache\huggingface\datasets"
- "C:\Users\<user_name>\.cache\huggingface\hub"

It's worth checking what was recently added there and whether the folder/file names resemble the dataset or model you were just working with. For example, in the current case, searching for something like `pubmed` can help identify the relevant files.

You can also find the actual cache locations from Python:

```Python
from datasets import config
from huggingface_hub import constants

print("Datasets cache:")
print(config.HF_DATASETS_CACHE)

print("\nHugging Face Hub cache:")
print(constants.HF_HUB_CACHE)
```

Just be a little careful here. The cache may contain other datasets and models from previous experiments, so I wouldn't recommend deleting the entire `datasets` or `hub` directory just to clean up one experiment.

And most importantly: **RESTART THE KERNEL!** 🚨

This one caught me out.

Even after deleting the cached files, the disk space may not immediately come back because the Jupyter/Python kernel can still have files open or memory-mapped.

So my usual cleanup routine is:

1. Finish the experiment.
2. Delete the dataset-specific cache.
3. Check the Hugging Face cache directories for any remaining 
4. files related to the experiment.
5. Restart the Jupyter kernel.
6. Check the available disk space again.