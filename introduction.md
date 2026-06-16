---
title: "Introduction"
teaching: 30
exercises: 0
---

:::::::::::::::::::::::::::::::::::::: questions 

- Why is documenting software important?
- How should we document our code?
- What are the minimum elements of software documentation needed?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Describe the main types of software documentation and identify their primary audiences, including end users, developers, maintainers, contributors and system administrators.

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

Many researchers write code, install software libraries and share their scripts with collaborators. 
However, fewer people understand how packages are created, how dependencies are managed or how to make software easy for others to install and reuse.

Understanding packaging helps us move from writing code that works on our machine to developing software that others can reliably install, use, reproduce and contribute to.

In this session we will look into what software packaging entails and why should we care about packaging.
In the practical part of this session, we will explore modern Python packaging and package management using `uv`, a fast Python package manager and project management tool that is rapidly becoming popular across the Python community.

## What is a software package?

A package is more than just code - in usually contains:

* Source code
* Project metadata
* Dependency information
* Licensing information
* Documentation
* Tests
* Build instructions

Packages provide a way of bundling these components into an easy to transport form ("archive") that can be more easily distributed and installed.

Packages are used in almost all programming languages, operating systems and computing environments to simplify the process of installing and managing software.


## Why packaging software matters?

Let's start with a familiar situation. 
Imagine you have written a useful analysis script in Python and a colleague wants to use it.
You send them the code and they immediately encounter problems:

* Which version of Python should they use?
* Which libraries need to be installed?
* Which versions of those libraries are compatible?
* How can they reproduce your environment?

This is where packaging becomes important.
It provides a standard way to describe:

* What software is being distributed
* What dependencies it requires
* How it should be installed
* Which versions are supported


Packaging provides a number of important benefits.

Packaging makes software distribution more efficient as it promotes a modular approach to software development and code reuse - reducing duplication.
Applications and libraries typically include only the code that is specific to them, while shared external dependencies (package) can be installed separately and reused across multiple projects.
By not including copies of all the external code it uses - software distribution becomes more efficient.

Packaging helps ensure that software can be shared, reused and reproduced by others. 
Packaging captures important information about dependencies, versions and installation requirements, making it easier for collaborators to run your software and obtain consistent results. 
Good packaging practices also support software sustainability by making projects easier to maintain, distribute and build upon over time.


## What's inside a Python package?

A Python package contains much more than code.

Typical contents include:

- The application or library code.
- Metadata describing the package.
- Dependency information.
- A licence.
- Documentation such as a README.
- Optional assets such as data files, images, or compiled extensions.

Packaging helps bring all of these components together into a shareable and installable unit.

## Python Packaging Ecosystem

Packaging tools help with preparing metadata files, building packages and uploading them to a package repository.

### Traditional packaging tools

Historically, many researchers created Python packages using a combination of the following tools.

#### `setup.py`

For many years, `setup.py` file was the primary mechanism for defining how a Python package should be built and installed. 
It contains information about the package, including its name, version, dependencies and build instructions.

While still supported by many projects, modern Python packaging has moved away from relying directly on `setup.py` in favour of standardised configuration files.

#### `setuptools`

`setuptools` is one of the oldest and most widely used Python packaging libraries. 
It extends Python's original packaging capabilities and provides functionality for building, packaging and distributing software.

Many modern packaging tools still rely on `setuptools` behind the scenes, even when developers no longer interact with it directly.

#### `pip`

`pip` is Python's traditional package installer. 
It allows users to install packages from package repositories such as PyPI, as well as from local directories, archives or source code repositories such as GitHub.

While `pip` remains the standard installer included with Python, newer tools are increasingly combining package installation, environment management and dependency resolution into a single workflow.

#### `requirements.txt`

A `requirements.txt` file provides a simple way to record project dependencies. 
It is commonly used to specify the packages and versions needed to recreate a software environment.

Although still widely used, many modern projects now manage dependencies directly through `pyproject.toml` and lock files.

### Modern packaging tools

Python packaging has evolved significantly over time.

### `pyproject.toml`

The introduction of `pyproject.toml` file (written in [TOML format][toml]) has been one of the most significant developments in Python packaging.
It provides a standard location for:

* Project metadata
* Dependencies
* Build configuration
* Tool-specific settings

Instead of maintaining information across multiple files, projects can now define most packaging-related configuration in a single location.
This standardisation has simplified Python packaging considerably.

Modern Python projects increasingly adopt the following tools that manage packages described with `pyproject.toml`.

### `uv`

Uv is a modern package and project management tool developed by Astral. 
It combines dependency management, virtual environment management and package installation into a single, high-performance tool.

Many common tasks that previously required several separate tools can now be performed through a single command-line interface.
We will use `uv` tool in the practical part of this session.

### Poetry

Poetry focuses on dependency management, reproducible environments and simplified package publishing. 
It introduced many of the ideas that influenced modern Python project management and remains popular in both industry and research.

### Hatch

Hatch provides a flexible framework for managing Python projects, environments and package builds. 
It is particularly popular among developers who require more advanced build and release workflows.

## Managing dependencies

Dependencies are one of the biggest challenges in software development.
Research software often relies on dozens or hundreds of packages.
Managing these manually quickly becomes difficult.
Modern package managers help by:

* Tracking dependencies
* Resolving version conflicts
* Creating reproducible environments
* Recording exact package versions

With `uv`, dependencies are declared in `pyproject.toml`.
`uv` automatically resolves compatible versions and records them in a "lock" file.
The lock file ensures that collaborators can recreate the same software environment later.
This is an important contribution to reproducibility.

## Packaging and distribution formats

When discussing Python packaging, it is important to distinguish between source code, packages and distributions.

### Source distribution (sdist)

A source distribution contains the original source code and all the files needed to build the package.
Source distributions are typically distributed as compressed archives such as:

```text
my_package-1.0.0.tar.gz
```

Installing from a source distribution requires the package to be built on the user's machine. 
This provides maximum flexibility but may require additional build tools or compilers.

### Wheel

A wheel is a built distribution format designed for fast installation.
Wheel files typically have names such as:

```text
my_package-1.0.0-py3-none-any.whl
```

Unlike source distributions, wheels can usually be installed directly without rebuilding the package. 
This makes installation faster and more reliable.

Most users install wheel packages without realising it because package managers automatically select a compatible wheel when one is available.

### Package repositories

Once packages have been built, they are typically published to a package repository.
The most widely used repository for Python packages is the Python Package Index (PyPI), which hosts hundreds of thousands of open-source packages.
Package repositories provide a central location where users can discover, download and install software.




::: keypoints

- Documentation allows users to run and understand software without having to work things out for themselves directly from the source code.
- Different audiences (e.g. end users, developers, administrators) interact with our software in different ways and require different types of documentation.
- Documentation can be provided at different levels 
  - code-level documentation embedded within the source code to understand the implementation details,
  - software-level documentation on how to install, use, configure and modify the code, 
  - project-level documentation on how to contribute to, maintain and govern the software project.
- A (good) README, CITATION and LICENSE files are the minimum project-level documentation elements required to make research code understandable and reusable (and research it supports reproducible).
- Documentation frameworks such as Diátaxis provide content and style guidelines that can helps us write high quality documentation.

:::