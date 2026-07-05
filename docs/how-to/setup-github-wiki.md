# How-To: Set Up and Initialize a GitHub Wiki

This guide describes how to initialize and configure a GitHub Wiki for your project once your remote repository is established, allowing you to synchronize your `/docs` folder contents automatically.

---

## Prerequisites
*   You must have a remote GitHub repository created (e.g., `github.com/username/moonfall`).
*   You must have push access (admin or write permissions) to the repository.

---

## Step 1: Initialize the Wiki on GitHub
GitHub Wikis are disabled or uninitialized by default.
1. Navigate to your repository homepage on **GitHub.com**.
2. Click on the **Wiki** tab in the top navigation bar.
3. Click the green button: **Create the first page**.
4. Set the title of the first page to `Home` (default) and write a brief welcome message.
5. Click **Save Page** at the bottom.
*This step is required to provision the underlying Git repository for the Wiki.*

---

## Step 2: Clone the Wiki Locally
Since the Wiki is its own repository, you can clone it to your local machine:
1. Open your terminal in WSL2/Linux.
2. Clone the Wiki repository:
   ```bash
   git clone git@github.com:username/moonfall.wiki.git
   ```
   *(Replace `username` and `moonfall` with your GitHub details)*

---

## Step 3: Populate and Structure Pages
1. Move the markdown files you want to publish from your local `docs/` folder into the newly cloned `moonfall.wiki` directory.
2. Commit and push the changes:
   ```bash
   cd moonfall.wiki
   git add .
   git commit -m "docs: Initial wiki import"
   git push origin master
   ```
3. Refresh the Wiki tab on GitHub to see the pages automatically populated.

---

## Step 4: Automate Syncing (Optional)
To avoid manually copying files, you can configure a GitHub Action to sync your `/docs` folder to the Wiki on every push to the `main` branch:

1. Create a file in your main repository: `.github/workflows/wiki-sync.yml`.
2. Add the following action configuration:
   ```yaml
   name: Sync Wiki
   on:
     push:
       branches:
         - main
       paths:
         - 'docs/**'
   jobs:
     sync-wiki:
       runs-on: ubuntu-latest
       steps:
       - name: Checkout Repository
         uses: actions/checkout@v4
       - name: Push to Wiki
         uses: Andrew-Chen-Wang/github-wiki-action@v4
         env:
           WIKI_DIR: docs
           GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
   ```
This will ensure the online Wiki stays in sync with the repository automatically.
