# 4.2 Using pretrained models

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter4/2)


## 🗒 Section Notes

In this chapter, we used a a **French-based** model that can perform **mask filling**. <br>
For this we needed to do the following:
1. Go to [HuggingFace Model Hub](https://huggingface.co/models)
2. Select **Fill-Mask** from the Tasks section on the side-pane.
3. Select the preferred Python library from the Libraries section. We selected **Pytorch**.
4. Select **French** from the Languages section.
5. A list of filtered models will appear on the right side. Select [camembert-base](https://huggingface.co/almanach/camembert-base) from there and check the documentation.

Rest of the work is described in the [Notebook](Using_pretrained_models_(PyTorch).ipynb) directly.

## Cleanup

After experimenting with a downloaded model, I cleaned up its resources to free **RAM, GPU VRAM, and disk space**.

The cleanup process has two parts:

1. **Free memory:** Delete the model, tokenizer, and pipeline objects, run Python's
   garbage collector, and clear PyTorch's unused GPU cache.
2. **Free disk space:** Find the Hugging Face cache location, search for cached
   CamemBERT files, review the matching directories, and then remove only the
   CamemBERT-related cache and lock files.

The search step is useful because it lets me see exactly where the downloaded
model files are stored before deleting them.

> **Note:** Removing a model from the Hugging Face cache does not uninstall
> Transformers or PyTorch. It only removes the downloaded model files. If I use
> the model again later, Hugging Face will download it again.

Here is the code:

```Python
import gc
import os
import shutil
from pathlib import Path

import torch


# ============================================================
# 1. Free RAM and GPU VRAM
# ============================================================

for name in ["camembert_fill_mask", "tokenizer", "model"]:
    if name in globals():
        del globals()[name]

# Run Python's garbage collector
gc.collect()

# Clear unused PyTorch GPU cache
if torch.cuda.is_available():
    torch.cuda.empty_cache()
    torch.cuda.ipc_collect()


# ============================================================
# 2. Find the Hugging Face cache
# ============================================================

cache_dir = Path(
    os.environ.get(
        "HF_HOME",
        Path.home() / ".cache" / "huggingface"
    )
)

hub_dir = cache_dir / "hub"

print("=" * 60)
print("Hugging Face cache")
print("=" * 60)
print(cache_dir)


# ============================================================
# 3. Search for CamemBERT-related cached files
# ============================================================

print("\n" + "=" * 60)
print("Searching for CamemBERT cache files...")
print("=" * 60)

camembert_matches = list(hub_dir.rglob("*camembert*"))

if camembert_matches:
    for path in camembert_matches:
        print(path)
else:
    print("No CamemBERT cache files found.")


# ============================================================
# 4. List only CamemBERT cache directories
# ============================================================

camembert_dirs = [
    hub_dir / "models--camembert--camembert-base-wikipedia-4gb",
    hub_dir / "models--camembert-base",

    # Corresponding lock directories
    hub_dir / ".locks" / "models--camembert--camembert-base-wikipedia-4gb",
    hub_dir / ".locks" / "models--camembert-base",
]


# ============================================================
# 5. Show what will be deleted
# ============================================================

print("\n" + "=" * 60)
print("CamemBERT directories selected for deletion")
print("=" * 60)

for path in camembert_dirs:
    if path.exists():
        print(path)
    else:
        print(f"Not found: {path}")


# ============================================================
# 6. Delete the selected CamemBERT cache
# ============================================================

print("\n" + "=" * 60)
print("Deleting CamemBERT cache...")
print("=" * 60)

for path in camembert_dirs:
    if path.exists():
        print(f"Deleting: {path}")
        shutil.rmtree(path)

print("\nCleanup complete.")


# ============================================================
# 7. Verify that CamemBERT cache files are gone
# ============================================================

print("\n" + "=" * 60)
print("Verifying cleanup...")
print("=" * 60)

remaining = list(hub_dir.rglob("*camembert*"))

if remaining:
    print("The following CamemBERT files still remain:")
    for path in remaining:
        print(path)
else:
    print("No CamemBERT cache files found.")
```