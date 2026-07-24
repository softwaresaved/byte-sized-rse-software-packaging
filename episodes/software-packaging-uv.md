---
title: "Software packaging with uv"
teaching: 15
exercises: 45
---

:::::::::::::::::::::::::::::::::::::: questions 

- How do I create a new Python project with `uv`?
- What project files does `uv` create/maintain and what are they used for?
- What is `pyproject.toml` and why has it become the standard project configuration file?
- How does `uv` manage dependencies and virtual environments?
- What is a lock file and why is it important for reproducible software?
- How do I add, remove and update project dependencies?
- How can I run Python scripts using `uv`?
- How do I build a source distribution and a wheel using `uv`?
- How can I migrate an existing project from `pip` and `venv` to `uv`?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain the role of `uv` in modern Python project development.
- Create and initialise a new Python project using `uv`.
- Manage project dependencies and understand the purpose of lock files.
- Describe the structure and role of `pyproject.toml` as the central project configuration file.
- Run Python commands and scripts using `uv` without manually managing virtual environments.
- Build Python source distributions and wheel packages for distribution.
- Migrate an existing pip/venv-based projects to a `uv` development workflow.

::::::::::::::::::::::::::::::::::::::::::::::::


## Introduction

`uv` is a multi-functional tool, that performs a number of tasks:

- installs and manages Python versions,
- provides comprehensive Python project and dependency management capabilities and handles virtual environments with a universal lockfile,
- runs Python scripts,
- runs and installs tools (Python modules) published as Python packages (e.g. `pip`, etc.),
- includes a pip-compatible interface for users with existing pip-based development workflows,
- is really performant and much faster than `pip`.

The core `uv` project development workflow looks like as follows:

1. project creation and setup - `uv` can create a new project layout for you so you start with a clean structure instead of a blank folder
2. virtual environments - `uv` manages virtual environments directly, which means you do not need to switch between different tools to create and activate environments
3. dependency installation - `uv` installs packages quickly and consistently while still supporting the same dependency ecosystem you may already be familiar with
4. running commands inside the environment - `uv` can run Python commands or scripts inside the environment without you having to manually activate it
5. (optionally) package and publish your code.

There is a number of `uv` commands to handle Python projects as part of the development workflow; we give a short summary below.

::: callout
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
:::

The full list of all `uv` commands is available from the [`uv` features documentation](https://docs.astral.sh/uv/getting-started/features/).
A number of [`uv` guides](https://docs.astral.sh/uv/guides/) covering the full set of commands and features is also available and highly recommended.

## Using `uv` for new projects

If you are starting a new project then you can use `uv` straight away to replace all the other tools you may have used in the past in your development workflow.

### Creating a new project

To create a brand new Python project called "hello-world" using the `uv init` command, do:

```bash
$ uv init hello-world
$ cd hello-world
```

Alternatively, you can initialise a project in an existing, empty working directory:

```bash
$ mkdir hello-world
$ cd hello-world
$ uv init
```

`uv` will create the following files and directories as a basic scaffolding for your new Python project:

```text
.
├── .git/
├── .gitignore
├── .python-version
├── README.md
├── main.py
└── pyproject.toml
```

The `.python-version` file contains the project's default Python version (based on what is installed on your machine). 
This file tells `uv` which Python version to use when creating the project's virtual environment.

The `main.py` Python file contains a simple "Hello world" program. 

`uv` has also created a starter `pyproject.toml` file to store the metadata about your project.

```text
[project]
name = "hello-world"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = []
```

`pyproject.toml` is a configuration file introduced by [PEP 518](https://peps.python.org/pep-0518/) to standardise Python project setup. 
Written in TOML (Tom’s Obvious, Minimal Language), it serves as a single source of truth for your project’s build system, metadata, and tool configurations. 
It tells tools like `uv`, `pip`, `poetry`, or `hatch` how to construct your project.

Unlike the older `setup.py` approach, which relied on executable Python code, `pyproject.toml` uses a declarative, static format. 

`pyptoject.toml` structure contains three main components (tables):

- **\[build-system]**: defines the tools and other dependencies are needed to build your project's source distribution and wheel (built distribution).
- **\[project]**: stores metadata like your project’s name, version, and dependencies.
- **\[tool]**: configures settings for development tools like linters or formatters.

The [build-system] table is not compulsory - you can use `pyproject.toml` just to store metadata and configuration. 
If your project is an application (not designed to be published as a package), you do not need a [build-system] table. 
`uv` will install and manage your dependencies, but it will not attempt to build the project itself.
If you are building a package or library to be published, the [build-system] table is required so `uv` knows which build backend to use (e.g. `uv_build`, `setuptools`, `hatch`).

For example:

```text
[build-system]
requires = ["uv_build>=0.11.31,<0.12"]
build-backend = "uv_build"
```

Or:
```text
[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"
```

`uv` supports all build backends as specified by [PEP 517](https://peps.python.org/pep-0517/), but also provides a native build backend (`uv_build`) that integrates tightly with `uv` to improve performance and user experience.
When you create a new project with `uv init`, the `uv_build` backend will be used by default if you issue the `uv build` command to create the source distribution and wheel for your project.
`uv_build` backend has reasonable defaults and requires zero configuration for most users so you do not actually have to declare it in `pyptoject.toml` unless you want to change it to another build backend or configure it further.
You can initialise projects as packages even even you are just developing applications by running `uv init --package` - this will setup the project using a src-layout structure and setup `uv_build` as a build system in your `pyptoject.toml`.

The [tool] table has tool-specific subtables, e.g., [tool.uv], [tool.hatch], content of which are defined by each tool. 
You would need to consult the particular tool’s documentation to know what it can contain.

A minimal `pyproject.toml` just requires the [project] table (as shown above).

### Managing dependencies

You can add dependencies to your project (and `pyproject.toml`) with the `uv add` command. 
For example, to add `requests` package, do:

```bash
$ uv add requests
```

You may now notice the new `.venv` virtual environment and `uv.lock` file being added to your project after running the `uv add` command.
They will be updated each time you, e.g. add or remove a dependency or run the code (i.e. run either of the `uv run`, `uv add`, `uv sync`, or `uv lock` commands).

The project structure now looks like the following - all these parts work together and allow `uv` to manage your project.

```text
.
├── .git/
├── .venv/
│   ├── bin
│   ├── lib
│   └── pyvenv.cfg
├── .gitignore
├── .python-version
├── README.md
├── main.py
├── pyproject.toml
└── uv.lock
```

You can also specify version constraints or alternative sources for packages with the `uv add` command.

```bash
$ uv add 'requests==2.31.0'
```

To add a dependency sourced from GitHub (as opposed to the default PyPI):

```bash
$ uv add git+https://github.com/psf/requests
```

To remove a dependency, you can use `uv remove`:

```bash
$ uv remove requests
```

To upgrade a package, run `uv lock` with the `--upgrade-package` flag:

```bash
$ uv lock --upgrade-package requests
```

The `--upgrade-package` flag will attempt to update the specified package to the latest compatible version, while keeping the rest of the lock file intact.

### Running commands

`uv run` can be used to run Python scripts or Python commands in your project environment.

```text
$ uv run main.py
```

Prior to every `uv run` invocation, `uv` will verify that the lockfile is up-to-date with the `pyproject.toml`, and that the environment is up-to-date with the lock file, keeping your project in-sync without the need for manual intervention. 
`uv run` guarantees that your command is run in an environment with all required dependencies at their locked versions.

Alternatively, you can use `uv sync` to manually update the environment (recorded in in `.venv/` folder) then activate it before executing a Python command or running your Python script:

```bash
$ uv sync
$ source .venv/bin/activate # macOS or Linux
$ python3 main.py
```

```bash
$ uv sync
$ source .venv\Scripts\activate # Windows
$ python3 main.py
```

Note: the virtual environment must be active to run scripts and commands in the project without `uv run`. 
Virtual environment activation differs per shell and platform.

### Building distributions

`uv build` command can be used to build source distributions and binary distributions (wheel) for your project.

By default, `uv build` will build the project from the current directory, and place the built artifacts in a dist/ subdirectory:

```bash
$ uv build
$ ls dist/
hello_world-0.1.0-py3-none-any.whl	
hello_world-0.1.0.tar.gz
```

## Switching to `uv` in existing projects

If you already have an existing project that uses `venv` and `pip`, you might be wondering how to migrate to using `uv` for your development workflow. 
It is rather straightforward.
Let's have a look at the [example project we downloaded as part of the setup](./index.html#example-code).

If you have not done it already, make a copy of [example project repository](https://github.com/softwaresaved/spacewalks_example) into your GitHub account by using the `Use this template` button on GitHub.
Next, clone your copy of the repository locally:

```bash
$ cd ~
$ git clone https://github.com/YOUR-GITHUB/spacewalks_example
```

Let's navigate into the `spacewalks_example` project directory.

```bash
$ cd spacewalks_example
$ ls -la
total 56
drwxr-xr-x   15 mbassan2  staff    480 23 Jul 17:21 .
drwx------@ 897 mbassan2  staff  28704 23 Jul 17:21 ..
drwxr-xr-x   12 mbassan2  staff    384 23 Jul 17:21 .git
-rw-r--r--    1 mbassan2  staff     26 23 Jul 17:21 .gitignore
-rw-r--r--    1 mbassan2  staff    721 23 Jul 17:21 CITATION.cff
drwxr-xr-x    3 mbassan2  staff     96 23 Jul 17:21 data
drwxr-xr-x    7 mbassan2  staff    224 23 Jul 17:21 docs
-rw-r--r--    1 mbassan2  staff   3991 23 Jul 17:21 eva_data_analysis.py
-rw-r--r--    1 mbassan2  staff   1090 23 Jul 17:21 LICENSE
-rw-r--r--    1 mbassan2  staff    296 23 Jul 17:21 mkdocs.yml
-rw-r--r--    1 mbassan2  staff   2009 23 Jul 17:21 README.md
-rw-r--r--    1 mbassan2  staff    805 23 Jul 17:21 requirements.txt
drwxr-xr-x    5 mbassan2  staff    160 23 Jul 17:21 results
drwxr-xr-x   12 mbassan2  staff    384 23 Jul 17:21 site
drwxr-xr-x    3 mbassan2  staff     96 23 Jul 17:21 tests
```

Let's assume this project uses `venv`, `pip` and `requirements.txt` to manage and record dependencies and the virtual environment.

To migrate from the existing development workflow to use `uv`, you need to use `uv init` first to initiate the project and get the basic `pyptoject.toml` file created.

```bash
$ uv init
```

### Managing dependencies

Next, we need to install dependencies for our project, which are recorded in `requirements.txt` file.
By looking at the code in `eva_data_analysis.py` Python script, we can tell we require `matplotlib` and `pandas` packages. 

We can use the `uv add` command to install them:

```bash
$ uv add matplotlib
$ uv add pandas
```

Alternatively, we can use the `uv add` command with the `-r` flag to add all dependencies from the `requirements.txt` file in a single go.

```bash
$ uv add -r requirements.txt
```

You may notice that after running the `uv add` command - we now have a virtual environment in `.venv` directory (as well as the `uv.lock` file).
Note that if you have an existing virtual environment in `.venv` directory, `uv` will continue to use that by default to record changes to it.
If you do not have an existing environment, you can create it with the `uv venv` command - by default it will end up in `.venv` directory in the project root along with the `uv.lock` file.

Recall that `uv` creates a virtual environment and the `uv.lock` file the first time you run any project command, i.e., `uv add`, `uv run`, `uv sync`, or `uv lock` (so you do not have to do it explicitly with `uv venv`).

#### Runtime and development dependencies

Tools like `uv` understand that there are two different types of dependencies: runtime dependencies and development dependencies. 
Runtime dependencies need to be installed for our code to run, like `matplotlib` for our code. 
Development dependencies are an essential part of the development process for a project, but are not required to run it. 
For example - `mkdocs` used to build the documentation for our software project or `pytest` to run tests.

With `uv add matplotlib` and `uv add -r requirements.txt` commands, `uv` will add all packages to the list of runtime dependencies in `pyproject.toml` (regardless if they are runtime or development dependencies - there is not way to tell the difference with `requirements.txt`).

To add `mkdocs` and `pytest` as development dependencies, the --group option can be used:

```bash
$ uv add --group dev mkdocs pytest
```

This will add a new section to the `pyproject.toml` file for development dependencies:

```text
[dependency-groups]
dev = [
"mkdocs>=1.6.1",
"pytest>=TODO"
]
```

Now when someone installs our package on their machine, only the runtime dependencies will be installed since development dependencies are not needed to run the code.

To install the development dependencies, one needs to clone our repository from GitHub and then specify the dev extra when installing:

```
$ python3 -m pip install .[dev]
```

At this point, you can delete `requirements.txt` and carry on running your Python scripts or Python tools with `uv`.
Recall that if you are using `uv` to run Python commands - you do not have to activate the environment explicitly.
If you prefer to use Python directly, you will have to activate your environment first but switch to `uv` for managing it from whatever tools you used before.

### Building distributions

Once we have made sure our project is in the right structure, we can go ahead and build a distributable version of our software as before:

```bash
$ uv build
$ ls dist/
spacewalks_example-0.1.0-py3-none-any.whl	
spacewalks_example-0.1.0.tar.gz
```

The one we care most about is the `.whl` or wheel file. 
This is the file that package managers use to distribute and install Python packages, so this is the file we would need to share with other people who want to install our software.

By convention distributable package names use hyphens, whereas module package names use underscores. 
While we could choose to use underscores in a distributable package name, we cannot use hyphens in a module package name, as Python will interpret them as a minus sign in our code when we try to import them.

Now if we gave this wheel file to someone else, they could install it using `uv` (or `pip` or another package installer).

```bash
$ python3 -m pip install spacewalks_example-0.1.0-py3-none-any.whl
```

And then they could import our package in their own Python environment:

```python
import spacewalks_example
```

After we have been working on our code for a while and want to publish an update, we just need to update the version number in the pyproject.toml file (using SemVer perhaps), then use `uv` to build and publish the new version.

uv can help easily increment the version number following Semantic Versioning conventions. For example, to increment the minor version number, we can do:

```bash
$ uv version --bump minor
```
Then the version number in `pyproject.toml` will be updated from 0.1.0 to 0.2.0.

## Summary

`uv` provides a modern, standards-based workflow that simplifies Python project management by bringing dependency management, virtual environments, packaging and publishing together in a single tool.

::: keypoints

- `uv` combines Python installation, dependency management, virtual environments, packaging and publishing into a single, high-performance tool.
- `pyproject.toml` is the standard configuration file for modern Python projects and stores project metadata, dependencies and tool configuration.
- Dependencies are declared in `pyproject.toml`, while `uv.lock` records the exact package versions needed to reproduce the project environment with `uv`.
- `uv` automatically keeps the project environment and lock file synchronised, reducing manual environment management.
- `uv` run executes commands in the correct project environment without requiring manual activation of a virtual environment.
- Python packages are typically distributed as source distributions (.tar.gz) and wheels (.whl), which can be built using `uv` build.
- Existing `pip` and `venv` development workflows can be migrated to `uv` with only a few commands for most projects.

:::