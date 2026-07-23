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

Packages provide a way of bundling these components into an easy to transport artefact ("archive") - that can be more easily distributed and installed.

Packages are used in almost all programming languages, operating systems and computing environments to simplify the process of installing and managing software.

## Why packaging software matters?

Let's start with a familiar situation.
Imagine you have written a useful analysis script in Python and a colleague wants to use it.
You send them the code and they immediately encounter problems:

* Which version of Python should they use?
* Which libraries need to be installed?
* Which versions of those libraries are compatible with one another?
* How can they reproduce your software environment?

This is where packaging becomes important - it provides one standard way distribute and install your code on other people's machines.

Packaging provides a number of important benefits.

Packaging makes software distribution more efficient as it promotes a modular approach to software development and code reuse - reducing duplication.
Applications and libraries typically include only the code that is specific to them, while shared external dependencies can be installed separately and reused across multiple projects.
By not including copies of all the external code it uses - software distribution becomes more efficient.

Packaging helps ensure that software can be shared, reused and reproduced by others.
Packaging captures important information about dependencies, versions and installation requirements, making it easier for collaborators to run your software and obtain consistent results.
Good packaging practices also support software sustainability by making projects easier to maintain, distribute and build upon over time.

## Dependencies, virtual development environments and packages

**Dependency management** and **software packaging** are closely related but serve different purposes. 

**Dependency management** focuses on identifying, installing and maintaining the external packages that the software relies on - making sure that all the external libraries required by the software are available and compatible.
Dependency management is useful even if your software is never distributed as a package (because it will almost always rely on a number of external dependencies that you will have to manage on your system).

Next step is to capture this **dependency information** (dependencies and their versions) for your software project's environment somehow so others (or yourself on another machine) can replicate that.
Dependency information can be recorded manually (e.g. just describing them as a list) but it is not very practical as software often relies on dozens or hundreds of external packages.
Such lists, if not managed by special tools, quickly become out of date.

Dependency information for your software can then used to recreate a **virtual (reproducible) software environment** for your code on any machine (in theory).
This is a mechanism of isolating the environment for developing and running a software project (containing the specified dependencies) from other projects on the same machine.

**Packaging** goes a step further by preparing the software for distribution (e.g. via PyPI or other software registries), bundling the code, software metadata, dependency information and other supporting files into into a distributable and installable artefact ("archive"). 

Modern tools such as `uv` combine these capabilities for Python, making it easy to manage dependencies while also building and distributing packages (and uploading them to a shared package repository).

By contrast, more traditional tools such as `pip` and `venv` each address a specific part of the workflow: `pip` manages installation of Python packages, while `venv` creates isolated Python environments, requiring multiple tools to accomplish what `uv` provides through a single interface.

## Python packaging and dependency management tool ecosystem

The Python packaging and dependency management ecosystem is rich but can also be complicated and, at times, confusing. 
Different tools address different aspects of the software development workflow. 
Some tools focus solely on installing packages (such as `pip`), others create isolated Python environments (such as `venv`), while modern tools like `uv`, `poetry` and `hatch` combine package installation, dependency management, environment management and packaging into a single workflow.

If you'd like to explore the ecosystem in more detail, ["Python dependency management is a dumpster fire" guide](https://nielscautaerts.xyz/python-dependency-management-is-a-dumpster-fire.html) provides a comprehensive overview of dependency management in Python.

We are mostly going to focus on packaging tools but will unavoidably mention dependency and environment management tools too.

### Traditional tools & methods

Historically, many researchers created Python packages using a combination of the following tools.

#### Pip

`pip` is Python's traditional package installer.
It allows users to install packages from package repositories such as [PyPI][pypi], as well as from local directories, archives or source code repositories such as GitHub.

While `pip` remains the standard installer included with standard Python distribution, newer tools are increasingly combining package installation, environment management and dependency resolution into a single workflow.

#### Setup.py

For many years, `setup.py` file (which exists at the root of a software project directory) was the primary mechanism for defining how a Python package (contained in that directory) should be built and installed. 
It contains information about the package, including its name, version, dependencies and build instructions.

`setup.py` is a Python file, the presence of which is an indication that the module/package you are about to install has likely been packaged and distributed with `setuptools`, which is the traditional standard for distributing Python modules.
Invoking Python `setup.py` script directly is now deprecated, but `setup.py` is still a valid `setuptools` configuration file.

Alternatively, if you run:

```bash
$ pip install .
``` 

from the software project directory root, `pip` will use information in `setup.py` to install the software package.

Below is an [example `setup.py`](https://pythonhosted.org/an_example_pypi_project/setuptools.html#setting-up-setup-py) from [Pythonhosted's "Getting Started With setuptools and setup.py" guide](https://pythonhosted.org/an_example_pypi_project/setuptools.html).
Recall that the content of `setup.py` is just Python code.

```python
import os
from setuptools import setup

# Utility function to read the README file.
# Used for the long_description.  It's nice, because now 1) we have a top level
# README file and 2) it's easier to type in the README file than to put a raw
# string in below ...
def read(fname):
    return open(os.path.join(os.path.dirname(__file__), fname)).read()

setup(
    name = "an_example_pypi_project",
    version = "0.0.4",
    author = "Andrew Carter",
    author_email = "andrewjcarter@gmail.com",
    description = ("An demonstration of how to create, document, and publish "
                                   "to the cheese shop a5 pypi.org."),
    license = "BSD",
    keywords = "example documentation tutorial",
    url = "http://packages.python.org/an_example_pypi_project",
    packages=['an_example_pypi_project', 'tests'],
    long_description=read('README'),
    classifiers=[
        "Development Status :: 3 - Alpha",
        "Topic :: Utilities",
        "License :: OSI Approved :: BSD License",
    ],
)
```

While still supported by many projects, modern Python packaging has moved away from relying directly on `setup.py` in favour of other standardised configuration files, such as `pyproject.toml`.

#### Setuptools

`setuptools` is one of the oldest and most widely used Python packaging tools. 
It extends Python's original package management tool called `distutils` and provides functionality for building, packaging and distributing software.
`distutils` is now officially abandoned and has been removed entirely from the Python standard library as of Python 3.12.

One of the main advantages of `setuptools` over `distutils` is that it allows you to specify dependencies for your package, so that other packages that your package depends on will be automatically installed when your package is installed. 
This makes it easier to distribute your package, because users don't have to manually install the dependencies before installing your package.

`setuptools` also provides tools for creating and managing Python virtual environments, which are isolated Python environments that allow you to have multiple versions of the same package installed on the same machine. 
This is useful for development and testing, because you can test your package in different environments without affecting other projects.

Many modern packaging tools still rely on `setuptools` behind the scenes, even when developers no longer interact with it directly.

#### Requirements.txt

A `requirements.txt` file provides a simple way to record project's dependency information. 
It is commonly used to specify the packages and versions needed to recreate a virtual software environment (e.g. using `pip`).

Although still used, many modern Python projects now manage dependencies and development environment through `pyproject.toml` and lock files (to be covered soon).

Below is an example `requirements.txt` for the software project we will use in examples in this lesson.
`requirements.txt` is a plain text file which records dependencies (one per line) and locks their versions installed in the current virtual development environment (it acts as a basic lock file).

```text
contourpy==1.3.3
coverage==7.12.0
cycler==0.12.1
fonttools==4.60.1
iniconfig==2.3.0
kiwisolver==1.4.9
matplotlib==3.10.7
numpy==2.3.5
packaging==25.0
pandas==2.3.3
pillow==12.0.0
pluggy==1.6.0
Pygments==2.19.2
pyparsing==3.2.5
pytest==9.0.1
pytest-cov==7.0.0
python-dateutil==2.9.0.post0
pytz==2025.2
six==1.17.0
tzdata==2025.2
```

### Modern tools & methods

Python packaging has evolved significantly over time.
Modern package managers help by:

* Tracking dependencies
* Resolving version conflicts
* Creating reproducible environments
* Recording exact package versions

#### pyproject.toml

The introduction of `pyproject.toml` file (written in [TOML format][toml]) has been one of the most significant developments in Python packaging.
It provides a standard location for:

* Project metadata
* Dependencies
* Build configuration
* Tool-specific settings

Instead of maintaining information across multiple files, projects can now define most packaging-related configuration in a single location.
This standardisation has simplified Python packaging considerably.

Modern Python projects increasingly adopt the following tools that manage packages described with `pyproject.toml`.

#### Lock files

A lock file records the exact versions of all dependencies used in a software environment.
While a project's `pyproject.toml` file specifies the packages that a project depends on, it often allows a range of compatible versions to be installed.
As a result, two people installing the same project at different times may receive slightly different versions of the underlying dependencies.

A lock file removes this uncertainty by recording the precise version of every package that was resolved during installation, including indirect/transitive dependencies (dependencies of dependencies) that have not been explicitly declared anywhere in your project.
This allows the same software environment to be recreated consistently across different machines and at different points in time.

For example, a project may specify a dependency on `pandas >=2.0`.
Without a lock file, one user might install version 2.1 while another installs version 2.3.
With a lock file, both users install exactly the same version that was originally tested with the project.

Modern package managers such as `uv` automatically generate and maintain lock files (e.g. `uv.lock`).
These files play an important role in reproducible research by helping collaborators recreate the same software environment and reducing the risk of unexpected behaviour caused by dependency updates.

::: callout 

### Fully reproducible environments

Remember, if two people create environments from a dependency definition file at different points in time, they could end up with different environments.
One could work fine, while the other could be broken. 
And this could be caused by a broken release from some transitive dependency we did not even know we needed.

The lock file represents a fully reproducible environment (as long as packages and versions remain available in package repositories). 

Thus, good dependency management boils down to a few steps:

* Creating a dependency definition file (listing all direct dependencies)
* Generating a lock file (explicit step of "locking" our direct and indirect dependencies)
* Tracking both the definition file and the lock file in version control
* Syncing our environment with the lock file
:::
 
#### uv

[Uv][uv] is a modern package and project management tool developed by Astral. 
It combines dependency management, virtual environment management and package installation into a single, high-performance tool.

With `uv`, dependencies are declared in `pyproject.toml`.
`uv` automatically resolves compatible versions and records them in a "lock" file called `uv.lock`.
The lock file ensures that collaborators can recreate the same software environment later.
This is an important contribution to reproducibility.

Many common tasks that previously required several separate tools can now be performed through a single command-line interface.
We will use `uv` tool in the practical part of this session.

#### Poetry

[Poetry][poetry] focuses on dependency management, reproducible environments and simplified package publishing. 
It introduced many of the ideas that influenced modern Python project management and remains popular in both industry and research.

#### Hatch

[Hatch][hatch] provides a flexible framework for managing Python projects, environments and package builds. 
It is particularly popular among developers who require more advanced build and release workflows.

#### Comparison of modern packaging tools

The above modern packaging tools are not competing packaging standards — they all work with the same underlying Python packaging standards. 
They mainly differ in the workflow and user experience they provide.
A summary of their main characteristics is shown below.

| Feature | `uv`                                               | `poetry`                                   | `hatch`                                                                                                 |
|---------|----------------------------------------------------|----------------------------------------------|---------------------------------------------------------------------------------------------------------|
| **Primary focus** | Fast package, dependency and project management    | Dependency management and package publishing | Flexible project, environment and build management                                                      |
| **Uses `pyproject.toml`** | ✅                                                  | ✅                                            | ✅                                                                                                       |
| **Package installation** | ✅                                                  | ✅                                            | ✅                                                                                                       |
| **Dependency management** | ✅                                                  | ✅                                            | ✅                                                                                                       |
| **Virtual environment management** | ✅ Built in                                         | ✅ Built in                                   | ✅ Built in                                                                                              |
| **Lock file support** | ✅ `uv.lock`                                        | ✅ `poetry.lock`                              | ✅  It does not have a single, universally adopted default lock file                                    |
| **Build Python packages** | ✅                                                  | ✅                                            | ✅                                                                                                       |
| **Publish packages** | ✅                                                  | ✅                                            | ✅                                                                                                       |
| **Performance** | ⭐⭐⭐⭐⭐ Excellent - very fast (Rust implementation) | ⭐⭐⭐ Good | ⭐⭐⭐⭐ Very good                                                                                          |
| **Learning curve** | Low                                                | Low–Moderate                                 | Moderate                                                                                                |
| **Typical use** | General-purpose modern Python development          | Research and application development         | Advanced projects and library development                                                               |
| **Adoption** | Rapidly growing                                    | Mature and widely adopted | Mature, especially for library development                                                              |
| **Best suited for** | Most new Python projects                           | Existing projects and teams already using Poetry | Developers who need flexible build configurations, multiple environments and advanced release workflows |


## Packaging and distribution formats

When discussing Python packaging, it is important to distinguish between source code, distributions and packages.

### Source code

Source code contains the original project files created by the developer.

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

| Concept | What it is                                                                                     | Typical contents | Typical format | When it's used                                                                                                                      |
|---------|------------------------------------------------------------------------------------------------|------------------|----------------|-------------------------------------------------------------------------------------------------------------------------------------|
| **Source code** | The original project files created by the developer.                                           | Python modules, `pyproject.toml`, README, tests, licence, documentation and configuration files. | Project directory | By developers during software development and maintenance.                                                                          |
| **Source distribution (sdist)** | A distributable archive containing the source code and everything needed to build the package. | Source code, metadata, build configuration, documentation and other project files. | `.tar.gz` | Used by packaging tool (e.g. `uv build`) when the package needs to be built on the user's machine or when distributing source code. |
| **Wheel** | A pre-built distribution that is ready to install.                                             | Built package files and package metadata. | `.whl` | Used by package installers (e.g. `uv`, `pip`) for fast, reliable installation without rebuilding the package.                      |

## Package repositories

Once packages have been built, they are typically published to a package repository.
Package repositories provide a central location where users can discover, download and install software.

The most widely used repository for publishing Python packages is the **Python Package Index (PyPI)**, which hosts hundreds of thousands of open-source packages.
While PyPI remains the standard and primary public package repository, it is not the only option. 
For example, GitHub can also be used to distribute packages, especially useful for private or organisation-managed software. 
Developers can attach source distributions and wheel files to **GitHub Releases**, allowing users to download and install specific versions directly. 
In addition, **GitHub Packages** provides a package hosting service that enables individuals and organisations to publish and manage Python packages alongside their source code repositories. 
This can be particularly useful for private projects or software intended for use within a specific organisation or research group.

## Summary

Software packaging is a fundamental practice that helps make software easier to share, install, reuse and maintain.

We introduced the concepts behind software packaging, package structure, dependency management, packaging tools and distribution formats.
We also covered why packaging and dependency management is important for sustainable research software and reproducible research, and provided an overview of the complex Python packaging and dependency management ecosystem.

Next we will exploring modern Python packaging workflows using `uv` in a practical session.
It provides a single, modern interface for creating projects, managing dependencies, creating virtual environments and building packages. 
Its speed, simplicity and standards compliance make it an excellent choice for new Python projects, while still supporting the same packaging standards (`pyproject.toml`, wheels and source distributions) used by other modern tools.

::: keypoints

- Software packaging bundles code, metadata, dependencies and documentation into a distributable artefact.
- Packaging makes software easier to share, install, reuse and reproduce.
- Python packages are commonly distributed as source distributions (sdists) or wheels.
- Modern Python packaging is built around the `pyproject.toml` standard.
- Package managers such as `uv` simplify dependency management, environment creation and package installation.
- Lock files help create reproducible software environments by recording exact dependency versions.
- Package repositories such as PyPI provide a central location for publishing and installing software.

:::