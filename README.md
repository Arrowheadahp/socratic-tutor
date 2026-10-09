# socratic-tutor

Teach any concept via Socratic questioning — elenchus and guided discovery. The agent asks, never lectures. It draws the answer out of the learner, one question at a time.

Works with any coding agent that supports [Agent Skills](https://agentskills.io) (`SKILL.md`): Claude Code, OpenCode, Codex, Cursor, GitHub Copilot, Gemini CLI, and more.

## Install

### Option A: `npx skills` (recommended)

```bash
# List first (optional)
npx skills add Arrowheadahp/socratic-tutor --list

# Install globally (available in every project)
npx skills add Arrowheadahp/socratic-tutor -g -y

# Or install to the current project only
npx skills add Arrowheadahp/socratic-tutor -y

# Target specific agents only
npx skills add Arrowheadahp/socratic-tutor -g -a claude-code -a opencode -y
```

### Option B: manual copy

Clone the repo, then copy the `socratic-tutor` folder (the one containing `SKILL.md`) into your agent's skills directory:

| Agent | Global path | Project path |
|---|---|---|
| Claude Code | `~/.claude/skills/socratic-tutor/` | `.claude/skills/socratic-tutor/` |
| OpenCode | `~/.config/opencode/skills/socratic-tutor/` | `.opencode/skills/socratic-tutor/` |
| Codex / Cursor / Gemini / most others | `~/.agents/skills/socratic-tutor/` | `.agents/skills/socratic-tutor/` |
| GitHub Copilot (VS Code) | `~/.copilot/skills/socratic-tutor/` | `.github/skills/socratic-tutor/` |

```bash
git clone https://github.com/Arrowheadahp/socratic-tutor.git
mkdir -p ~/.claude/skills/socratic-tutor
cp -r socratic-tutor/SKILL.md socratic-tutor/references socratic-tutor/templates ~/.claude/skills/socratic-tutor/
```

The copy must include `SKILL.md` plus the `references/` and `templates/` directories — the skill won't work with `SKILL.md` alone.

## Use

The skill is user-invocable only — it never auto-triggers. Start it explicitly:

- `@socratic-tutor` / `/skill socratic-tutor` (depends on your agent), or
- "Tutor me Socratically on X", "Question me on X", "Teach me X through dialogue"

What to expect: the tutor elicits your topic, goal, and current thesis, runs 2–3 diagnostic probes, then asks exactly one question per turn — refuting via elenchus, building up via maieutics, tracking mastery (Clarity / Evidence / Counterexamples / Implications) across sessions.

Progress persists in per-topic state files (default `~/.opencode/socratic/`, override with `SOCRATIC_DIR`). See `references/state.md` for details.

## Layout

```
SKILL.md                  # skill definition (frontmatter: name, description, license)
references/
  state.md                # multi-session persistence (where, slug, start/resume, update)
  dialogue-example.md     # elenchus turn + resume + procedural example
  question-bank.md        # question taxonomy (diagnostic → transfer)
  edge-cases.md           # learner-state responses (stuck, frustrated, "just tell me", …)
templates/
  state-template.md       # per-topic state file
  index-template.md       # topic index
```

## Update / remove

```bash
npx skills update socratic-tutor -y
npx skills remove socratic-tutor
```

## License

MIT — see [LICENSE](LICENSE).
