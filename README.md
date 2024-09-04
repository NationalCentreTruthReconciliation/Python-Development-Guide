# NCTR Python Style Guide

Python is the main programming language used at the NCTR for software development. The following document outlines the general rules and tools for developing Python code at the NCTR.

## Software

Software development is done using the [Visual Studio Code editor](https://code.visualstudio.com/). You should use the latest Python version unless you are specifically targeting an older version of Python, for example for running code on a legacy server.

The following extensions are required in VS Code for Python development:

- [Ruff](https://marketplace.visualstudio.com/items?itemName=charliermarsh.ruff), a fast, Rust-based language server with linting and formatting built in.
  - This also installs the Python extension (which also installs a Python debugger and the Pylance language server)
- [Even Better TOML](https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml) – For reading TOML files like pyproject.toml and ruff.toml.

## Setup

Copy the NCTR's `ruff.toml` file from this repository to the root of any Python project you work on. The Ruff extension will read this file and enable linting on your project. This file is subject to change.

Access the settings in Visual Studio Code with `CRTL+,`. Open the JSON settings file  and make sure these settings are configured like this:

```json
{
    "[python]": {
        "editor.formatOnSave": true,
        "editor.defaultFormatter": "charliermarsh.ruff",
        "editor.rulers": [
            99
        ]
    },
    "ruff.nativeServer": "on",
    "python.analysis.typeCheckingMode": "standard",
    "editor.formatOnSave": true
}
```

You may have other settings in your `settings.json` beside these settings, just be sure these ones are configured as noted above.

## Formatting

The Ruff language server handles code formatting and will auto-format your code every time you press Save.

Ruff can sometimes auto-address issues with your code, which are denoted by yellow-underlined lines. To tell Ruff to auto-fix your code, you can run the **Ruff: Fix all auto-fixable problems** task. Press `CTRL+Shift+P` and search for “Ruff” to find this task and click it to run.

## Other Miscellaneous Rules

For any project larger than a throwaway script, use the `logging` module over calling `print()`. Use [dictConfig](https://docs.python.org/3/library/logging.config.html#logging.config.dictConfig) (preferred method) or [basicConfig](https://docs.python.org/3/library/logging.html#logging.basicConfig) rather than instantiating loggers directly.

Do not use [the match statement](https://peps.python.org/pep-0622/#the-match-statement). `match` is a relatively new syntax element and is not compatible with versions of Python that are still supported.

## Docstrings

Docstrings of all types follow [Google’s style guidelines for comments and docstrings](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings). Ruff will enforce these rules most of the time.

## Type Annotations

Developers must use [Python’s type annotation system](https://docs.python.org/3/library/typing.html) to specify the return type and argument type for every function. Because standard type checking is turned on in the VS Code settings, these type annotations will be required.

Here is an example of a type-annotated function (with a proper docstring):

```python
from typing import Optional, Union


def my_function(arg_1: Optional[str] = None) -> Union[int, str]:
    """Returns 0 if arg_1 is None, otherwise returns an empty string.

    Args:
        arg_1: An optional string argument

    Returns:
        0 if arg_1 is None, an empty string otherwise.
    """
    if arg_1 is None:
        return 0
    return ""
```

Note that Python does not *enforce* types when you run code. The reason we include types is to be able to fix type errors while we’re writing code so that they don’t come up when running the code.

## Documentation

For small projects, Markdown files (.md) like this README are sufficient for documenting these mandatory sections:

- Introduction
- Installation
- Usage
- Testing

For larger projects, use [Sphinx](https://www.sphinx-doc.org/en/master/). Or for internal projects, you can make a [GitLab Wiki repository](https://docs.gitlab.com/ee/user/project/wiki/). Note that the [napoleon extension](https://www.sphinx-doc.org/en/master/usage/extensions/napoleon.html) will need to be installed in Sphinx to be able to interpret Google-style docstrings.

## Dependencies and Installation

Developers should create a [pyproject.toml configuration file](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/) for each project. This file should be used for specifying dependencies instead of requirements.txt or setup.py or any other type of file.

## Testing

[Pytest](https://docs.pytest.org/en/stable/) should be used for unit testing. For Django projects, Django’s own test library can be used instead of pytest.
