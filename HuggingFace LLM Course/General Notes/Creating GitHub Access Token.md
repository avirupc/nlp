## Creating a GitHub Access Token

For chapter 5.5, we created a GitHub Personal Access Token (PAT) so that the corresponding Jupyter Notebook can authenticate with the GitHub API when downloading the repository issues. Here are the steps for doing that:

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