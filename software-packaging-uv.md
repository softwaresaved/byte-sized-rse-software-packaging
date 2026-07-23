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

- installs and manages Python versions,
- provides comprehensive Python project and dependency management capabilities and handles virtual environments with a universal lockfile,
- runs Python scripts,
- runs and installs tools (Python modules) published as Python packages (e.g. `pip`, etc.),
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

### Creating a new project

You can create a new Python project called "hello-world" using the `uv init` command:

```shell
uv init hello-world
cd hello-world
```

Alternatively, you can initialise a project in the working directory:

```shell
mkdir hello-world
cd hello-world
uv init
```

`uv` will create the following files and directories as a scaffolding for the new Python project:

```text
├── .git/
├── .gitignore
├── .python-version
├── README.md
├── main.py
└── pyproject.toml
```

The `main.py` Python file contains a simple "Hello world" program. 
We can try it out with `uv run`:

```text
uv run main.py
```

### `pyptoject.toml` structure

`uv` has also created a starter `pyproject.toml` file to store the metadata about your project.

```text
[project]
name = "hello-world"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = []
```

`pyproject.toml` is a configuration file introduced by PEP 518 to standardise Python project setup. 
Written in TOML (Tom’s Obvious, Minimal Language), it serves as a single source of truth for your project’s build system, metadata, and tool configurations. 
It tells tools like `uv`, `pip`, 'poetry`, or `hatch` how to construct your project.

Unlike the older `setup.py` approach, which relied on executable Python code, `pyproject.toml` brings a declarative, static format. 

`pyptoject.toml` structure contains three main components (tables):

- **\[build-system]**: defines the tools and other dependencies are needed to build your project's source or built distribution.
- **\[project]**: stores metadata like your project’s name, version, and dependencies.
- **\[tool]**: configures settings for development tools like linters or formatters.

The [build-system] table is not compulsory - you can use `pyproject.toml` just to store metadata and configuration. 
If your project is an application (not designed to be published as a package), you do not need a [build-system] table. 
`uv` will install and manage your dependencies, but it will not attempt to build the project itself.
If you are building a package or library to be published, the [build-system] table is required so `uv` knows which build backend to use (e.g. `hatch`, `setuptools`).

A minimal `pyproject.toml` just requires the [project] table (as shown above).

::: callout

https://packaging.python.org/en/latest/guides/writing-pyproject-toml/

### `uv` project commands

A summary of `uv` commands for creating and working on Python projects, i.e., with a `pyproject.toml` file.

- `uv init`: create a new Python project.
- `uv add`: add a dependency to the project.
- `uv remove`: remove a dependency from the project.
- `uv sync`: sync the project's dependencies with the environment.
- `uv lock`: create a lockfile for the project's dependencies (locking the virtual environment).
- `uv run`: run a Python command or a Python script in the project environment.
- `uv tree`: view the dependency tree for the project.
- `uv build`: build the project into distribution archives.
- `uv publish`: publish the project to a package index.

The full list of all `uv` commands is available from the [`uv` features documentation](https://docs.astral.sh/uv/getting-started/features/).

:::

## Switching to `uv` in existing projects

### 

::: keypoints

- TODO
:::