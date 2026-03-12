# fixed-point

* Fixed-Point is a pytest plugin designed to record and replay deterministic function calls, making your tests stable and reproducible.
* Flaky tests are the bane of every developer's existence. I prefer not to rely on retries, mocks that drift from reality, or fragile test setups to keep things green.
* To tackle this problem, Fixed-Point captures real function outputs and replays them faithfully, so your tests always land on the same answer — a fixed point.
* It boasts simplicity, making it exceptionally easy to adopt in any pytest project.
* Fixed-Point serves as a specialized testing tool, particularly tailored for pinning down the behavior of functions that talk to external services. If you encounter any cases not covered by the plugin, please don't hesitate to create an issue for further assistance.

## Installation

You can install pytest-fixedpoint using pip, the Python package manager:

```
pip install pytest-fixedpoint
```

## Getting Started

To start using fixed-point in your project, simply decorate the functions you want to pin down with `@recordable`:

```python
from fixedpoint import recordable

@recordable
def call_external_api(query):
    return external_service.search(query)

def test_search(fixedpoint):
    result = call_external_api("hello")
    assert result == expected
```

The first time you run with `--fixedpoint=record_once`, it captures real outputs. Every subsequent run replays them — no network calls, no flakiness.

## Modes

Fixed-Point supports 4 operational modes via the `--fixedpoint` CLI option:

| Mode | Behavior |
|------|----------|
| `off` | Default. Plugin is disabled, functions execute normally. |
| `record_once` | Record new calls, replay existing ones. Ideal for initial setup. |
| `replay` | Replay only. Fails with `CassetteNotFoundError` if no recording exists. Perfect for CI. |
| `rewrite` | Always re-execute and overwrite recordings. Use when the real behavior has changed. |

```bash
# Record cassettes for the first time
pytest tests/ --fixedpoint=record_once

# Replay from cassettes (safe for CI)
pytest tests/ --fixedpoint=replay

# Force re-record everything
pytest tests/ --fixedpoint=rewrite

# Disable (default)
pytest tests/
```

## Cassettes

Recordings are stored as human-readable YAML files in `tests/cassettes/`:

```
tests/cassettes/
  test_module_name/
    test_function_name.yaml
```

A cassette file looks like:

```yaml
version: 1
calls:
  myapp.api.fetch_user:
    - args: [42]
      kwargs: {}
      return: {"name": "Alice", "age": 30}
```

Cassettes are committed to your repo so the whole team replays the same results.

## Supported Types

The serializer handles these types out of the box:

| Type | Serialized As |
|------|---------------|
| `None`, `bool`, `int`, `float`, `str` | Direct values |
| `bytes` | `{"__bytes__": "<base64>"}` |
| `tuple` | `{"__tuple__": [...]}` |
| `set` | `{"__set__": [...]}` |
| `list`, `dict` | Direct JSON arrays/objects |
| `dataclass` | `{"__dataclass__": "module.Class", ...fields}` |

## Error Handling

Fixed-Point raises clear errors when things don't match:

| Exception | When |
|-----------|------|
| `CassetteNotFoundError` | No cassette file found in `replay` mode |
| `CassetteMismatchError` | Recorded args don't match actual call, or too many calls |
| `SerializationError` | Unsupported type passed to a `@recordable` function |

When you see a `CassetteMismatchError`, the error message includes a hint:

> Run with `--fixedpoint=rewrite` to re-record

## Why fixed-point?

* Today I want to dedicate something to my beloved wife, and I'm eager to make it meaningful.
* I've created this open-source project called "Fixed-Point," because she is my fixed point — always cute, always kind, and forever constant no matter how chaotic the world gets.
* In mathematics, a fixed point is a value that remains unchanged by a function. She is exactly that — the one thing in my life that is steady, warm, and unchanging.
* The package is named "fixed-point" because, just like in math, some things converge to a beautiful, stable truth. She is mine.
* This project is designed to bring stability to tests the same way she brings stability to my life — quietly, reliably, and with grace.
* With this, I hope to bring a smile to her face, and maybe to yours too.
