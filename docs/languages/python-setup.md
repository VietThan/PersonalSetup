# :simple-python: Python Setup

## 1. System Wide Python with `pyenv`

If you're on Linux or Mac, there is already Python installed and you can use `python` on the command line out of the box. 
However, it's usually out of date.

![Insert XKCD here](https://imgs.xkcd.com/comics/python_environment_2x.png)

But it's not advisable to upgrade your system python because all kinds of errors might pop up. 

??? warning "Viet's Story"

    At once point, I tried to upgrade my `python` system wide on an Ubuntu machine to the latest `python3` version.

    My bash terminal died/won't start up.

    As it turns out bash was relying on `python` being symlinked to `python2.7` and I've messed up.

    Just let smarter people figure it out for you.

**Solution**: Let [pyenv][1] manages system-wide python.

1. (Optional) read the README.md in its entirety
2. Install pyenv
    1. Don't forget to go through the "Setup shell environment for pyenv"
3. Install like the 2 latest version of Python3 (might need system packages). For example:
   ```{ ..console .copy }
   pyenv install 3.10 3.11
   ```

4. Usage:

    > To select a Pyenv-installed Python as the version to use, run one of the following commands:
    > 
    > 1. `pyenv shell <version>` -- select just for current shell session
    > 2. `pyenv local <version>` -- automatically select whenever you are in the current directory (or its subdirectories)
    > 3. `pyenv global <version>` -- select globally for your user account



[1]: https://github.com/pyenv/pyenv

## 2. Single Project

### 2.1. Bare (no dependency management)
At the very least use python's standard [venv](https://docs.python.org/3/library/venv.html) and a [requirements.txt](https://realpython.com/what-is-pip/#using-requirements-files).

Standard commands:

```{ ..console }
$ mkdir new_project
$ cd new_project
$ python3.11 -m venv venv
```

Do some coding using the venv and save the packages.

```{ ..console }
$ cd new_project
$ venv/bin/python -m pip install requests
$ venv/bin/python -m pip freeze > requirements.txt
```

### 2.2. Dependencies Manager
I am familiar with [PDM](https://pdm-project.org/) and [poetry](https://python-poetry.org/). 
They themselves also rely on python's standard `venv` (mentioned above).

???+ note "Viet's Note"

    I am marginally excited about [uv](https://github.com/astral-sh/uv).

#### 2.2.1. Using the `pyproject.toml`

[PEP-518](https://peps.python.org/pep-0518/) and [PEP-621](https://peps.python.org/pep-0621/) introduces and reinforces that the `pyproject.toml` is the primary configuration file for package management.

Since then most Python tooling has moved to support configuration in this file instead of their own custom files.

So for example, don't use a [pytest.ini](https://docs.pytest.org/en/stable/reference/customize.html#pytest-ini), use [pytest with pyproject.toml](https://docs.pytest.org/en/stable/reference/customize.html#pytest-ini). Don't use a [mypy.ini](https://docs.pytest.org/en/stable/reference/customize.html#pytest-ini), use [mypy with pyproject.toml](https://mypy.readthedocs.io/en/stable/config_file.html#using-a-pyproject-toml-file)


#### 2.2.2. Local `.venv`
It is important to always have a local `.venv` per project.
This is to not cross-contaminate dependencies between different projects. And also makes it easy to package your project for pypi in the future.

If using `poetry`, enable [virtualenvs.in-project](https://python-poetry.org/docs/configuration/#virtualenvsin-project) to `true`. This would ensure the creation of a `.venv` managed by the dependency manager, containing the installed dependencies of the project.

`pdm` creates local `.venv` out of the box.

As such, in any project, you can expect the following structure:

```
.
└── my-python-project/
    ├── .venv/
    │   ├── bin/
    │   │   └── python
    │   └── lib/
    │       └── ...
    ├── src/
    │   └── my_python_project/
    │       ├── __init__.py
    │       └── main.py
    ├── tests/
    │   ├── __init__.py
    │   └── test_main.py
    ├── pyproject.toml
    ├── README.md
    └── pdm.lock_or_poetry.lock
```

## 3. Monorepo Python
Parent `VENV` with a `.helper_rc` (contains any bash commands you want). Each project has their own `.venv`.

See the example in the expand:

??? note "Viet's Note"

    ```
    .
    └── Big-MonoRepo/
        ├── Lib-A/
        │   ├── .venv/
        │   │   ├── bin/
        │   │   │   └── python
        │   │   └── lib/
        │   │       └── ...
        │   ├── src/
        │   │   └── lib_a/
        │   │       ├── __init__.py
        │   │       └── main.py
        │   ├── tests/
        │   │   ├── __init__.py
        │   │   └── test_main.py
        │   ├── pyproject.toml
        │   ├── README.md
        │   └── pdm.lock
        ├── Service-A/
        │   ├── .venv/
        │   │   ├── bin/
        │   │   │   └── python
        │   │   └── lib/
        │   │       └── ...
        │   ├── src/
        │   │   └── service_a/
        │   │       ├── __init__.py
        │   │       └── main.py
        │   ├── tests/
        │   │   ├── __init__.py
        │   │   └── test_main.py
        │   ├── pyproject.toml
        │   ├── README.md
        │   └── pdm.lock
        ├── Service-B/
        │   ├── .venv/
        │   │   ├── bin/
        │   │   │   └── python
        │   │   └── lib/
        │   │       └── ...
        │   ├── src/
        │   │   └── service_b/
        │   │       ├── __init__.py
        │   │       └── main.py
        │   ├── tests/
        │   │   ├── __init__.py
        │   │   └── test_main.py
        │   ├── pyproject.toml
        │   ├── README.md
        │   └── pdm.lock
        ├── venv/
        │   ├── bin/
        │   │   └── python
        │   └── lib/
        │       └── ...
        ├── docs/
        │   └── index.md
        ├── mkdocs.yaml
        ├── docker-compose.yaml
        ├── .gitignore
        ├── .pre-commit-config.yaml
        └── big-monorepo.code-workspace # vscode workspace
    ```

### 3.1. Packages depend on local libs

In the example above, let's say Service-A and Service-B relies on Lib-A, what to do?

1. `poetry` calls them [path dependencies](https://python-poetry.org/docs/dependency-specification/#path-dependencies). Note the `develop` attribute.
2. `pdm` calls them [local dependencies](https://pdm-project.org/latest/usage/dependency/#local-dependencies), but then put [editable dependencies](https://pdm-project.org/latest/usage/dependency/#editable-dependencies) in a different section.

### 3.2. `pre-commit` in a monorepo

Can't just use [pre-commit](https://pre-commit.com/) normally because each project should have their own 

TODO


## Docs

I like [mkdocs with material](https://squidfunk.github.io/mkdocs-material/getting-started/).