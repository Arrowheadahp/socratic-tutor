# State — Multi-Session Persistence

## Where

Resolution order:

1. If env `SOCRATIC_DIR` is set, use it.
2. Else default global dir: `~/.opencode/socratic/`. Create it if missing.
3. Optional project override: `./.opencode/socratic/` in the nearest ancestor with `.git`, else CWD — use only if the learner explicitly asks for project-local state, or if the global dir is unwritable. Never split silently: if both exist, read both, prefer the most recently `updated` file per slug, and tell the learner which dir is active.

Per-topic file: `<slug>.md` (e.g. `justice.md`).
Index: `index.md` from `templates/index-template.md`.

Legacy: if `socratic-state-*.md` exists in CWD, read/migrate it once into the active dir, then use the new location.

## Failure fallback

- If state dir is unwritable or `glob`/`read`/`write` is unavailable: continue tutoring memory-only for this session and tell the learner state will not persist.
- If a state file has corrupt frontmatter: do not overwrite it. Tutor memory-only, note the corruption in chat, and ask the learner before resetting the file.

## Index repair

- If `index.md` is missing but topic files exist: rebuild it by reading each file's frontmatter (`topic`, `slug`, `goal`, `level`, `session_count`, `updated`, `status`). Tell the learner you rebuilt it.
- If an index row points to a deleted file: drop the row and note it in chat.
- If a topic file exists without an index row: add the row.

## Slug

Lowercase kebab-case, 1-64 chars: `^[a-z0-9]+(-[a-z0-9]+)*$`. Normalize topic ("Justice & Fairness" → `justice-fairness`). On collision with a different topic/goal, append `-2`, `-3` and ask the learner to confirm the disambiguation (e.g. "You have `justice.md` about fairness and now justice-as-law — use `justice-2`?").

## Start

1. Glob active state dir + legacy `socratic-state-*.md` in CWD.
2. Collect all matches with `status: active`. Sort by `updated` descending.
   - If one match: read it, summarize Goal + Current Hypothesis (with version, e.g. v3) + Next Move + Updated date, ask learner to confirm/correct ("Is that still your view?"), then ask ONE resume question. See `references/dialogue-example.md`.
   - If multiple matches: list at most 5 (Topic + Updated + 1-line hypothesis) and ask the learner to pick one, or offer the most recent as default ("Resume `justice` from 2026-09-20, or pick another?"). Then follow the single-match resume.
   - If state older than ~14 days: offer quick review ("Want a 2-sentence recap or restart fresh?") before resuming.
3. If no match: run staged Opening (see `SKILL.md`), then create file from `templates/state-template.md`. Set `created`/`updated` to today (`YYYY-MM-DD`), `session_count: 1`.
4. If topic switch mid-session: save current file + index before creating new one.

## Session identity

- `session_count` increments once per new conversation (not per turn). A new conversation = a new session ID / resumed chat after a gap, not a topic switch within the same chat.
- Log timestamps as `YYYY-MM-DD`. Same-day turns share the date; ordering comes from monotonically increasing `Turn N`. Never reuse or rewrite a turn number.

## Update

After EVERY turn, update the per-topic state file: `Current Hypothesis`, `Hypothesis History` (append-only `v1, v2...`, never rewrite), `Agreed Premises`, `Misconceptions ([active|resolved] -- why)`, `Mastery` (only on observed evidence), `Next Move`, `Remaining Uncertainties`, `updated`.

Update `index.md` on session start, session end, and status change. Do NOT rewrite the index row on every turn — per-turn `Updated` churn is noise. The per-topic file's `updated` field is the per-turn source of truth; sync it to the index at session boundaries.

Keep Dialogue Log append-only with `edit` (append blocks, never rewrite prior turns):

```markdown
### Turn N (Session S, YYYY-MM-DD)
Q: <your question>
A: <learner answer summary>
Assessment: <what held, what broke>
```

Rotation: keep last 10 turns full. Older turns → compress to `Summary of earlier turns` (1-3 sentences per session: starting thesis → key refutation → ending thesis). Compress when the log exceeds ~15 turns or context feels tight. Never delete `Hypothesis History`.

Template uses YAML frontmatter (machine-readable) + Markdown body (human-readable). Quote all free-text frontmatter values with double quotes and escape embedded `"` as `\"` (goals often contain colons).
