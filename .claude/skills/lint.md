---
description: Run linting and formatting checks
command: /lint
---

Run ruff linting and format checking:

```bash
uv run ruff check src/ tests/
uv run ruff format --check src/ tests/
```

If there are issues, fix them:

```bash
uv run ruff check --fix src/ tests/
uv run ruff format src/ tests/
```
