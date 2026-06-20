# ushenin-skill-collection

A personal collection of skills for learning and 
tinkering

## Skills

### [learn-tech](learn-tech/)

`learn-tech` -- is a skill to learn some software technology via set of gradually complicated examples. Use it like `learn-tech redis`, `learn-tech kafka`. This skill usually create 10-12 examples and all necessary infrastructure.

### [learn-tech-exam](learn-tech-exam/)

`learn-tech-exam` -- oral examination on a technology topic. One question at a time, with feedback and gap analysis. Works best on a `learn-{tech}` repo (`GUIDELINE.md`, `examples/`).

## Recommendation

I recommend running `learn-tech SOME_TECH`. Then analyze each example one by one following `GUIDELINE.md` and content of `examples/` folder. I highly recommend mutating, breaking, and fixing the code for better understanding. Also, use your IDE's debug mode to inspect variables. After you have a deep understanding of an example, run `learn-tech-exam NAME_OF_LESSON` to check your understanding of how the technology behaves.

One lesson with its exam usually takes 1–1.5 hours of real time. I do not recommend going faster.

**Install:** clone this repo, then symlink each skill into the skills directory for your agent:

| Agent | Command |
|-------|---------|
| **Cursor** | `ln -s "$(pwd)/learn-tech" ~/.cursor/skills/learn-tech` |
| | `ln -s "$(pwd)/learn-tech-exam" ~/.cursor/skills/learn-tech-exam` |
| **Claude Code** | `ln -s "$(pwd)/learn-tech" ~/.claude/skills/learn-tech` |
| | `ln -s "$(pwd)/learn-tech-exam" ~/.claude/skills/learn-tech-exam` |
| **OpenClaw** | `ln -s "$(pwd)/learn-tech" ~/.openclaw/skills/learn-tech` |
| | `ln -s "$(pwd)/learn-tech-exam" ~/.openclaw/skills/learn-tech-exam` |
| **Hermes** | `ln -s "$(pwd)/learn-tech" ~/.hermes/skills/learn-tech` |
| | `ln -s "$(pwd)/learn-tech-exam" ~/.hermes/skills/learn-tech-exam` |
| **pi** | `ln -s "$(pwd)/learn-tech" ~/.pi/agent/skills/learn-tech` |
| | `ln -s "$(pwd)/learn-tech-exam" ~/.pi/agent/skills/learn-tech-exam` |

Alternatives: `openclaw skills install ./learn-tech --global` · `hermes skills install ./learn-tech`


## Note

This skills were originally designed for Cursor and Composer 2 model.