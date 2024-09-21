# Push to the GitHub Pages branch gh-pages
This GitHub Workflow Action will automate the process of deploying files to
GitHub Pages. The action will commit and push a selection of files to the
GitHub Pages branch `gh-pages` using a commit message to reference back to the
original commit SHA.

## Inputs

### source-pattern
Used by `cp -r` as the source for what to push to branch `gh-pages`. If you omit
this input the default is used.

    description: 'The pattern used to match what to push to gh-pages'
    required: true
    default: 'dist/*'

## How to use
### 1. Prepare your application to run from GitHub pages
Make sure your application can run from GitHub pages with an URL like:
`https://<github_username>.github.io/<app_repo>`.

### 2. Prepare the branch gh-pages
Create an empty branch and push it to `gh-pages`.

```
git checkout --orphan gh-pages
git rm -rf .
touch index.html
git add index.html
git commit -m "Add empty index.html"
```

### 3. Setup GitHub Pages
Enable GitHub Pages from the repository's settings by selecting the branch
`gh-pages` as your source branch for GitHub Pages under `Settings -> Pages`.

### 4. Create a GitHub Workflow Action
Add this action inside your repository's Actions

```
name: Build and Deploy

on:
  push:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4
      with: # To also get the gh-pages branch
        fetch-depth: 0

    # Build stuff ex.
    - name: Use Node.js 22.x
      uses: actions/setup-node@v4
      with:
        node-version: 22.x
        cache: 'npm'
    - run: npm ci
    - run: npm run build

    # Deploy to github pages
    - name: Build and Deploy React app to GitHub Pages
      uses: angelstam/github-push-to-gh-pages@v1
      # with:
      #   source-pattern: 'dist/*' # This is the default pattern
```

## License
[![GitHub license](https://img.shields.io/github/license/angelstam/github-push-to-gh-pages?style=for-the-badge)](https://github.com/angelstam/github-push-to-gh-pages/blob/main/LICENSE)

Copyright (c) 2024 Johan Angelstam
