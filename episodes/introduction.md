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

Many researchers write Python code, install Python libraries, and share scripts with collaborators. 
However, fewer people understand how packages are created, how dependencies are managed or how to make software easy for others to install and reuse.

Understanding packaging helps us move from writing code that works on our machine to developing software that others can reliably install, use, reproduce and contribute to.

In the practical part of this session, we will explore modern Python packaging and package management using `uv`, a fast Python package manager and project management tool that is rapidly becoming popular across the Python community.

# What is a software package?

A package is more than just code.

A Python package usually contains:

* Source code
* Project metadata
* Dependency information
* Licensing information
* Documentation
* Tests
* Build instructions

Packaging combines all of these components into a form that can be distributed and installed.

A useful way to think about packages is that they provide a contract between software developers and software users.

The package tells users:

"I need these dependencies, I support these Python versions, and this is how I should be installed."

## Why packaging software matters?

Let's start with a familiar situation.

Imagine you have written a useful analysis script in Python and a colleague wants to use it.

You send them the code and they immediately encounter problems:

* Which version of Python should they use?
* Which libraries need to be installed?
* Which versions of those libraries are compatible?
* How can they reproduce your environment?

This is where packaging becomes important.

Packaging provides a standard way to describe:

* What software is being distributed
* What dependencies it requires
* How it should be installed
* Which versions are supported

Packaging makes software:

* Reusable
* Shareable
* Maintainable
* Reproducible

For research software, these are key characteristics of sustainable software.

## What's inside a package?

A Python package contains much more than code.

Typical contents include:

- The application or library code.
- Metadata describing the package.
- Dependency information.
- A licence.
- Documentation such as a README.
- Optional assets such as data files, images, or compiled extensions.

Packaging helps bring all of these components together into a shareable and installable unit.

# The Python Packaging Ecosystem

Python packaging has evolved significantly over time.

Historically, many researchers used:

* `setup.py`
* `setuptools`
* `pip`
* `requirements.txt`

These tools are still widely used, but the ecosystem has become increasingly fragmented.

Modern Python projects increasingly use:

* `pyproject.toml`
* uv
* Poetry
* Hatch
* PDM

The key change is the adoption of the pyproject.toml standard.

This file provides a standard location for:

* Project metadata
* Dependencies
* Build configuration
* Tool settings

Instead of maintaining multiple configuration files, projects can often keep everything in a single place.

This standardisation has simplified Python packaging considerably.

# Introducing uv (5 min)

uv is a modern Python package and project manager developed by Astral.

Its goals are:

* Speed
* Simplicity
* Compatibility with Python standards

uv can replace several tools commonly used in Python development:

| Traditional Tool | uv Equivalent        |
| ---------------- | -------------------- |
| pip              | uv add               |
| pip install      | uv sync              |
| virtualenv       | uv venv              |
| pip-tools        | built in             |
| pyenv workflows  | partially integrated |

One reason uv has gained attention is performance.

Dependency resolution and installation can be dramatically faster than traditional workflows.

However, the most important benefit for many researchers is that it simplifies project management.

Instead of stitching together multiple tools, many common tasks can be managed through a single interface.

---

# Managing Dependencies (4 min)

Dependencies are one of the biggest challenges in software development.

Research software often relies on dozens or hundreds of packages.

Managing these manually quickly becomes difficult.

Modern package managers help by:

* Tracking dependencies
* Resolving version conflicts
* Creating reproducible environments
* Recording exact package versions

With uv, dependencies are declared in pyproject.toml.

For example:

* numpy
* pandas
* matplotlib

uv automatically resolves compatible versions and records them in a lock file.

The lock file ensures that collaborators can recreate the same software environment later.

This is an important contribution to reproducibility.

---

# Packaging and Research Software Sustainability (3 min)

Packaging is not just a technical exercise.

Good packaging practices support:

* Reproducibility
* Reusability
* Collaboration
* Maintenance
* Credit and citation

A package that can be easily installed is more likely to be reused.

A package with explicit dependencies is easier to reproduce.

A package with clear metadata is easier to discover and cite.

Packaging therefore supports many of the goals promoted by the research software engineering community.

---

# From User to Developer (1 min)

Most researchers begin as package users.

They install libraries and use them in their work.

As projects mature, researchers often become package developers:

* Creating reusable tools
* Sharing software with collaborators
* Publishing software openly
* Building community around their code

Understanding packaging is one of the key steps in that transition.

---

# What We'll Do Next (1 min)

In the practical session we'll see how uv can be used to:

* Create a new project
* Manage dependencies
* Create isolated environments
* Structure a Python package
* Prepare software for sharing

By the end of the tutorial you'll have seen the core components of a modern Python project and how uv helps manage them.


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