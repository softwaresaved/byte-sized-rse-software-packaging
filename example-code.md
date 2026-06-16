---
title: "Example Code"
teaching: 10
exercises: 0
---

:::::::::::::::::::::::::::::::::::::: questions 

- What is a minimum documentation needed for people to be able to reuse some else's code?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Obtain and run example code used for this lesson
- List documentation types missing from the example code

::::::::::::::::::::::::::::::::::::::::::::::::

For this lesson we'll be using some [example code that does spacewalks analysis](https://github.com/softwaresaved/spacewalks) available on GitHub, which we'll clone onto our machines using the Bash shell.
The spacewalks code is available from:

`https://github.com/softwaresaved/spacewalks`.

## Obtaining Example Code

Create your own copy of the spacewalks repository above using `Use this template` button on GitHub.

Open a command-line shell (e.g. via Git Bash in Windows, bash shell on Linux or Terminal on a Mac) and navigate to where you would like the example code to reside (e.g. to your home directory).

Use Git to clone your copy of the spacewalks repository.

```bash
cd
git clone https://github.com/your-repository/spacewalks.git
cd spacewalks
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

- `eva_data_analysis.py` Python script already contains comprehensive docstrings comments
- `requirements.txt` file in the project root lists the software dependencies - such as Pandas, Pytest, Matplotlib
- `data` folder contains input data
- `results` folder contains cleaned dataset converted to CSV format and the resulting plot
- `tests` folder contains test code


## Running the Example Code

Let's run the code.

First, we will create and activate a virtual environment called "venv" from the root of the software project directory:

```bash
python3 -m venv venv
source venv/bin/activate # Mac or Linux
source venv_spacewalks/Scripts/activate # Windows
(venv) $
```
The active virtual environment is indicated in the command line prompt between the round brackets: "(venv)".

Next, we will install the necessary dependencies from the `requirements.txt` file using `pip`:

```
$ python3 -m pip install -r requirements.txt
```

Note: some users may be able to just use the `python` command instead of `python3`.

To ensure the code is working correctly, run the tests using Pytest.

```
python3 -m pytest
```

To run the analysis using the `eva_data_analysis.py` script from the command line terminal, do:

```
python3 eva_data_analysis.py data/eva-data.json results/eva-data.csv
```

If the code runs successfully, you should get the resulting plot in `results/cumulative_eva_graph.png`.


::: challenge

## What documentation for this software is missing?

::: hint
 
What documentation would you need to be able to run the code?

:::

:::

With the code provided as is (e.g. someone sent you the code via email or on a memory stick), would you be able to answer the following questions:

- Could you run the code on your platform/operating system (is there documentation that covers installation instructions)?
- What programs or libraries do you need to install to make it work (and which versions)?
- Are you allowed to use this code in your own work? If you did, would the owner expect credit in some form (paper authorship, citation or acknowledgement)?
- Are you allowed to modify the files or share them with others?
- How easy would it be to change its parameters to calculate a different statistic, or run the analysis on a different input file?


::::::::::::::::::::::::::::::::::::: keypoints 

- Minimum information needed to be able to run someone else code should include the reuse licence, description and purpose of the code, installation and setup instructions (including dependencies), and usage example (how to run the code).
- Additional information may include more detailed installation guides, API documentation or design documents, list of authors and how to cite the code (this list is not exhaustive).

::::::::::::::::::::::::::::::::::::::::::::::::
