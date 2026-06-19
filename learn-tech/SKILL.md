---
name: learn-tech
description: Scaffolds a hands-on learning repository for a user-specified technology — incremental examples, Docker/runtime infrastructure, shared helpers with debug checkpoints, README, and PhD-level GUIDELINE.md. Use when the user wants to learn a technology, asks for educational examples, a training curriculum, learn-{tech} repo, or GUIDELINE.md for self-study.
---

# Learn Tech

Build a **runnable, incremental curriculum repo** from whatever technology the user names, plus an optional capstone goal.

## Discovery (ask if missing)

| Input | Required | Default |
|-------|----------|---------|
| Technology | Yes | — |
| End goal / capstone topic | No | deepest practical topic for that stack |
| Audience level | No | graduate / PhD (formal theory + LaTeX) |
| Language | No | Python |
| Project directory | No | `learn-{tech}` in workspace |

Confirm: repo name, runtime requirements (Docker vs local install), and external credentials the capstone may need.

## Workflow

Copy and track progress:

```
- [ ] Phase 1: Research & curriculum design
- [ ] Phase 2: Infrastructure scaffold
- [ ] Phase 3: Shared library (config + helpers + checkpoints)
- [ ] Phase 4: Numbered examples (01 … N)
- [ ] Phase 5: README.md (quick start)
- [ ] Phase 6: GUIDELINE.md (full training manual)
- [ ] Phase 7: Verify — syntax-check, run lesson 01, run full chain if feasible
```

---

## Phase 1: Research & curriculum design

1. **Web search** the technology's official docs, getting-started guides, and a realistic path to the user's stated goal.
2. **Design 8–12 sequential lessons** grouped into 3–5 modules. Derive module topics from the technology — do not reuse a fixed syllabus from another stack.

   | Module | Typical role (adapt to stack) |
   |--------|--------------------------------|
   | I | Connect, hello-world, core primitives |
   | II | Primary operations / API / query patterns |
   | III | Persistence, schema, configuration, bulk data |
   | IV | Advanced or performance-related features |
   | V | Capstone integrating the stated end goal |

3. Each lesson must:
   - Build on prior lessons (shared state or idempotent writes).
   - Be runnable as a **standalone script** (`examples/NN_<topic>.py` or equivalent).
   - Have one clear learning objective tied to that technology.

4. Note which lessons need external services, credentials, or paid APIs; mark them in a table.

---

## Phase 2: Infrastructure scaffold

Create minimum viable repo layout:

```
learn-{tech}/
├── docker-compose.yml      # only if the stack needs a local service
├── requirements.txt        # or package.json / go.mod / Cargo.toml
├── .env.example
├── .gitignore                # .env, .venv, __pycache__
├── data/                     # sample fixtures for ingest lessons
├── lib/
│   ├── config.py             # env-based client/settings
│   └── helpers.py            # print_header, print_records, checkpoint
├── examples/
│   └── 01_*.py … NN_*.py
├── .vscode/launch.json       # debug configs with CHECKPOINT=1
├── README.md
└── GUIDELINE.md
```

**Runtime:** use official images/binaries for the requested technology; expose documented ports; mount `./data` when bulk-import lessons need it; document credentials in `.env.example`.

**Dependencies:** pin loosely; include only what the examples use.

---

## Phase 3: Shared library

### `lib/config.py`

- Load `.env` via `python-dotenv` (or idiomatic equivalent for the chosen language).
- Export connection constants and a factory (`get_client()`, `get_driver()`, etc.) appropriate to the stack.

### `lib/helpers.py` — required utilities

| Function | Purpose |
|----------|---------|
| `print_header(title, step=None)` | Visual lesson banner |
| `print_records(records, label)` | Pretty-print structured results |
| `print_summary(...)` | Stats/counters when the SDK provides them (optional) |
| `checkpoint(name, **variables)` | Print state; call `breakpoint()` when `LEARN_CHECKPOINT=1` |

Use a **single generic** checkpoint env var for all projects:

```python
CHECKPOINT_ENV = "LEARN_CHECKPOINT"

def checkpoint(name: str, **variables: Any) -> None:
    print(f"\n>>> CHECKPOINT: {name}")
    for key, value in variables.items():
        print(f"    {key} = {format_value(value)}")
    if os.getenv(CHECKPOINT_ENV, "0") == "1":
        print(f"    [{CHECKPOINT_ENV}=1 → breakpoint()]")
        breakpoint()
```

Document `LEARN_CHECKPOINT=1` in `.env.example`, README, and GUIDELINE.

Every example script:

```python
sys.path.insert(0, str(Path(__file__).resolve().parent.parent))
from lib.config import ...
from lib.helpers import checkpoint, print_header, print_records
```

---

## Phase 4: Numbered examples

For each `examples/NN_<name>.py`:

1. Module docstring: one-line purpose.
2. `print_header("Example NN — …", "short subtitle")`.
3. **Verbose prints** before/after every operation (params, counts, rows, status codes).
4. **`checkpoint()` at 2–4 key steps** (before connect, after mutation, after query/response).
5. End with `"Done. Run example NN+1 next."` (except last lesson).
6. Guard optional credentials: clear error naming the env var + which earlier lessons work without it.

When the capstone needs a paid or external API, add a **mock/fake lesson** immediately before it so the user can study mechanics without credentials.

**Quality bar:** run examples sequentially on a fresh environment without manual fixes.

---

## Phase 5: README.md

Keep short (~100 lines). Include:

- One-time setup (install, docker if any, venv, `.env`)
- Table: `# | script | topic | credentials?`
- Checkpoint usage (`LEARN_CHECKPOINT=1` + debugger)
- Reset/clean-state command for that stack
- Link to `GUIDELINE.md`
- 2–3 official doc links for the requested technology

Do **not** duplicate per-lesson theory in README — that lives in GUIDELINE.

---

## Phase 6: GUIDELINE.md

PhD-level training manual. Follow [guideline-template.md](guideline-template.md). See [repo-layout.md](repo-layout.md) for structural conventions.

**Required sections:**

- §0 Prerequisites & notation (formal background; LaTeX where it clarifies concepts **for this technology**)
- §0.2 Environment setup (commands and connection table for **this stack**)
- §0.3 How to run any lesson (+ `LEARN_CHECKPOINT` protocol)
- §0.4 Curriculum map (modules table)

**Per lesson (repeat for 01 … N):**

| Block | Content |
|-------|---------|
| Metadata table | Script path, prerequisites, credentials? |
| Purpose | One paragraph — competence gained |
| Theory | Formal definitions; LaTeX for models and operators relevant to **this lesson** |
| How to run | Exact command |
| What to observe | Expected output, counters, side effects |
| Study checkpoint | What to inspect at `breakpoint()` |

**Appendices:** checkpoint protocol, optional exercises, capstone architecture diagram (mermaid) + formula if applicable, references (official docs + seminal papers for the domain).

Choose LaTeX notation from the technology's domain (e.g. algebras for databases, latency models for queues, type rules for languages) — do not assume graphs, vectors, or RAG unless the user asked for them.

---

## Phase 7: Verification

1. Syntax-check all example and lib files.
2. Start required infrastructure (`docker compose up -d`, local daemon, etc.).
3. Run `examples/01_*`; fix connection issues.
4. Run subsequent examples as far as credentials allow.
5. Confirm `.vscode/launch.json` sets `LEARN_CHECKPOINT=1`.

---

## Decision rules

| Situation | Action |
|-----------|--------|
| Technology needs no server | Omit docker-compose; document install-only setup |
| Language is not Python | Same layout spirit; use idiomatic config + `examples/NN_*.{ts,go,rs}` |
| User wants undergraduate level | Reduce LaTeX; keep structure |
| User wants PhD level (default) | Formal notation, references, failure-mode discussion |
| Capstone needs paid API | Provide a no-credential lesson immediately before using mocks/fakes |
| User names only the technology, no goal | Research and propose a sensible capstone; confirm if ambiguous |

---

## Anti-patterns

- Do not hardcode a syllabus from a previous project (Neo4j, Kafka, Redis, etc.) — derive from research.
- Do not create one monolithic script — one file per lesson.
- Do not skip checkpoints or sparse prints — user debugs at breakpoints.
- Do not put full theory in README — use GUIDELINE.md.
- Do not create 20 lessons when 10 focused ones suffice.
- Do not hardcode secrets; use `.env.example` only.

---

## Additional resources

- Per-lesson GUIDELINE template: [guideline-template.md](guideline-template.md)
- Generic repo conventions: [repo-layout.md](repo-layout.md)
