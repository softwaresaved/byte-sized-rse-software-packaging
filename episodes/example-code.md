---
title: "Example Code"
teaching: 10
exercises: 0
---

:::::::::::::::::::::::::::::::::::::: questions 

- How to install dependencies and set up virtual environment for a Python project using `pip` and `venv`

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Obtain and inspect the example code used for this lesson
- Install dependencies and set up virtual environment for running and developing the code using `pip` and `venv`

::::::::::::::::::::::::::::::::::::::::::::::::

For this lesson we'll be using the [example code that does spacewalks analysis](https://github.com/softwaresaved/spacewalks_example) available on GitHub, which we'll clone onto our machines using the Bash shell.

## Obtaining Example Code

Create your own copy of the [spacewalks_example repository](https://github.com/softwaresaved/spacewalks_example) using `Use this template` button on GitHub.

Open a command-line shell (e.g. via Git Bash in Windows, bash shell on Linux or Terminal on a Mac) and navigate to where you would like the example code to reside (e.g. to your home directory).

Use Git to clone your copy of the spacewalks repository.

```bash
$ cd ~
$ git clone https://github.com/YOUR-GITHUB/spacewalks_example.git
$ cd spacewalks_example
```

## Examining the Code

Let's take a look at the spacewalk analysis code, which is in the file called `eva_data_analysis.py`.
Feel free to use your preferred editor of choice, such as Notepad, Nano or Visual Studio Code.

The code is designed to:

- Read in the data from the JSON file
- Change the data from one data format to another and saves to a file in the new format (CSV)
- Perform some calculations to generate summary statistics about the data
- Make a plot to visualise the data

A few things to note about the software project:

- `requirements.txt` file in the project root lists the software dependencies - such as Pandas, Matplotlib, Pytest, MkDocs - and describes the software environment
- `data` folder contains input data
- `results` folder contains cleaned dataset converted to CSV format and the resulting plot
- `tests` folder contains test code
- `site` folder contains documentation compiled with MkDocs


## Setting up the Environment

Before we can run the code, we need to create and activate a virtual environment called ".venv" from the root of the software project directory:

```bash
$ python3 -m venv .venv
$ source .venv/bin/activate # Mac or Linux
$ source .venv/Scripts/activate # Windows
(.venv) $
```
The active virtual environment is indicated in the command line prompt between the round brackets: "(.venv)".

Next, we will install the necessary dependencies from the `requirements.txt` file using `pip`:

```
$ python3 -m pip install -r requirements.txt
```

Note: some users may be able to just use the `python` command instead of `python3`.

To ensure the code is working correctly, run the tests using Pytest.

```
$ python3 -m pytest
```

## Running the Code

To run the analysis using the `eva_data_analysis.py` script from the command line terminal, do:

```
$ python3 eva_data_analysis.py data/eva-data.json results/eva-data.csv
```

If the code runs successfully, you should get the resulting plot in `results/cumulative_eva_graph.png`.


::::::::::::::::::::::::::::::::::::: keypoints 

- `pip` and `venv` are traditional tools for installing dependencies and setting up virtual environments for running and developing Python code.
- `requirements.txt` file can be used to record and share a snapshot of the virtual environment.

::::::::::::::::::::::::::::::::::::::::::::::::
