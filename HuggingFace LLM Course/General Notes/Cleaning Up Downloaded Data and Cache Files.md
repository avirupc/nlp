### A Note on Cleaning Up Downloaded Data and Cache Files

Some chapters in this course require downloading data files, and some of them can be **quite large**. Since I am almost always running out of disk space 😅, I thought it would be useful to keep a little note here on how to clean up files we no longer need once we are done with the experiments.

I have already added a similar note to the README of [Chapter 5.4](/HuggingFace%20LLM%20Course/Chapter5%20-%20%20The%20HuggingFace%20Datasets%20library/5.4%20Memory%20Mapping%20%26%20Streaming%20for%20large%20datasets), but I thought it would be useful to keep it here as a general note for this course.

First, if you loaded a dataset using 🤗 Datasets, you can remove the cached files associated with that dataset using the following helper function:

```python
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

It's worth checking what was recently added there and whether the folder/file names resemble the dataset or model you were just working with.

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

That last step is particularly important on Windows. Otherwise, you might delete several GB of data and still wonder:

> "Wait... where did all my disk space go?" 😅