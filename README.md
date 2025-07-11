# b4cli - Modern Python CLI Application Template

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![License: CC BY-NC 4.0](https://img.shields.io/badge/License-CC_BY--NC_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc/4.0/)
[![Code style: Ruff](https://img.shields.io/badge/code%20style-ruff-000000.svg)](https://github.com/astral-sh/ruff)

Modern Python CLI application template built with Typer, Rich, Pydantic, and comprehensive development tools.

## 🚀 Template Setup Commands

```bash
# Set your CLI name
NEW_NAME="mycli"

# Download and extract the template, then rename it
curl -L https://github.com/durandom/b4cli/archive/main.tar.gz | tar -xz
mv b4cli-main $NEW_NAME && cd $NEW_NAME

# Rename everything in 3 commands
find . -name "*b4cli*" | while read f; do mv "$f" "${f//b4cli/$NEW_NAME}"; done
find . -type f \( -name "*.py" -o -name "*.md" -o -name "*.toml" \) -exec sed -i '' "s/b4cli/$NEW_NAME/g" {} +
uv sync --group dev && uv run $NEW_NAME --help
```

## Essential Commands

```bash
# Install dependencies
uv sync --group dev

# Run CLI
uv run b4cli

# Run tests
uv run pytest

# Code quality
uv run ruff check && uv run ruff format

# Run with logging
uv run b4cli -v example hello
```

## License

Creative Commons Attribution-NonCommercial 4.0 - Free for personal/educational use, no commercial use.
