# GUIDELINE.md Template

Use this structure when writing `GUIDELINE.md`. Replace placeholders with content derived from the **requested technology** and researched curriculum.

Placeholders: `{TECH}`, `{tech}`, `{GOAL}`, `{N}`, `{RUN_CMD}`, `{VERIFY_TOOL}`, `{RESET_CMD}`.

---

```markdown
# Training Guideline: {TECH} — from foundations to {GOAL}

**Audience:** self-directed study at graduate / PhD level
**Goal:** {one sentence formal goal for this stack}
**Repository:** `learn-{tech}` — {N} incremental lessons (`examples/01` … `examples/{N}`).

---

## 0. Prerequisites and notation

### 0.1 Mathematical / conceptual background

List formal prerequisites with LaTeX where useful — chosen for **this technology**, e.g.:

- Data structures, algebras, or models the stack implements
- Concurrency, ordering, or consistency models if relevant
- Domain-specific notation (types, protocols, query languages)

Brief mapping: abstract concept → how {TECH} implements it.

### 0.2 Environment (one-time setup)

\`\`\`bash
{setup commands for this stack}
\`\`\`

| Resource | Value |
|----------|-------|
| ... | ... |

Note credentials or API keys for lessons X–Y.

### 0.3 How to run any lesson

**Standard run:**
\`\`\`bash
{RUN_CMD} examples/NN_<name>.py
\`\`\`

**With checkpoints:**
\`\`\`bash
LEARN_CHECKPOINT=1 {RUN_CMD} examples/NN_<name>.py
\`\`\`

Or set `LEARN_CHECKPOINT=1` in `.env`. At each `checkpoint(name, **vars)` the script prints state and calls `breakpoint()` when enabled.

**Verify externally:** {VERIFY_TOOL — CLI, dashboard, REPL, admin UI, etc.}

**Reset state:**
\`\`\`bash
{RESET_CMD}
\`\`\`

### 0.4 Curriculum map

| Module | Lessons | Theme |
|--------|---------|-------|
| I | 01–… | … |
| … | … | … |

Lessons are sequential.

---

## Module I — {Title}

### Lesson 01 — {Title}

| | |
|---|---|
| **Script** | `examples/01_<name>.py` |
| **Requires** | {prerequisites} |

#### Purpose

{One paragraph: what competence the learner gains.}

#### Theory

{Formal definitions for this lesson. Use LaTeX when it adds precision:}

$$
\text{operation}(x) = \ldots
$$

Explain how the example instantiates the theory.

#### How to run

\`\`\`bash
{RUN_CMD} examples/01_<name>.py
\`\`\`

#### What to observe

- Expected prints, counters, state changes specific to this lesson

#### Study checkpoint

At `{checkpoint_name}`, inspect `{variables}` and verify via {VERIFY_TOOL}.

---

{Repeat per-lesson block for 02 … N}

---

## Appendix A — Checkpoint protocol

| Mode | Config | Behavior |
|------|--------|----------|
| Observe only | default | Print variables, continue |
| Interactive | `LEARN_CHECKPOINT=1` | Print + `breakpoint()` |
| IDE debugger | launch.json with `LEARN_CHECKPOINT=1` | Same as interactive |

Recommended workflow: read lesson → skim script → run → re-run with checkpoint → verify externally.

## Appendix B — Suggested exercises

| Lesson | Exercise |
|--------|----------|

## Appendix C — End-to-end architecture

\`\`\`mermaid
flowchart LR
  ...
\`\`\`

Capstone summary in prose and LaTeX if the domain warrants it.

## Appendix D — References

1. Official {TECH} documentation
2. Seminal papers or specs relevant to the capstone domain

---

*Document version: aligned with examples 01–{N}.*
\`\`\`

---

## Per-lesson theory checklist

Include at least one of:

- Formal definition or notation for the concept taught
- Mapping to a known model (algebra, protocol, type system, etc.)
- Complexity, consistency, or failure-mode note
- Comparison table (naive vs idiomatic usage in this stack)

## LaTeX patterns (use when relevant to the requested technology)

Pick notation that matches the domain — do not copy formulas from unrelated stacks.

| Domain | Example notation |
|--------|------------------|
| State / transition | $ \delta : S \times \Sigma \to S $ |
| Query / algebra | $ \sigma_\phi(R),\; \pi_A(R),\; R \bowtie S $ |
| Distributed / queues | ordering, delivery guarantees, latency bounds |
| Types | $ \Gamma \vdash e : \tau $ |
| ML / retrieval (only if capstone needs it) | similarity metrics, conditional generation |
| Idempotent updates | $ f(f(s, \delta), \delta) = f(s, \delta) $ |

Research the technology first; derive notation from its official model, not from a template project.

## Math notation (Obsidian + GitHub)

Use **dollar delimiters only** — do not use `\(...\)` or `\[...\]`:

| Kind | Delimiter | Example |
|------|-----------|---------|
| Inline | `$...$` | The join $R \bowtie S$ |
| Display | `$$...$$` on its own line | See block below |

$$
\sigma_\phi(R) = \{ t \in R \mid \phi(t) \}
$$

- Put display blocks on their own lines; blank lines before/after help GitHub.
- Escape literal `$` in prose (e.g. `\$100`) or wrap in backticks (`` `$VAR` ``).
