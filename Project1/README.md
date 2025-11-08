
# Project 1: Sentiment Analysis of IMDB Movie Reviews

This is my first project in my NLP learning path. This document provides a brief overview of the tasks I have performed. More details on the code and the concepts can be found in the markdown cells of the notebook.

## Project Overview

This project implements a **sentiment classifier** for IMDB movie reviews.  
The goal is to predict whether a review is **positive** or **negative** using natural language processing (NLP) techniques and machine learning models.

We explore **multiple feature representation approaches** — from classical frequency-based methods to word embeddings — and compare their performance.

---

## Dataset

We use the **IMDB Dataset of 50K Movie Reviews**:

- 25,000 positive reviews
- 25,000 negative reviews
- Each review is labeled as `positive` or `negative`

**Source:** [Kaggle - IMDB Dataset of 50K Movie Reviews](https://www.kaggle.com/datasets/lakshmi25npathi/imdb-dataset-of-50k-movie-reviews)

---

## Project Workflow

The project is organized into **four main stages**:

### 1. Data Loading & Exploration

- Load the dataset using `pandas`
- Check for missing values
- Explore the distribution of sentiment labels
- Visualize class balance

### 2. Text Preprocessing & TF-IDF Features

- **Text Cleaning:** Remove HTML tags, punctuation, numbers, lowercase conversion
- **Tokenization:** Split reviews into words
- **Stopword Removal:** Remove common words like `the`, `is`, `and`
- **TF-IDF Feature Extraction:**  
  - Converts text into numerical vectors based on **Term Frequency–Inverse Document Frequency**  
  - Highlights words that are frequent in a review but rare across the dataset  
  - Supports **unigrams and bigrams**  
  - Generates sparse matrices as input to ML models

### 3. Classical ML Models

- **Logistic Regression** and **Linear SVM** trained on TF-IDF vectors
- Evaluated using **accuracy, precision, recall, F1-score, and confusion matrices**

### 4. Word2Vec Embeddings

#### a. Self-Trained Word2Vec

- Train Word2Vec model on the IMDB corpus
- Represents each word as a **dense vector** capturing semantic meaning
- Each review is represented by the **average of its word vectors**


#### b. Pre-Trained Word2Vec (GoogleNews)

- Use embeddings trained on **~100 billion words from Google News**
- Rich semantic knowledge, but less domain-specific for IMDB reviews
- Represent reviews by averaging vectors of known words


**Word2Vec Concepts:**

- **Skip-Gram:** Predicts context words from a target word (used in this project)
- **Context Window:** Number of surrounding words considered
- Captures semantic similarity: words used in similar contexts have closer vectors

---

## Key Observations

- **TF-IDF** is simple, fast, and performs well for sentiment classification on medium-sized datasets.
- **Self-trained Word2Vec** embeddings often outperform generic pre-trained embeddings on domain-specific datasets.
- Pre-trained embeddings are useful when:
  - Your dataset is small
  - You want to leverage general semantic knowledge