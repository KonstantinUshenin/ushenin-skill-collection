# Generic repo conventions

Technology-agnostic structural patterns for any `learn-{tech}` project. Content (lessons, APIs, docker services) must come from research on the **requested** stack.

## Naming

| Item | Pattern |
|------|---------|
| Repo directory | `learn-{tech}` (lowercase, hyphenated) |
| Examples | `examples/01_<topic>.py`, …, `NN_<topic>.py` |
| Checkpoint env var | `LEARN_CHECKPOINT=1` (same for all projects) |
| Config prefix in `.env` | derive from stack, e.g. `REDIS_URL`, `DATABASE_URL` |

## Standard files

```
learn-{tech}/
├── docker-compose.yml      # optional
├── requirements.txt          # or package.json / go.mod / Cargo.toml
├── .env.example
├── .gitignore
├── data/                     # fixtures when ingest lessons need them
├── lib/
│   ├── config.py
│   └── helpers.py
├── examples/
├── .vscode/launch.json
├── README.md                 # quick start only
└── GUIDELINE.md              # full curriculum + theory
```

## `lib/helpers.py` contract

All projects implement the same debugging interface:

- `print_header(title, step=None)`
- `print_records(records, label="Results")`
- `checkpoint(name, **variables)` — respects `LEARN_CHECKPOINT`

Optional: `print_summary(...)` when the client SDK exposes operation counters.

## Example script skeleton

```python
#!/usr/bin/env python3
"""Example NN: {one-line purpose}."""

import sys
from pathlib import Path

sys.path.insert(0, str(Path(__file__).resolve().parent.parent))

from lib.config import get_client  # name varies by stack
from lib.helpers import checkpoint, print_header, print_records


def main() -> None:
    print_header("Example NN — {Title}", "{subtitle}")

    params = {...}
    print("\nParameters:")
    for k, v in params.items():
        print(f"  {k}: {v}")

    checkpoint("before_operation", params=params)

    client = get_client()
    result = client.do_something(**params)  # stack-specific

    checkpoint("after_operation", result=result)
    print_records(result, "Operation result")

    print("\nDone. Run example NN+1 next.")


if __name__ == "__main__":
    main()
```

## README vs GUIDELINE split

| File | Contains |
|------|----------|
| `README.md` | Setup, example table, checkpoints, reset, links |
| `GUIDELINE.md` | Per-lesson purpose, theory (LaTeX), run, observe, study checkpoint |

Never duplicate full lesson theory in README.

## `.vscode/launch.json` pattern

Set `LEARN_CHECKPOINT=1` in `env` for debug configurations. Provide:

- **Example (current file)** — generic entry
- One named config per capstone or mid-curriculum lesson (optional)

## Curriculum sizing

| Lessons | When |
|---------|------|
| 8–10 | Focused stack, narrow goal |
| 10–12 | Broad platform + capstone |
| >12 | Avoid unless user explicitly wants depth |

Modules I–V are **roles**, not fixed topics. Fill them from official learning paths for the requested technology.

## Credential lessons

Split the curriculum so early lessons need no external credentials. Before any paid-API capstone:

1. Add a lesson using mocks, fakes, or local-only mode.
2. Document required env vars in `.env.example` with comments.
3. Exit with a clear message naming the missing variable if unset.

## Verification checklist

```bash
# adapt compile/run commands to language
python -m py_compile examples/*.py lib/*.py
docker compose up -d          # if applicable
python examples/01_*.py
# run further examples while credentials allow
```

Fix failures before delivering the repo.
