# Pages

> Here we explain GitHub pages, a way to easily publish your work as a website. 

## What it is
GitHub (but also GitLab, if it is enabled) allows you to make use of **pages**. This is a feature that let's you create a website hosted on GitHub. You can create .html files, or even have markdown files converted to a website using Jupyter Book. 

To do this automatically, you can make use of GitHub Actions, or in GitLab a CI/CD script. In both cases, a Linux-based runner  is used to perform some actions (like installing software, converting from markdown to html, and creating an artifact containing the generated HTML files). More on the _actions_ below. 

Your website is usually hosted using the following url: https://<username>.github.io/<repositoryname>. It is possible to connect GitLab/GitHub to an external server - but that is beyond our scope.


## GitHub action for JB
As said, we can make use of GitHub actions to build our website.

````{tab-set}
```{tab-item} pixi

```
```{tab-item} pip
name: MyST GitHub Pages Deploy
on:
  push:
    branches: [main]

env:
  BASE_URL: /${{ github.event.repository.name }}

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v7

      - name: Setup Pages
        uses: actions/configure-pages@v5

      - name: Setup Node
        uses: actions/setup-node@v5
        with:
          node-version: 24

      - name: Install MyST
        run: npm install -g mystmd

      - name: Install Python dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
         
      - name: Build HTML Assets
        run: myst build --html 

      - name: Upload HTML artifact
        uses: actions/upload-pages-artifact@v5
        with:
          path: ./_build/html

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v5
```
````



- instructions



- What it is
- Buildscript
- How to enable that script

## Instructions for repo owner
1. Go to the [repo](https://github.com/Contemporary-Physicslab/gitintro)
1. Click the green button `Use this template` and choose `Create a new repository`
1. Choose your repository name wisely - this will also become part of the URL! Don't change any of the other settings and click `Create repository`.

You repository will now be created using the template repo. 

Before your website can be seen, we will need to enable GitHub pages.

1. Go to `Settings` (top mid of the screen) and click `Pages`. From the dropdown menu under `Build and Development` choose `GitHub Actions` as source. 
1. Click on `Code` in the top left corner and click on ⚙ (the `gear-icon` near **About**) at the right site of the page. Check the box `Use your GitHub Pages` website.




## Inviting your partner
For IP2, you work in pairs. You can invite your partner(s) to collaborate on your repository. To do this, go to your new repository on GitHub and follow the steps below.

1. Go to your new repository on GitHub.

1. Click on **Settings** (in the top right corner of your screen) and click on **Collaborators** in the left-hand menu.

1. Click the **Add people** button under the "Manage access" heading.

1. Type your partner's (or partners') username and click on **Add <username> to this repository** (where `<username>` is your partner's username).

## Partner accepts invitation
Your partner will receive an email with an invitation to collaborate on the repository. Once accepted, both of you can make changes to the repository.

Your partner follows steps X through X, but uses the URL of **your** repository.