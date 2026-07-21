---
title: "Generating documentation with MkDocs"
teaching: 15
exercises: 45
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can we use tools like MkDocs to create documentation websites for our software?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Generate and manage comprehensive software documentation using MkDocs

::::::::::::::::::::::::::::::::::::::::::::::::

## MkDocs documentation tool

MkDocs is a static site generator specifically designed for creating documentation websites.
MkDocs can build other websites (i.e. not just documentation sites). 
It takes Markdown files as input and builds/outputs HTML files which you host anywhere, for example on GitHub Pages or any other website hosting platform.

`mkdocs` is a Python package that can be installed, for example, with `pip` and requires Python to run the `mkdocs` command.
You do not need any further knowledge of Python to build your website with MkDocs after that.

By using the MkDocs tool alone, you get a relatively vanilla and straightforward website.
There are additional plugins that build on top of MkDocs that provide additional functionalities to the built websites. 

### mkdocs-material plugin for MkDocs

"Material for MkDocs" (`mkdocs-material`) is a theme plugin for MkDocs that creates documentations site using a modern, responsive design.
In addition to changing the look of the website, "Material for MkDocs" also enhances the functionality of websites with features like blog posts, social cards, advanced search capabilities, etc.
MkDocs provides the core functionality of building a static site, while "Material for MkDocs" elevates the visual and interactive experience of your documentation.

### mkdocstrings plugin for MkDocs

If you also want to automatically generate documentation from your code's docstrings, you will need the `mkdocstrings` plugin.
This plugin can be used with MkDocs to create documentation for software projects written in a number of programming language - as long as there is a `mkdocstrings` "handler" for it.
Currently, there are `mkdocstrings` handlers for the C, Crystal, GitHub Actions, Python, MATLAB, TypeScript, and VBA languages, as well as for shell scripts.
Without this plugin, core MkDocs tool only "understands" Markdown files.

## Installing MkDocs & relevant plugins

Let's install the `mkdocs` tool, and `mkdocs-material` and `mkdocstrings` (for Python) plugins.

Within an active virtual environment of your software project, do:

```bash
python3 -m pip install mkdocs
python3 -m pip install "mkdocstrings[python]"
python3 -m pip install mkdocs-material
```

## Generating a documentation website

Let's create a new MkDocs site in the root of the spacewalks directory by running the `mkdocs new` command:

```bash
mkdocs new .    
```

```output
INFO    -  Writing config file: ./mkdocs.yml
INFO    -  Writing initial docs: ./docs/index.md
```

This command creates a bunch of files:

- `docs` folder in the current directory for our new MkDocs site (containing `index.md` file) 
- `mkdocs.yml` configuration file in the root of our project.

Now, let's fill in the `mkdocs.yml` file with the basic configuration information for our project and `mkdocs-material` theme.

```yaml
site_name: Spacewalks Documentation
use_directory_urls: false
theme:
  name: "material"
  font: false
```

Note `font: false` variable is for GDPR compliance; `use_directory_url: false` variable tells MKDocs tools how to handle URLs for documentation that is served as a website (we will cover this in a moment).

We can run the `mkdocs serve` command to see what website looks like now.

```bash
mkdocs serve
```

```output
INFO    -  Building documentation...
INFO    -  Cleaning site directory
INFO    -  Documentation built in 0.24 seconds
INFO    -  [13:59:18] Watching paths for changes: 'docs', 'mkdocs.yml'
INFO    -  [13:59:18] Serving on http://127.0.0.1:8000/
INFO    -  [13:59:32] Browser connected: http://127.0.0.1:8000/
```

This command first builds the website from the files in `docs` folder and then serves it locally at [http://127.0.0.1:8000](http://127.0.0.1:8000).
TIf you go to that URL in your browser, you can see your basic "Material for MkDocs" website.

The "built" version of our documentation site is located in the new `site` folder in the root of our project.
This is where MkDocs tool saves the HTML files compiled from Markdown documentation files. 
This directory serves as your documentation website and can be distributed together with your code.
Users of your code can then read these pages locally using a Web browser. 
Alternatively, you can host them as a website that users can navigate to.

Note that we used the setting `use_directory_urls: false` in the `mkdocs.yml` file.
This setting ensures that the documentation site is generated with URLs that are easy to navigate locally on a user's device.

An alternative to using the `mkdocs serve` command is the `mkdocs build` command - it will only build the webpages in `site` folder but not serve them as a website at [http://127.0.0.1:8000](http://127.0.0.1:8000).
You will need to re-run this command each time you make a change.
To see your documentation webpages, you have to navigate directly to the `site` folder and open files in a Web browser.

```bash
mkdocs build
````

Let's add more documentation elements to our site - for example a setup guide to help users install and run the software, how to guides for common tasks and a reference manual for developers.

```yaml
site_name: Spacewalks Documentation
use_directory_urls: false
theme:
  name: "material"
  font: false
nav:
  - Home: index.md
  - Setup Guide: setup-guide.md
  - How-To Guides: how-to-guides.md
  - Reference: reference.md
```
Let's create Markdown files `setup-guide.md`, `how-to-guides.md` and `reference.md` in `docs/` folder to match our configuration.

```bash
touch docs/setup-guide.md
touch docs/how-to-guides.md
touch docs/reference.md
```

We can add the following Markdown text to our "Setup guide":

```text
## Pre-requisites

Spacewalks was developed using Python version 3.12.

To install and run Spacewalks you will need have Python3 installed.
You will also need the following libraries (minimum versions in brackets):

- [NumPy](https://www.numpy.org/) >=2.0.0 - Spacewalk's test suite uses NumPy's statistical functions
- [Matplotlib](https://matplotlib.org/stable/index.html) >=3.0.0  - Spacewalks uses Matplotlib to make plots
- [pytest](https://docs.pytest.org/en/8.2.x/#) >=8.2.0  - Spacewalks uses Pytest for testing
- [pandas](https://pandas.pydata.org/) >= 2.2.0 - Spacewalks uses Pandas for data frame manipulation

## Installation instructions

Clone the Spacewalks repository to your local machine using Git.
If you don't have Git installed, you can download it from the official Git website.

`
git clone https://github.com/your-repository-url/spacewalks.git
cd spacewalks
`

Install the necessary dependencies:
`
python3 -m pip install -r requirements.txt
`

- To ensure everything is working correctly, run the tests using Pytest.

`
python3 -m pytest
`

## Usage Example

To run an analysis using the `eva_data_analysis.py` script from the command line terminal, do:

`
# Usage Examples
python3 eva_data_analysis.py data/eva-data.json results/eva-data.csv
`

The first argument is path to the JSON data file.
The second argument is the path the CSV output file.

If the code runs successfully, you should get the resulting plot in `results/cumulative_eva_graph.png`
```

Next, we can add the following Markdown text to our "How to guides":

```text
## How to change the file path of Spacewalk's output dataset

This guide shows you how to set the file path for Spacewalk's output data set to a location of your choice.

By default, the cleaned data set in CSV format generated by the Spacewalk software is saved to the `results/` folder within the working directory with file name `eva-data.csv`.

If you would like to modify the name or location of the output dataset, set the second command line argument to your chosen file path. 
For example, if you want to save the output data set to the subfolder `data/clean/` you can invoke the script as:

`python3 eva_data_analysis.py eva-data.json data/clean/eva-data-clean.csv`

The specified destination folder `data/clean/` must exist before running spacewalks analysis script.
```

Finally, let's replace the default Markdown content of the homepage of our site in `index.md` (which was generated by MkDocs) to include some introduction to our software and point to other documentation parts:

```text
# Welcome to Spacewalks Documentation Site

## Overview
Spacewalks is a Python analysis tool for researchers to generate visualisations and statistical summaries of NASA's extravehicular activity datasets.

## Features
Key features of Spacewalks:

- Generates a CSV table of summary statistics of extravehicular activity crew sizes
- Generates a line plot to show the cumulative duration of space walks over time

## Documentation

You can find the following documentation:

- [Setup guide](./setup-guide.html) to help you install the software and learn how to run it
- [How to guides](./how-to-guides.html) to help you perform common tasks
- [Reference manual](./reference.html) - a full API reference
```

Let's regenerate our documentation website and see what it looks like in a Web browser after these changes.

### Generating a reference manual from dosctrings 

Let's now add support for `mkdocstrings` - this will allow us to automatically add docstrings from our code into our documentation using a simple tag.

```yaml
site_name: Spacewalks Documentation
use_directory_urls: false
theme:
  name: "material"
  font: false
nav:
  - Home: index.md
  - Setup Guide: setup-guide.md
  - How-To Guides: how-to-guides.md
  - Reference: reference.md

plugins:
  - mkdocstrings
```

Let's populate our reference file `reference.md` with some preamble to include before the reference manual that will be generated from the docstrings we created.

```markdown
This file documents the key functions in the Spacewalks tool.
It is provided as a reference manual.

::: eva_data_analysis

```

Let's regenerate our documentation website and see what it looks line now.

```bash
mkdocs build
````

## Hosting documentation

We saw how MkDocs documentation can be distributed with our repository and viewed "offline".
We can also make our documentation available as a live website by deploying our documentation to a website hosting service.

As our repository is hosted in GitHub, we can use GitHub Pages - a free service built into GitHub that allows GitHub users to host websites directly from their GitHub repositories.

GitHub Pages deploys and serves site files from a branch - by default this is the `gh-pages` branch. 
GitHub Pages can also be configured to serve webpages from any other branch within the project repository. 

Let us commit our documentation to the main branch of our Git repository and push the changes to GitHub.

```bash
git add mkdocs.yml 
git add docs/
git add site/
git commit -m "Add project-level documentation"   
python3 -m pip freeze > requirements.txt
git add requirements.txt 
git commit -m "Added MkDocs tool and plugins"
git push origin main
```

::: caution

### Warning
Before we proceed to the next step, we **must** ensure that there are no uncommitted changes or untracked files in our repository.

If there are, the commands used in the upcoming steps will include them in our documentation.
:::

```bash
git status
# Add any files you want to ignore to .gitignore
# Commit .gitignore
```

To deploy your documentation, run the following command from the command line to deploy your documentation to GitHub.

```bash
mkdocs gh-deploy
```

```output
INFO    -  Cleaning site directory
INFO    -  Building documentation to directory: /Users/AnnResearch/spacewalks/site
WARNING -  griffe: eva_data_analysis.py:105: No type or annotation for returned value 'int'
WARNING -  griffe: eva_data_analysis.py:84: No type or annotation for returned value 1
WARNING -  griffe: eva_data_analysis.py:33: No type or annotation for returned value 1
INFO    -  Documentation built in 0.37 seconds
WARNING -  Version check skipped: No version specified in previous deployment.
INFO    -  Copying '/Users/AnnResearcher/spacewalks/site' to 'gh-pages' branch and pushing to
           GitHub.
Enumerating objects: 63, done.
Counting objects: 100% (63/63), done.
Delta compression using up to 11 threads
Compressing objects: 100% (60/60), done.
Writing objects: 100% (63/63), 578.91 KiB | 7.93 MiB/s, done.
Total 63 (delta 7), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (7/7), done.
remote: 
remote: Create a pull request for 'gh-pages' on GitHub by visiting:
remote:      https://github.com/kkh451/spacewalks/pull/new/gh-pages
remote: 
To https://github.com/kkh451/spacewalks-dev.git
 * [new branch]      gh-pages -> gh-pages
INFO    -  Your documentation should shortly be available at: https://kkh451.github.io/spacewalks/
```

This command will build our documentation with MkDocs, then commit and push the files to the `gh-pages` branch using the `ghp-import` tool, which is installed together with MkDocs.

For more options, use:

```bash
mkdocs gh-deploy --help
```

Notice that the deploy command did not allow us to preview the site before it was pushed to GitHub.
So, it is a good idea to build site and check it locally with the build/serve commands before deploying.

If you navigate to your software repository on GitHub, you may notice the new branch called `gh-pages` had been created by the `mkdocs gh-deploy` command and populated with the contents of your `site` folder.

If you navigate to "Settings/Pages" portion of your software repository, you may notice that GitHub Pages has already been configured to serve a website from your `gh-pages` branch.
You can visit the live site by following the URL that GitHub Pages creates for your repository.

If it has not worked as described, on the "Settings/Pages" page under "Build and deployment" section you can configure the repository branch (should be `gh-pages`) that the GitHub Pages site is built from.

## Summary

We have explored the static website generator tool MkDocs for generating documentation website for our software project.
`mkdocs-material` plugin for MkDocs creates richer and more responsive websites, while `mkdocstrings` plugin creates reference manuals from docstrings in our code.

Generated documentation pages can be shared with our software - they can reside in our software repository and be used by end users offline or we can use website hosting platforms such as GitHub to host documentation websites.

::: keypoints

- Static site generators (such as MkDocs) can help use generate documentation websites from Markdown files or docstrings.
- GitHub Pages provide a free webpage hosting service for your documentation website.

:::