## Creating a HuggingFace Access Token

For chapter 5.5 of this course, we needed a HuggingFace access token so that the Jupyter Notebook can authenticate with the HuggingFace Hub. Here are the steps to create an access token:

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