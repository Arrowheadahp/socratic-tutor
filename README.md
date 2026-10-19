# teaching-socratically

Teach any concept via Socratic questioning — elenchus and guided discovery. The agent asks, never lectures. It draws the answer out of the learner, one question at a time.

> Formerly `socratic-tutor`. The skill is now named `teaching-socratically` (verb-first, per skill-authoring convention); the GitHub repo and install slug stay `Arrowheadahp/socratic-tutor`. If you invoke it by name, use `@teaching-socratically` / `/skill teaching-socratically`.

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

Clone the repo, then copy `SKILL.md` plus `references/` and `templates/` into a folder named `teaching-socratically` in your agent's skills directory (the cloned repo folder is `socratic-tutor` — the installed skill folder name must be `teaching-socratically`):

| Agent | Global path | Project path |
|---|---|---|
| Claude Code | `~/.claude/skills/teaching-socratically/` | `.claude/skills/teaching-socratically/` |
| OpenCode | `~/.config/opencode/skills/teaching-socratically/` | `.opencode/skills/teaching-socratically/` |
| Codex / Cursor / Gemini / most others | `~/.agents/skills/teaching-socratically/` | `.agents/skills/teaching-socratically/` |
| GitHub Copilot (VS Code) | `~/.copilot/skills/teaching-socratically/` | `.github/skills/teaching-socratically/` |

```bash
git clone https://github.com/Arrowheadahp/socratic-tutor.git
mkdir -p ~/.claude/skills/teaching-socratically
cp -r socratic-tutor/SKILL.md socratic-tutor/references socratic-tutor/templates ~/.claude/skills/teaching-socratically/
```

The copy must include `SKILL.md` plus the `references/` and `templates/` directories — the skill won't work with `SKILL.md` alone.

## Use

The skill is user-invocable only — it never auto-triggers. Start it explicitly:

- `@teaching-socratically` / `/skill teaching-socratically` (depends on your agent), or
- "Tutor me Socratically on X", "Question me on X", "Teach me X through dialogue"

What to expect: the tutor elicits your topic, goal, and current thesis, runs 2–3 diagnostic probes, then asks exactly one question per turn — refuting via elenchus, building up via maieutics, tracking mastery (Clarity / Evidence / Counterexamples / Implications) across sessions.

Progress persists in per-topic state files (default `~/.agents/socratic/`, legacy `~/.opencode/socratic/` is still read and never migrated silently, override with `SOCRATIC_DIR`). See `references/state.md` for details.

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
npx skills update Arrowheadahp/socratic-tutor -y
npx skills remove Arrowheadahp/socratic-tutor
```

## Validation

Behavioral changes are validated by simulated multi-session tutoring dialogues. The reproducible scenario suite, its pass criteria, and the recorded baselines live in [`tests/scenarios.md`](tests/scenarios.md) — run them before and after any edit to the skill's behavior.

## License

MIT — see [LICENSE](LICENSE).
