# Public Repository Workflow Configuration

This file contains the workflow that should be placed in the **public repository** (`ropean/me`).

## Purpose

This workflow automatically deploys the `dist/` directory to GitHub Pages whenever changes are pushed to the main branch (from your private repository's build workflow).

## Setup Instructions

1. Go to your public repository: <https://github.com/ropean/me>
2. Create the directory: `.github/workflows/`
3. Create a file: `.github/workflows/deploy.yml`
4. Copy and paste the content below into that file
5. Commit and push

## Workflow File: `deploy.yml`

Place this in `ropean/me` repository at `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  # Trigger when main branch is updated (by private repo's build workflow)
  push:
    branches: ['main']

  # Allow manual trigger
  workflow_dispatch:

# Permissions needed for GitHub Pages deployment
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment
concurrency:
  group: 'pages'
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          # Deploy the dist directory
          path: './dist'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

## How It Works

1. **Private repo** (`fetch-and-build.yml`) pushes updated `dist/` files to `ropean/me` main branch
2. **Public repo** (`deploy.yml`) detects the push and automatically deploys `dist/` to GitHub Pages
3. Your site is live at <https://ropean.github.io/me/>

## Enable GitHub Pages

After creating the workflow file, enable GitHub Pages:

1. Go to repository Settings → Pages
2. Under "Build and deployment":
   - Source: **GitHub Actions**
3. Save

That's it! The deployment will happen automatically whenever the private repository pushes updates.
