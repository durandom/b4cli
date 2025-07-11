# b4cli - Modern Python CLI Application Template

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![License: CC BY-NC 4.0](https://img.shields.io/badge/License-CC_BY--NC_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc/4.0/)
[![Code style: Ruff](https://img.shields.io/badge/code%20style-ruff-000000.svg)](https://github.com/astral-sh/ruff)

A modern Python CLI application template built with best practices, featuring Typer, Rich, Pydantic, and comprehensive development tools.

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Quick Start](#quick-start)
- [Template Usage](#template-usage)
- [Development Setup](#development-setup)
- [Project Structure](#project-structure)
- [CLI Architecture](#cli-architecture)
- [Testing](#testing)
- [Customization Guide](#customization-guide)
- [🚀 Template Setup Commands](#-template-setup-commands)
- [Contributing](#contributing)
- [License](#license)

## Overview

b4cli is a production-ready Python CLI application template that provides a solid foundation for building modern command-line tools. It incorporates industry best practices, modern Python features, and a comprehensive development environment.

## Key Features

- **Modern CLI Framework**: Built with Typer and Rich for beautiful, interactive command-line interfaces
- **Type-safe Configuration**: Pydantic-based settings with environment variable support
- **Advanced Logging**: Configurable Loguru logging with multiple formats and outputs
- **Modular Architecture**: Command registry pattern for easy extension and organization
- **Comprehensive Testing**: Unit, integration, and end-to-end test examples with pytest
- **Code Quality Tools**: Ruff for linting/formatting, mypy for type checking, pre-commit hooks
- **Development Tools**: UV for dependency management, coverage reporting, documentation generation
- **Production Ready**: Error handling, logging, configuration management, and testing infrastructure

## Quick Start

### Using This Template

Choose one of these methods to get the template files:

#### Option 1: Download Archive (Recommended)
```bash
# Download and extract template files without git history
curl -L https://github.com/your-org/b4cli/archive/main.tar.gz | tar -xz
mv b4cli-main my-new-cli
cd my-new-cli

# Initialize your own git repository
git init
```

#### Option 2: Use degit (Clean Template)
```bash
# Install degit if you don't have it
npm install -g degit

# Download template files without git history
degit your-org/b4cli my-new-cli
cd my-new-cli

# Initialize your own git repository
git init
```

#### Option 3: GitHub Template (Web Interface)
1. Go to [github.com/your-org/b4cli](https://github.com/your-org/b4cli)
2. Click the **"Use this template"** button
3. Create a new repository from the template
4. Clone your new repository:
   ```bash
   git clone https://github.com/your-username/your-new-cli.git
   cd your-new-cli
   ```

#### Option 4: Git Archive (Command Line)
```bash
# Download just the files without git history
git archive --remote=https://github.com/your-org/b4cli.git main | tar -x
# Or if the above doesn't work:
git clone --depth 1 https://github.com/your-org/b4cli.git my-new-cli
cd my-new-cli
rm -rf .git
git init
```

2. **Install dependencies**:
   ```bash
   uv sync --group dev
   ```

3. **Test the template**:
   ```bash
   # Run the CLI
   uv run b4cli --help

   # Try example commands
   uv run b4cli example hello --name "Your Name"
   uv run b4cli example config

   # Run tests
   uv run pytest
   ```

4. **Customize for your project** (see [Customization Guide](#customization-guide))

## Template Usage

### Rename the Project

To customize this template for your own CLI application:

1. **Update package name** in `pyproject.toml`:
   ```toml
   name = "your-cli-name"
   description = "Your CLI description"
   ```

2. **Rename the source directory**:
   ```bash
   mv src/b4cli src/your_cli_name
   ```

3. **Update imports** throughout the codebase:
   ```bash
   find . -name "*.py" -exec sed -i 's/b4cli/your_cli_name/g' {} +
   ```

4. **Update script entry point** in `pyproject.toml`:
   ```toml
   [project.scripts]
   your-cli = "your_cli_name:main"
   ```

5. **Customize CLI help text** in `src/your_cli_name/cli/app.py` and `base.py`

### Add New Commands

1. **Create a new command group** in `src/b4cli/commands.py` or a new file:
   ```python
   class YourCommands(CommandGroup):
       def create_app(self) -> typer.Typer:
           app = typer.Typer(help="Your command group")

           @app.command()
           def your_command():
               """Your command description."""
               # Implementation here

           return app

   # Register the command group
   command_registry.register("your-group", YourCommands)
   ```

2. **Import the module** in `src/b4cli/cli/app.py` to register commands

## Development Setup

### Prerequisites

- Python 3.12 or higher
- [UV](https://docs.astral.sh/uv/) (recommended) or pip

### Installation

```bash
# After downloading template files (see Quick Start above)
cd your-cli-directory

# Install dependencies including development tools
uv sync --group dev

# Or with pip
pip install -e ".[dev]"
```

### Development Commands

```bash
# Run the CLI
uv run b4cli

# Run with debug logging
uv run b4cli -v example hello

# Run with trace logging
uv run b4cli -vv example config
```

## Project Structure

```
b4cli/
├── src/b4cli/              # Main package
│   ├── __init__.py         # Package initialization and main entry point
│   ├── settings.py         # Pydantic settings and configuration
│   ├── commands.py         # Example command implementations
│   └── cli/                # CLI framework components
│       ├── __init__.py
│       ├── app.py          # Main CLI application and global options
│       ├── base.py         # Base classes and command registry
│       └── CLAUDE.md       # CLI-specific documentation
├── tests/                  # Test suite
│   ├── conftest.py         # Pytest configuration and fixtures
│   ├── unit/               # Unit tests
│   ├── integration/        # Integration tests (empty in template)
│   └── e2e/                # End-to-end CLI tests
├── logs/                   # Log files (auto-created)
├── docs/                   # Documentation
├── pyproject.toml          # Project configuration and dependencies
├── Makefile                # Development shortcuts
├── CLAUDE.md               # AI assistant instructions
└── README.md               # This file
```

## CLI Architecture

### Command Registry Pattern

The template uses a modular command registry pattern:

```python
# 1. Create command group
class MyCommands(CommandGroup):
    def create_app(self) -> typer.Typer:
        app = typer.Typer(help="My commands")
        # Add commands to app
        return app

# 2. Register command group
command_registry.register("my-group", MyCommands)

# 3. Commands automatically available as: cli my-group command
```

### Configuration Management

- **Pydantic Settings**: Type-safe configuration with validation
- **Environment Variables**: Automatic loading with `LOG_LEVEL`, `LOG_FORMAT`, etc.
- **Hierarchical Config**: Support for nested configuration objects
- **CLI Override**: Command-line options override environment variables

### Logging System

- **Loguru Integration**: Modern, structured logging
- **Multiple Formats**: Pretty (colored) and JSON formats
- **File and Console**: Simultaneous output to both
- **Log Rotation**: Automatic file rotation and retention
- **Testing Integration**: Automatic log file creation during tests

## Testing

### Test Structure

- **Unit Tests** (`tests/unit/`): Test individual components
- **Integration Tests** (`tests/integration/`): Test component interactions
- **End-to-End Tests** (`tests/e2e/`): Test complete CLI workflows

### Running Tests

```bash
# Run all tests
uv run pytest

# Run with coverage
uv run pytest --cov=src --cov-report=html --cov-report=term-missing

# Run specific test types
uv run pytest -m unit          # Unit tests only
uv run pytest -m integration   # Integration tests only
uv run pytest -m e2e          # End-to-end tests only
uv run pytest -m "not slow"   # Skip slow tests
```

### Test Examples

The template includes examples for:
- **CLI Testing**: Using `typer.testing.CliRunner`
- **Settings Testing**: Configuration and environment variables
- **Command Testing**: Individual command functionality
- **Error Handling**: Exception and error case testing

## Customization Guide

### 1. Basic Customization

Update these files for basic customization:

- `pyproject.toml`: Project metadata and dependencies
- `src/b4cli/__init__.py`: Package information and version
- `src/b4cli/settings.py`: Application settings and configuration
- `src/b4cli/cli/app.py`: CLI help text and global options

### 2. Adding Dependencies

```bash
# Add runtime dependency
uv add requests

# Add development dependency
uv add --group dev pytest-mock

# Update requirements
uv sync
```

### 3. Custom Configuration

Extend the `Settings` class in `settings.py`:

```python
class Settings(BaseSettings):
    # Your custom settings
    api_token: str = Field(..., description="API token")
    timeout: int = Field(default=30, description="Request timeout")
```

### 4. Advanced Logging

Customize logging in `cli/base.py` or create logging presets in `settings.py`.

### 5. Adding New Command Groups

1. Create command group class inheriting from `CommandGroup`
2. Implement `create_app()` method with your commands
3. Register with `command_registry.register()`
4. Import the module in `cli/app.py`

## Code Quality

### Linting and Formatting

```bash
# Check code style
uv run ruff check

# Format code
uv run ruff format

# Type checking
uv run mypy src/

# All checks
uv run pre-commit run --all-files
```

### Code Standards

- **Line Length**: 120 characters maximum
- **Type Hints**: Required for all public functions
- **Docstrings**: Google Style for all public APIs
- **Import Sorting**: Automatic with Ruff
- **Error Handling**: Explicit exception handling with logging

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes following the coding standards
4. Add tests for new functionality
5. Ensure all tests pass (`uv run pytest`)
6. Run code quality checks (`uv run ruff check && uv run ruff format`)
7. Update documentation as needed
8. Commit your changes (`git commit -m 'Add amazing feature'`)
9. Push to the branch (`git push origin feature/amazing-feature`)
10. Open a Pull Request

## License

This project is licensed under the Creative Commons Attribution-NonCommercial 4.0 International License.

- ✅ **Personal Use**: Free for personal projects
- ✅ **Educational Use**: Free for learning and teaching
- ✅ **Research Use**: Free for academic research
- ✅ **Sharing**: You can share and adapt the code
- ✅ **Attribution**: You must give appropriate credit

- ❌ **Commercial Use**: Prohibited without explicit permission
- ❌ **Selling**: Cannot sell the software or derivative works

See the [LICENSE](LICENSE) file for details or visit [https://creativecommons.org/licenses/by-nc/4.0/](https://creativecommons.org/licenses/by-nc/4.0/).

## 🚀 Template Setup Commands

### Quick Template Setup Script

After downloading the template files, rename everything with one simple command:

```bash
# Set your CLI name
NEW_NAME="mycli"

# Download and extract the template, then rename it
curl -L https://github.com/durandom/b4cli/archive/main.tar.gz | tar -xz
mv b4cli-main $NEW_NAME && cd $NEW_NAME && git init

# Rename everything in 3 commands
find . -name "*b4cli*" | while read f; do mv "$f" "${f//b4cli/$NEW_NAME}"; done
find . -type f \( -name "*.py" -o -name "*.md" -o -name "*.toml" \) -exec sed -i '' "s/b4cli/$NEW_NAME/g" {} +
uv sync --group dev
```

---

**b4cli** - A modern Python CLI template built with best practices ⚡
