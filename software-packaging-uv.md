---
title: "Software packaging with uv"
teaching: 15
exercises: 45
---

:::::::::::::::::::::::::::::::::::::: questions 

- TODO

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- TODO

::::::::::::::::::::::::::::::::::::::::::::::::


## Introduction

Let's have a more detailed look into `uv`.

`uv` is a multi-functional tool, which:

- provides comprehensive Python project and dependency management capabilities and handles virtual environments with a universal lockfile,
- runs Python scripts,
- installs and manages Python versions,
- runs and installs tools published as Python packages (e.g. `pip`, etc.),
- includes a pip-compatible interface for users with existing pip-based development workflows,
- is really performant and much faster than `pip`.

## Using `uv` for new projects

If you are starting a new project then you can use `uv` straight away to replace all the other tools you may have used in the past in your development workflow.

The core `uv` project development workflow looks like as follows:

1. project creation and setup - `uv` can create a new project layout for you so you start with a clean structure instead of a blank folder
2. virtual environments - `uv` manages virtual environments directly, which means you do not need to switch between different tools to create and activate environments
3. dependency installation - `uv` installs packages quickly and consistently while still supporting the same dependency ecosystem you may already be familiar with
4. running commands inside the environment - `uv` can run Python commands or scripts inside the environment without you having to manually activate it
5. (optionally) package and publish your code.


::: callout

### `uv` commands summary
A summary of `uv` commands for creating and working on Python projects, i.e., with a `pyproject.toml` file.

- `uv init`: create a new Python project.
- `uv add`: add a dependency to the project.
- `uv remove`: remove a dependency from the project.
- `uv sync`: sync the project's dependencies with the environment.
- `uv lock`: create a lockfile for the project's dependencies (locking the virtual environment).
- `uv run`: run a Python command in the project environment.
- `uv tree`: view the dependency tree for the project.
- `uv build`: build the project into distribution archives.
- `uv publish`: publish the project to a package index.

The full list of all `uv` commands is available from the [`uv` features documentation](https://docs.astral.sh/uv/getting-started/features/).

:::

## Switching to uv for existing projects

### 

::: keypoints

- TODO
:::