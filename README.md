# gh-deployment-workflow

Source: https://roadmap.sh/projects/github-actions-deployment-workflow

The goal of this project is to help you learn the notion of continuous integration and continuous deployment. You will write a simple GitHub Actions workflow to deploy a static website to GitHub Pages.

## Requirements

You are required to write a GitHub action that deploys any changes made to the index.html file to GitHub Pages. It should only deploy the file when the index.html file is changed.

## Steps to Get Started

1. Create a GitHub repository for the project called `gh-deployment-workflow`

2. Repository should contain a simple `index.html` file saying "Hello, GitHub Actions!"

3. It should also have a `README.md` file explaining the project

4. There should also be a `deploy.yml` file in the `.github/workflows` directory which contains the GitHub Actions workflow to deploy the website to GitHub Pages

5. Every push to the main branch that changes the index.html file should trigger the workflow to run and deploy the website to GitHub Pages

6. Website and any changes you make should be accessible at the GitHub pages URL for the repository e.g. `https://<username>.github.io/gh-deployment-workflow/`

## Stretch Goal

You can also make this project more practical e.g. use some sort of a static site generator such as Hugo, Jekyll, Astro or similar generator to create a more complex website e.g. your own personal portfolio.

## What You'll Learn

After finishing this project, you will have a good understanding of the following concepts:

- GitHub Actions
- GitHub Pages
- Continuous Integration and Continuous Deployment
- Writing GitHub Actions workflows

Continue solving more projects for advanced CI/CD concepts.
