# 5.6 Semantic Search with FAISS

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter5/6)

▶ Video links:

[Text Embeddings & Semantic Search](https://youtu.be/OATCgQtNX2o)


## 🗒 Section Notes

In this chapter  we will explore how Transformer models represent text as embedding vectors and how these vectors  can be used to find similar documents in a corpus. 

### What are embeddings?
Text embedding is a representation of text as an array of numbers called a vector. To create these embeddings we  usually use an encoder-based model like BERT.  

For example:


| Sentence | Embeddings |
| --- | --- |
| I took my dog for a walk | `0.1` `0.5` `1.7` `2.3` `0.6` `6.3` |
| Today is going to rain | `0.8` `2.5` `7.4` `1.8` `0.9` `0.1` |
| I took my cat for a walk | `0.1` `0.5` `1.4` `2.2` `0.6` `5.7` |

Reading  the text, we can see that walking the  dog seems to be most similar to walking the  cat, but we need to quantify this.

### Similarity Metric

To determine how similar two pieces of text are, we compute a distance or similarity metric between their corresponding embedding vectors.

A popular metric for this is **cosine similarity**, which measures the cosine of the angle between two vectors. The mathematical formula is defined as:

$$\text{cosine}(\theta) = \frac{\mathbf{A} \cdot \mathbf{B}}{\Vert{}\mathbf{A}\Vert{} \Vert{}\mathbf{B}\Vert{}}$$

Where:

* $\mathbf{A} \cdot \mathbf{B}$ is the dot product of vectors $\mathbf{A}$ and $\mathbf{B}$.
* $\Vert{}\mathbf{A}\Vert{}$ and $\Vert{}\mathbf{B}\Vert{}$ are the Euclidean norms (magnitudes) of the vectors.

#### Example Sentences

* **S1:** "I took my dog for a walk"
* **S2:** "I took my cat for a walk"
* **S3:** "Today is going to rain"

#### 3x3 Cosine Similarity Matrix

|  | **S1 (Dog)** | **S2 (Cat)** | **S3 (Rain)** |
| --- | --- | --- | --- |
| **S1 (Dog)** | `1.00` | **`0.83`** | `0.12` |
| **S2 (Cat)** | **`0.83`** | `1.00` | `0.15` |
| **S3 (Rain)** | `0.12` | `0.15` | `1.00` |

Notice that **S1** and **S2** score high (`0.83`) because they share strong semantic context, while **S3** scores low against both.


### The Transformer Token Challenge

Transformer models (such as BERT) process text by returning **one embedding vector per token** rather than a single vector for the whole sentence.

For example, given the sentence *"I took my dog for a walk"*, the model outputs multiple vectors across hundreds of dimensions. To perform sentence-level comparisons, we need a single vector representation.



### Pooling Strategies

To collapse token-level vectors into a single sentence vector, we use **pooling**:

* **CLS Token Pooling:** Taking the embedding output specifically corresponding to the classification (`[CLS]`) token. 
* **Mean Pooling:** Averaging all token embeddings across the sequence. When using mean pooling, attention masks must be applied to exclude padding tokens from the calculation.

A quick comparison of different pooling strategies can be found [here](https://zilliz.com/ai-faq/how-do-i-implement-embedding-pooling-strategies-mean-max-cls).


### From Sentence to Document Embeddings

We can take the idea of single sentence embeddings (for example, by averaging token vectors using mean pooling) one step further to compare the similarity between a **question** and a **corpus of documents**.

The process works as follows:

1. **Embed the Corpus:** Take a collection of documents or passages (such as a dataset of posts from the Hugging Face forums or the SQUAD dataset) and apply the same embedding logic to every single post or passage.<br><br>
2. **Store in a Column:** This generates a new column (often called "embeddings") that stores the numerical vector representation of every passage in the dataset.

### What is Semantic Search and Why Use It?

**Semantic search** seeks to understand the *intent and contextual meaning* behind a user query rather than relying on exact keyword matching (lexical search).

#### Why It Matters:

* **Synonym & Concept Understanding:** Connects queries like "automobile repairs" to documents about "car maintenance."
* **Handling Natural Language:** Matches conceptual ideas even when the query shares zero exact words with the source document.


### Query Matching with Vector Indexes

Query matching connects a user's search prompt to indexed content through a three-step pipeline:

1. **Embed the Query:** Pass the user's incoming query string through the same embedding model used for the documents.
2. **Search the Vector Space:** Send the query vector into a fast vector index (such as **FAISS** - Facebook AI Similarity Search).
3. **Retrieve Top Matches:** Calculate vector distances to find the $k$-nearest neighbors and return the most relevant context blocks to the user.


-----

### Technical Note: FAISS Installation and GPU Adaptation on Windows

**Note on Dependencies:**
The original notebook recommends using `faiss-gpu` for vector similarity search acceleration. However, because my notebook runs in a **Windows environment using `pip**`, standard installation paths require adjustment:
1. **Lack of Official PyPI Support for GPU Binaries:** Meta (the maintainers of FAISS) does not officially publish pre-built GPU binaries (`faiss-gpu`) to PyPI due to package size limitations. Official GPU packages are restricted to the Conda ecosystem (via Anaconda/Miniconda) and are primarily optimized for Linux distributions.
2. **Environment Constraint:** Since this setup relies on a standard Python virtual environment managed via `pip` on Windows rather than Conda, installing `faiss-gpu` directly via pip fails.
3. **The Solution:** We fallback to **`faiss-cpu`** via `pip install faiss-cpu`.


*Impact:* While the vector indexing and similarity search will now execute on the CPU, your primary deep learning model (e.g., PyTorch transformer models) can—and should—remain on the GPU for embedding generation. For most standard dataset sizes, CPU-based FAISS remains exceptionally fast and eliminates complex CUDA/Conda configuration overhead on Windows.
