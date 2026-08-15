# 5.2 Loading a custom dataset

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter5/2)

▶ Video links:
[Loading a custom dataset](https://youtu.be/HyQgpJTkRdE)

## 🗒 Section Notes

In addition to the examples in the [notebook](./What_if_my_dataset_isn't_on_the_Hub_.ipynb), I have recorded below the examples from the above video:

### 1. Loading a Local CSV Dataset

```Python
!wget https://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-white.csv

from datasets import load_dataset

local_csv_dataset = load_dataset("csv", data_files="winequality-white.csv", sep=";")
local_csv_dataset
```
Output:

```out
DatasetDict({
    train: Dataset({
        features: ['fixed acidity', 'volatile acidity', 'citric acid', 'residual sugar', 'chlorides', 'free sulfur dioxide', 'total sulfur dioxide', 'density', 'pH', 'sulphates', 'alcohol', 'quality'],
        num_rows: 4898
    })
})
```

### 2. Loading the CSV directly from its URL

```Python
from datasets import load_dataset

# Load the dataset from the URL directly
dataset_url = "https://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-white.csv"
remote_csv_dataset = load_dataset("csv", data_files=dataset_url, sep=";")
remote_csv_dataset
```
Output:
```out
DatasetDict({
    train: Dataset({
        features: ['fixed acidity', 'volatile acidity', 'citric acid', 'residual sugar', 'chlorides', 'free sulfur dioxide', 'total sulfur dioxide', 'density', 'pH', 'sulphates', 'alcohol', 'quality'],
        num_rows: 4898
    })
})
```

### 3. Loading a text dataset directly from a URL

```Python
from datasets import load_dataset

dataset_url = "https://raw.githubusercontent.com/karpathy/char-rnn/master/data/tinyshakespeare/input.txt"
text_dataset = load_dataset("text", data_files=dataset_url)
text_dataset["train"][:5]
```

Output:

```out
{'text': ['First Citizen:',
 'Before we proceed any further, hear me speak.',
 '',
 'All:',
 'Speak, speak.']}
 ```

 ### 4. Loading a JSON Lines (`.jsonl`) Dataset

```python
dataset_url = (
    "https://raw.githubusercontent.com/hirupert/sede/main/data/sede/train.jsonl"
)
json_lines_dataset = load_dataset("json", data_files=dataset_url)
json_lines_dataset["train"][:2]
```

Output:
```out
{
    "QuerySetId": [466, 784],
    "Title": [
        "Most controversial posts on the site",
        "Comments asking for questions to be made wiki",
    ],
    "Description": [
        "Looks for posts with more than half the amount of downvotes as they have upvotes\nOrdered by upvotes\n",
        "All comments that contain the text should and wiki",
    ],
    "QueryBody": [
        "SELECT \n* from Votes",
        "SELECT PostId as [Post Link], Text from Comments\nwhere Text like '%should%wiki%'",
    ],
    "CreationDate": [
        datetime.datetime(2020, 6, 24, 11, 23, 10),
        datetime.datetime(2019, 7, 7, 11, 1, 51),
    ],
    "validated": [False, False],
}
```

### 5. Loading a JSON Dataset with a Specific Field

```Python
dataset_url = "https://rajpurkar.github.io/SQuAD-explorer/dataset/train-v2.0.json"
json_dataset = load_dataset("json", data_files=dataset_url, field="data")
json_dataset
```

Output:
```out
DatasetDict({
    train: Dataset({
        features: ['title', 'paragraphs'],
        num_rows: 442
    })
})
```

### 6. Specifying Multiple Splits with data_files:

```Python
url = "https://rajpurkar.github.io/SQuAD-explorer/dataset/"
data_files = {"train": f"{url}train-v2.0.json", "validation": f"{url}dev-v2.0.json"}
json_dataset = load_dataset("json", data_files=data_files, field="data")
json_dataset
```

Output:

```out
DatasetDict({
    train: Dataset({
        features: ['title', 'paragraphs'],
        num_rows: 442
    }),
    validation: Dataset({
        features: ['title', 'paragraphs'],
        num_rows: 35
    })
})
```