# 5.5  Creating your own dataset

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter5/5)

▶ Video links:
[Uploading a dataset to the hub](https://youtu.be/HaN6qCr_Afc)


## 🗒 Section Notes

Sometimes the dataset that you need to build an NLP application doesn't exist, so you'll need to create it yourself. In this section we'll show you how to create a corpus of [GitHub issues](https://github.com/features/issues/), which are commonly used to track bugs or features in GitHub repositories. This corpus could be used for various purposes, including:

* Exploring how long it takes to close open issues or pull requests
* Training a _multilabel classifier_ that can tag issues with metadata based on the issue's description (e.g., "bug," "enhancement," or "question")
* Creating a semantic search engine to find which issues match a user's query

Here we'll focus on creating the corpus, and in the next section we'll tackle the semantic search application. To keep things meta, we'll use the GitHub issues associated with a popular open source project: 🤗 Datasets! Let's take a look at how to get the data and explore the information contained in these issues.

This chapter has following sections:
1. Getting the data
2. Cleaning up the data
3. Augmenting the dataset
4. Uploading the dataset to the Hugging Face Hub
5.  Creating a dataset card

Details and code for these sections will be found in the [chapter](https://huggingface.co/learn/llm-course/chapter5/5) and in this [notebook](Creating_your_own_dataset.ipynb).


## ℹ️ Creating a HuggingFace Access Token

For this chapter, we need a HuggingFace access token so that the Jupyter Notebook can authenticate with the HuggingFace Hub. Here are the steps to create an access token:

### 1. Create or sign in to your HuggingFace account
Go to [HuggingFace](https://huggingface.co) and sign in to your account. If you don't have an account, create one first.

### 2. Open Access Tokens

After signing in:
1. Click your profile picture in the top-right corner.
2. Select **Settings**.
3. In the left-hand menu, select **Access Tokens**.
4. Click **+ Create new token**.
3. Create a token for this chapter

### 3. Give the token a descriptive name, for example: `llm-course-chapter-5`

For this chapter, select the permissions that allow the notebook to write to the HuggingFace Hub. A **Write** token is appropriate because later in the chapter we will upload/push our dataset to the Hub.

> ⚠️ Important: Treat your access token like a password. Do not commit it to GitHub, put it in a public notebook, or share it with anyone.

### 4. Copy the token

After creating the token, HuggingFace will display the token.

**Copy it immediately** and store it somewhere secure. You will not be able to view the complete token again after leaving the page.

It will look something like: `hf_xxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

### 5. Authenticate from the Jupyter Notebook

The course uses:

```Python
from huggingface_hub import notebook_login
notebook_login()
```


Run this cell. When prompted, paste the HuggingFace access token you created above.

You do not enter your HuggingFace account password here. The notebook authentication uses the access token.

### ⚠️Security reminder

**NEVER** write your real token directly into a notebook that you intend to publish. In particular, avoid anything like:

```Python
HF_TOKEN = "hf_actual_token_here"
```


if the notebook will be committed to a public GitHub repository.

If a token is accidentally exposed, revoke it immediately from your HuggingFace account and create a new one.


## ℹ️ Creating a GitHub Access Token

For this chapter, we need a GitHub Personal Access Token (PAT) so that the Jupyter Notebook can authenticate with the GitHub API when downloading the repository issues.

GitHub recommends using a **fine-grained personal access token** instead of a classic token whenever possible.


### 1. Open GitHub Settings

Go to [GitHub](https://github.com) and sign in to your account.

Click your profile picture in the top-right corner and select:

**Settings → Developer settings → Personal access tokens → Fine-grained tokens**

Then click **Generate new token**.


### 2. Give the token a name

Under **Token name**, enter a descriptive name, for example: `llm-course-chapter-5`


### 3. Set an expiration date

Choose an expiration period appropriate for your use.

For a learning project, it is recommended to use a limited expiration rather than keeping the token indefinitely. GitHub recommends limiting credentials to the minimum amount of time needed. I created a token for 3 months.

### 4. Select repository access

For this chapter, we are accessing the public huggingface/datasets repository. Select: ```Public repositories (read-only)```

We do not need write access to the Hugging Face repository because we are only reading issues from GitHub.

### 5. Generate the token

Review the settings and click Generate token. 

> I did not select anything in the Permissions section as I was not sure and there were many options. Anyway, it seemed to be optional.

GitHub will display your new token. It will typically begin with: `github_pat_`.......

After creating the token, **copy it immediately** and store it somewhere secure. You will not be able to view the complete token again after leaving the page.

### 6. Use the token in the Jupyter Notebook

The token can then be assigned to a variable:

```Python
GITHUB_TOKEN = "github_pat_xxxxxxxxxxxxxxxxxxxx"

headers = {
    "Authorization": f"Bearer {GITHUB_TOKEN}"
}
```
The quotation marks (" ") around the token are required because the token is a Python string.

### ⚠️ Security

Treat your GitHub access token like a password. Never commit the actual token to GitHub or include it in a publicly shared notebook. GitHub recommends using the minimum permissions necessary and storing credentials securely.
If a token is accidentally exposed, revoke it and create a new one.