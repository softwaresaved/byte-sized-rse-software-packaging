---
title: "Introduction"
teaching: 30
exercises: 0
---

:::::::::::::::::::::::::::::::::::::: questions 

- What is software packaging and why should researchers care about it?
- How does packaging make software easier to share, install and maintain?
- What information is typically included in a software package?
- How are software dependencies managed in modern Python projects?
- What is the difference between a source distribution and a wheel?
- How are Python packages published and distributed?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain what software packaging is and why it is important for software reuse, reproducibility and sustainability.
- Describe the key components of a Python package and the role of package metadata, dependencies and documentation.
- Distinguish between traditional and modern Python packaging tools and standards, including `setup.py`, `pip`, `pyproject.toml` and `uv`.
- Explain how package managers help manage dependencies and create reproducible software environments.
- Describe the difference between source distributions (sdists) and wheels.

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

Many researchers write code, install software libraries and share their scripts with collaborators. 
However, fewer people understand how packages are created, how dependencies are managed or how to make software easy for others to install and reuse.

Understanding packaging helps us move from writing code that works on our machine to developing software that others can reliably install, use, reproduce and contribute to.

In this session we will look into what software packaging entails and why should we care about packaging.
In the practical part of this session, we will explore modern Python packaging and package management using `uv`, a fast Python package manager and project management tool that is rapidly becoming popular across the Python community.

## What is a software package?

A package is more than just code - in may contain a number of components:

* **Source code** – modules, functions and classes that implement the software's functionality.
* **Project/software metadata** – information that describes the software project, such as its name, version, authors, description, homepage and supported Python versions. This information helps users and package repositories identify and manage the package.
* **Dependency information** – a list of the external libraries and packages required for the software to run correctly, along with any version constraints.
* **Licensing information** – the software licence that defines how others can use, modify and redistribute the software (for example, MIT, BSD or GPL).
* **Documentation** – information that helps users understand how to install, use and contribute to the software. This often includes a README, user guides, API documentation and examples.
* **Tests** – special code that verifies the software behaves as expected and helps developers identify bugs when making changes.
* **Build instructions** – configuration information (often part of documentation) that tells packaging tools how to build or install the software.
* Optional **assets** such as **data files**, **images** or **compiled extensions**.

Packages provide a way of bundling these components into an easy to transport form ("archive") that can be more easily distributed and installed.

Packages are used in almost all programming languages, operating systems and computing environments to simplify the process of installing and managing software.

## Software packaging and dependency management

**Software packaging** and **dependency management** are closely related but serve different purposes. 

**Dependency management** focuses on identifying, installing and maintaining the external libraries that the software relies on - making sure that all the external libraries required by the software are available and compatible.
Dependency management is useful even if the software is never distributed as a package. 
Many research projects manage their dependencies to create **virtual reproducible software environments** for reuse without packaging their code.

**Packaging** goes a step further by preparing the software for distribution (e.g. via PyPI or other software registries), bundling the code, metadata and other supporting files into into a distributable and installable artefact ("archive"). 

Modern tools such as `uv` combine these capabilities for Python, making it easy to manage dependencies while also building and distributing packages (and uploading them to a package repository).

By contrast, more traditional tools such as `pip` and `venv` each address a specific part of the workflow: `pip` manages installation of Python packages, while `venv` creates isolated Python environments, requiring multiple tools to accomplish what `uv` provides through a single interface.

::: callout

### Dependencies and virtual reproducible environments 

Software often relies on dozens or hundreds of external packages (dependencies).
Managing these manually (while possible) quickly becomes difficult - dependencies are one of the biggest challenges in software development.

We need a mechanism of isolating the environment for developing and running a software project from the host machine and other projects on the same machine.
You could use a separate machine per project but it is not practical; other approaches include virtual machines or development containers.
These options are not always convenient, so many developers opt for a more lightweight form of isolation: **virtual reproducible environments**.

Modern package managers help with virtual reproducible environments by tracking dependencies and recording exact package versions, and resolving version conflicts.
:::

## Why packaging software matters?

Let's start with a familiar situation. 
Imagine you have written a useful analysis script in Python and a colleague wants to use it.
You send them the code and they immediately encounter problems:

* Which version of Python should they use?
* Which libraries need to be installed?
* Which versions of those libraries are compatible?
* How can they reproduce your software environment?

This is where packaging becomes important.
It provides a standard way to provide answers to the above questions.

Packaging provides a number of important benefits.

Packaging makes software distribution more efficient as it promotes a modular approach to software development and code reuse - reducing duplication.
Applications and libraries typically include only the code that is specific to them, while shared external dependencies can be installed separately and reused across multiple projects.
By not including copies of all the external code it uses - software distribution becomes more efficient.

Packaging helps ensure that software can be shared, reused and reproduced by others. 
Packaging captures important information about dependencies, versions and installation requirements, making it easier for collaborators to run your software and obtain consistent results. 
Good packaging practices also support software sustainability by making projects easier to maintain, distribute and build upon over time.

## Python packaging and dependency management tooling ecosystem

Python packaging and dependency management tooling ecosystem is complicated and often confusing.
There are tools that install packages only, then install and create and dependency management tools and environment isolation.
Check out this [very good and complete-ish guide to dependency management in Python](https://nielscautaerts.xyz/python-dependency-management-is-a-dumpster-fire.html).

### Traditional tools

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
It allows users to install packages from package repositories such as [PyPI][pypi], as well as from local directories, archives or source code repositories such as GitHub.

While `pip` remains the standard installer included with Python, newer tools are increasingly combining package installation, environment management and dependency resolution into a single workflow.

#### `requirements.txt`

A `requirements.txt` file provides a simple way to record project dependencies. 
It is commonly used to specify the packages and versions needed to recreate a software environment.

Although still widely used, many modern projects now manage dependencies directly through `pyproject.toml` and lock files.

### Modern tools

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

[Uv][uv] is a modern package and project management tool developed by Astral. 
It combines dependency management, virtual environment management and package installation into a single, high-performance tool.

Many common tasks that previously required several separate tools can now be performed through a single command-line interface.
We will use `uv` tool in the practical part of this session.

### Poetry

[Poetry][poetry] focuses on dependency management, reproducible environments and simplified package publishing. 
It introduced many of the ideas that influenced modern Python project management and remains popular in both industry and research.

### Hatch

[Hatch][hatch] provides a flexible framework for managing Python projects, environments and package builds. 
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

### Lock files

A lock file records the exact versions of all packages and dependencies used in a software environment. 
While a project's `pyproject.toml` file specifies the packages that a project depends on, it often allows a range of compatible versions to be installed. 
As a result, two people installing the same project at different times may receive slightly different versions of the underlying dependencies.

A lock file removes this uncertainty by recording the precise version of every package that was resolved during installation, including indirect dependencies (dependencies of dependencies). 
This allows the same software environment to be recreated consistently across different machines and at different points in time.

For example, a project may specify a dependency on `pandas >=2.0`. 
Without a lock file, one user might install version 2.1 while another installs version 2.3. 
With a lock file, both users install exactly the same version that was originally tested with the project.

Modern package managers such as `uv` automatically generate and maintain lock files (e.g. `uv.lock`). 
These files play an important role in reproducible research by helping collaborators recreate the same software environment and reducing the risk of unexpected behaviour caused by dependency updates.

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

## Package repositories

Once packages have been built, they are typically published to a package repository.
The most widely used repository for Python packages is the Python Package Index (PyPI), which hosts hundreds of thousands of open-source packages.
Package repositories provide a central location where users can discover, download and install software.

## Summary

Software packaging is a fundamental practice that helps make software easier to share, install, reuse and maintain. 
This session introduces the concepts behind software packaging, explains why packaging is important for reproducible and sustainable research software, and provides an overview of the modern Python packaging ecosystem. 
Participants will learn about package structure, dependency management, packaging tools and distribution formats before exploring modern Python packaging workflows using `uv` in a practical session.

::: keypoints

- Software packaging bundles code, metadata, dependencies and documentation into a distributable artefact.
- Packaging makes software easier to share, install, reuse and reproduce.
- Python packages are commonly distributed as source distributions (sdists) or wheels.
- Modern Python packaging is built around the `pyproject.toml` standard.
- Package managers such as `uv` simplify dependency management, environment creation and package installation.
- Lock files help create reproducible software environments by recording exact dependency versions.
- Package repositories such as PyPI provide a central location for publishing and installing software.

:::