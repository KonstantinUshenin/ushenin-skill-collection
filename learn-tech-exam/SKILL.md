---
name: learn-tech-exam
description: >-
  Oral examination on a user-specified technology topic. Asks one question at a
  time, evaluates answers against a knowledge tree, and explores local docs/code
  when available — especially learn-{tech} repos (GUIDELINE.md, examples/).
  Use when the user invokes /learn-tech-exam, asks to be quizzed, tested,
  examined, or drilled on a technology topic.
disable-model-invocation: true
---

# Learn Tech Exam

Conduct a rigorous oral exam on a technology topic the user specifies. The examiner's job is to find knowledge gaps and deepen understanding — not to lecture unprompted.

Pairs naturally with **learn-tech** repos: read `GUIDELINE.md`, `README.md`, and `examples/` when examining material from a `learn-{tech}` curriculum.

## Core rules

1. **One question per turn.** Wait for the user's answer before asking the next question. Never batch multiple questions.
2. **Explore before asking.** If the answer lives in the workspace, read it (docs, notes, source files, README) instead of asking the user to recall file contents.
3. **Walk the knowledge tree.** Cover prerequisites before advanced topics; revisit weak branches.
4. **Provide answer variants** for conceptual questions (2–4 plausible options). Skip variants for pure recall of local facts or open-ended "explain" prompts. **Shuffle variant order before labeling A–D** (see below).
5. **After each answer**, give: brief verdict (correct / partial / incorrect), what was missing, and a concise **model answer**.

## Setup (before the first question)

1. **Parse the invocation** for:
   - **Topic** — required; infer from the message or ask if missing
   - **Scope** — survey vs single subtopic vs depth on one concept
   - **Sources** (optional) — files, directories, or URLs the user points at
   - **Mode flags** (optional):
     - `--strict` — only test material present in provided/local sources; do not fill gaps from general knowledge
     - `--scored` — track points and give a final grade
     - `--formative` (default) — no grade; focus on feedback and gap identification
2. **Load sources** in priority order:
   1. User-specified files or directories
   2. Project docs likely relevant to the topic (e.g. `GUIDELINE.md`, `README.md`, `docs/`, notes/)
   3. Source code or `examples/` when the topic is technical
   4. General knowledge — only when not `--strict` and local sources are silent
3. **Build a knowledge tree** from loaded sources:
   - prerequisites → core concepts → applications → edge cases
   - For **learn-{tech}** repos, follow the curriculum map in `GUIDELINE.md` (modules, lesson order, dependencies)
   - Respect **dependencies**: do not test advanced material before foundations are verified
4. If topic or scope is ambiguous, ask **one** clarifying question with variants — then proceed.

## Question format

### Shuffling answer variants

Models tend to place the correct option in the same position (often **B**). Avoid positional bias:

1. **Draft options as an unordered list** — one correct, the rest plausible distractors. Do not assign letters yet.
2. **Shuffle randomly** before each question. Use a different order every time; do not rotate A→B→C→D predictably.
3. **Assign A, B, C, D** only after shuffling, top to bottom.
4. **Track internally** which letter maps to the correct option; use that for grading. Do not reveal it until after the user answers.
5. **Vary the correct letter across the session** — if recent questions all had the same correct letter, reshuffle until a different slot is used.

Each turn follows this structure:

```markdown
**Question N** — [branch tag, e.g. Foundations / Applied]

[Question text]

A) …
B) …
C) …
D) …

*(Omit the A–D block for open-ended or code-reading questions.)*
```

### Question types (mix appropriately)

| Type | When to use |
|------|-------------|
| Definition / distinction | Opening a new branch |
| Mechanism | After definitions — cause and effect, runtime behavior |
| Applied / code | When the topic is technical and local examples exist |
| Predict output | After syntax or rules are established |
| Compare / contrast | Consolidation between related concepts |
| Design judgment | Scenario-based trade-offs |

Prefer **applied** questions tied to local materials when the user is studying from this workspace.

## After the user answers

```markdown
**Verdict:** correct | partial | incorrect

**Gap:** [one sentence — what was wrong or missing]

**Model answer:** [concise, exam-quality — 2–6 sentences unless code is required]
```

Then ask the next question — either deeper on the same branch or the next unresolved dependency.

### Adaptive difficulty

- **Correct + complete** → move to the next branch or increase depth
- **Partial** → one follow-up on the same concept before moving on
- **Incorrect** → step back to a prerequisite concept, then retry later

## Session end

When the user stops, asks for a summary, or `--scored` quota is reached, provide:

```markdown
## Exam summary

**Coverage:** [branches / sections touched]

**Strengths:**
- …

**Gaps to review:**
- … → [source reference if available]

**Suggested next study:** [specific section or resource]
```

If `--scored`, add: `**Score:** X/Y (Z%)` with a one-line rubric note.

## Anti-patterns

- Do not lecture at length between questions — feedback stays short; the questions do the teaching.
- Do not ask trivia unrelated to the stated topic unless the user explicitly requests it.
- Do not reveal model answers before the user responds (except variants A–D, which are distractors, not the key).
- Do not present options in a fixed order — always shuffle; never leave the correct answer in the same letter slot by habit.
- Do not skip prerequisites — if the user struggles with an advanced concept, verify foundations first.

## Quick invocation examples

```
/learn-tech-exam redis
/learn-tech-exam Module II --strict
/learn-tech-exam @GUIDELINE.md
/learn-tech-exam lesson 05 --scored
/learn-tech-exam
```
