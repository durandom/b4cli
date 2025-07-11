### CLI Structure
The CLI uses a modular command registry pattern:

- `src/b4cli/cli/base.py` - Base classes and command registry
- `src/b4cli/cli/app.py` - Main CLI application and command registration
- Commands are organized in separate modules and registered via `CommandRegistry`

### Configuration Management
- `src/b4cli/settings.py` - Centralized settings using Pydantic
- Supports environment variables with nested delimiter `__` (e.g., `LOGGING__LEVEL=DEBUG`)
- Settings include logging config, API config, and CLI options
- Environment variables automatically override file-based configuration

### Logging Setup
- Configured via `setup_logging()` in `src/b4cli/cli/base.py`
- Supports both "pretty" (colored) and "json" formats
- Can log to console and/or file with rotation
- Logging level and format controlled by settings
