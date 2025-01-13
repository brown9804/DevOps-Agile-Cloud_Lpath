# GitHub Pages -Overview 

Costa Rica

[![GitHub](https://img.shields.io/badge/--181717?logo=github&logoColor=ffffff)](https://github.com/) [brown9804](https://github.com/brown9804)

Last updated: 2025-01-13

----------------------

> **GitHub Pages** is a feature provided by GitHub that allows you to `host static websites directly from a GitHub repository`. It's a great way to showcase your projects, create personal websites, or host documentation for your repositories. 

## Wiki 

<details>
<summary><b>Table of Wiki</b> (Click to expand)</summary>

- [Websites for you and your projects](https://pages.github.com/)
- [Essentials of automated application deployment with GitHub Actions and GitHub Pages](https://resources.github.com/learn/pathways/automation/essentials/automated-application-deployment-with-github-actions-and-pages/)

</details>

## Content 

<details>
<summary><b>Table of Content</b> (Click to expand)</summary>

- [Wiki](#wiki)
- [Content](#content)
- [How is GitHub Pages Used?](#how-is-github-pages-used)
- [Automate the process of converting Markdown to static HTML and deploying it using GitHub Pages and GitHub Actions](#automate-the-process-of-converting-markdown-to-static-html-and-deploying-it-using-github-pages-and-github-actions)
- [Setting Up GitHub Pages](#setting-up-github-pages)

</details>

> What is GitHub Pages? <br/>
> GitHub Pages is a `free service that turns your GitHub repositories into websites`. You can host HTML, CSS, and JavaScript files, and it’s perfect for static websites that `don’t require server-side processing`. GitHub Pages supports custom domains, making it easy to create a professional-looking website.

## How is GitHub Pages Used?
- **Personal Websites**: Showcase your portfolio, resume, or blog.
- **Project Documentation**: Host documentation for your open-source projects.
- **Organization Sites**: Create websites for organizations or communities.
- **Demo Sites**: Share live demos of your projects.

## Automate the process of converting Markdown to static HTML and deploying it using GitHub Pages and GitHub Actions

1. Create a GitHub Repository
      - Go to GitHub and create a new repository. Name it `username.github.io`, where `username` is your GitHub username.
      - Make sure the repository is public.
2. Add Your Markdown Files
      - Clone the repository to your local machine.
      - Add your Markdown files to the repository.
      - Commit and push the changes to GitHub.
3. Create a GitHub Actions Workflow
      - In your repository, create a `.github/workflows` directory.
      - Inside this directory, create a file named `md-html-deploy.yml`.
4. Define the Workflow: Add the following content to the `md-html-deploy.yml` file to set up a workflow that converts Markdown to HTML and deploys it to the `main` branch:
      1. **Checkout Repository**: This step checks out your repository so that the workflow can access the files.
      2. **Set up Node.js**: This step sets up Node.js, which is required for some Markdown converters.
      3. **Install Dependencies**: This step installs the necessary dependencies for your project.
      4. **Convert Markdown to HTML**: This step uses `pandoc` to convert Markdown files to HTML and places them in the `_site` directory.
      5. **Deploy to GitHub Pages**: This step commits the generated HTML files back to the `main` branch and pushes the changes. This ensures that your GitHub Pages site is updated with the latest HTML files.
      
      ```yaml
      name: Workflow to convert Markdown files to HTML and deploy to GitHub Pages
      
      on:
        push:
          branches:
            - main  # Trigger the workflow on push to the main branch
            - dev   # Trigger the workflow on push to the dev branch
            - feature/*  # Trigger the workflow on push to any feature branch
      
      jobs:
        build-and-deploy:
          runs-on: ubuntu-latest  # Use the latest Ubuntu environment
      
          steps:
            # Step 1: Checkout the repository
            - name: Checkout repository
              uses: actions/checkout@v2
      
            # Step 2: Set up Node.js environment
            - name: Set up Node.js
              uses: actions/setup-node@v2
              with:
                node-version: '14'  # Specify the Node.js version
      
            # Step 3: Install npm dependencies
            - name: Install dependencies
              run: npm install
              
            # Step 4: Install pandoc for Markdown to HTML conversion
            - name: Install pandoc
              run: sudo apt-get install -y pandoc
      
            # Step 5: Convert all Markdown files to HTML while preserving directory structure
            - name: Convert Markdown to HTML
              run: |
                mkdir -p _site  # Create the _site directory if it doesn't exist
                # Find all Markdown files and convert them to HTML, preserving directory structure
                find . -name "*.md" -type f | while read file; do
                  # Create the corresponding directory in _site
                  mkdir -p "_site/$(dirname "$file")"
                  # Convert the Markdown file to HTML and save it in the corresponding directory
                  pandoc "$file" --standalone --toc -o "_site/${file%.md}.html"
                done
      
            # Step 6: Deploy the generated HTML files to GitHub Pages
            - name: Deploy to GitHub Pages
              run: |
                # Configure Git with a bot email and name
                git config --global user.email "github-actions[bot]@users.noreply.github.com"
                git config --global user.name "github-actions[bot]"
                # Check for local changes and stash them if present
                if [ -n "$(git status --porcelain)" ]; then
                  git stash
                  git pull origin ${{ github.ref }} --rebase
                  git stash pop || true  # Ignore error if no stash entries to pop
                else
                  git pull origin ${{ github.ref }} --rebase
                fi
                # Add all changes, including untracked files
                git add -A
                # Commit the changes
                git commit -m 'Deploy static HTML files'
                # Push the changes to the remote branch
                git push origin HEAD:${{ github.ref }}
      ```

## Setting Up GitHub Pages
1. **Create a Repository**: Create a new repository on GitHub or use an existing one.
2. **Enable GitHub Pages**:
   - Go to the repository settings on GitHub.
   - Under the `GitHub Pages` section, select the `main` branch as the source.
  
     <img width="550" alt="image" src="https://github.com/user-attachments/assets/6143fc94-5e77-45c5-8680-4f269b0dc242" />

    > Static HTML refers to web pages that are delivered to the user's browser exactly as stored, without any server-side processing. Static sites are fast, secure, and easy to deploy, making them ideal for simple websites, portfolios, blogs, and documentation.
3. Push Your Code: Commit and push your code to the main branch. The GitHub Actions workflow will automatically run and deploy your site to GitHub Pages.

<div align="center">
  <h3 style="color: #4CAF50;">Total Visitors</h3>
  <img src="https://profile-counter.glitch.me/brown9804/count.svg" alt="Visitor Count" style="border: 2px solid #4CAF50; border-radius: 5px; padding: 5px;"/>
</div>