---
title: "How To Create This Site"
date: 2021-04-13T14:09:28+05:30
draft: false
---

---


### Step 1: Install Git and Hugo

    brew install git hugo

To verify your install

    git version
    hugo version

### Step 2: Create a Github User Site

Go to [https://github.com/new](https://github.com/new)

![Github User Site](/img/github-new-user-site.png)

### Step 3: Clone the repository

    git clone https://github.com/LooseImpala/looseimpala.github.io.git
    cd looseimpala.github.io

:anchor: Replace the username with yours.


### Step 3: Create a New Hugo Site

    hugo new site . --force

The above will create a new Hugo site in the user site folder

### Step 4: Add a theme

    git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod --depth=1
    git submodule update --init --recursive # needed when you reclone your repo (submodules may not get cloned automatically)
    echo theme = \"PaperMod\" >> config.toml

:anchor: For our website, we're using the [PaperMod theme](https://github.com/adityatelange/hugo-PaperMod)

### Step 5: Add Some Content

    hugo new posts/my-first-post.md

### Step 6: Add, Commit and Push your changes

    git add --all
    git commit -m "Basic Hugo Site"
    git push origin master


### Step 7: Create a Github Token

Go to [https://github.com/settings/tokens/new](https://github.com/settings/tokens/new)

![Github Token](/img/github-new-token.png)

Add token as Secret in Github in /settings/secrets

![Github Secret](/img/github-secret.png)

### Step 8: Create a local branch gh-pages and push

    git checkout -b gh-pages
    git push -u origin gh-pages

### Step 9: Create a file in .github/workflows/gh-pages.yml

Go back to master branch

    git checkout master

Create file .github/workflows/gh-pages.yml

    name: github pages
    on: push
    jobs:
      deploy:
        runs-on: ubuntu-18.04
        steps:
          - name: Git checkout
            uses: actions/checkout@v2

          - name: Update theme
            # (Optional)If you have the theme added as submodule, you can pull it and use the most updated version
            run: git submodule update --init --recursive

          - name: Setup Hugo
            uses: peaceiris/actions-hugo@v2
            with:
              hugo-version: 'latest'
              extended: true

          - name: Build
            # remove --minify tag if you do not need it
            # docs: https://gohugo.io/hugo-pipes/minification/
            run: hugo --minify

          - name: Deploy
            uses: peaceiris/actions-gh-pages@v3
            with:
              personal_token: ${{ secrets.ACTIONS_DEPLOY_KEY }}
              publish_dir: ./public
              publish_branch: gh-pages

Commit and push changes
   git push origin master


### Step 10: Change deploy branch in github pages settings

Go to settings/pages


![Github Pages Deploy Branch](/img/github-pages-deploy-branch.png)


### Step 7: Run Server

    hugo server -D
