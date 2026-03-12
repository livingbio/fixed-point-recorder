# fixed-point

Pytest plugin for recording and replaying deterministic function calls. Decorate functions with `@recordable` to capture real outputs and replay them in tests.

## Commands

- Run tests: `uv run pytest tests/ -v`
- Lint: `uv run ruff check src/ tests/`
- Format: `uv run ruff format src/ tests/`

## Architecture

Source code lives in `src/fixedpoint/`:

- `_registry.py` — `@recordable` decorator, function registry, active interceptor management
- `_interceptor.py` — Intercepts decorated calls to record or replay (sync and async)
- `_cassette.py` — Loads/saves YAML cassette files with recorded call data
- `_serializer.py` — Serializes/deserializes args and return values (primitives, bytes, tuple, set, dataclass)
- `_types.py` — `CallRecord` and `CassetteData` data structures
- `plugin.py` — Pytest plugin: `fixedpoint` fixture, `--fixedpoint` CLI option, cassette lifecycle

## Code Style

- Formatter/linter: ruff (line length 88, target Python 3.10)
- Lint rules: E, W, F, I, UP, B, SIM
- Pre-commit hooks: ruff check --fix, ruff format
