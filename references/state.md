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

## Concurrency and deletion

- One active chat per slug, tracked by `last_session_id`. The chat whose ID matches the file persists; any other conversation on the same slug tutors memory-only and tells the learner which chat is persisting.
- On "delete my data" for a topic: delete `<slug>.md` + drop its index row. On "delete all": clear the state dir + index.

## Index repair

- If `index.md` is missing but topic files exist: rebuild it by reading each file's frontmatter (`topic`, `slug`, `goal`, `level`, `session_count`, `updated`, `status`). Tell the learner you rebuilt it.
- If an index row points to a deleted file: drop the row and note it in chat.
- If a topic file exists without an index row: add the row.

## Slug

Lowercase kebab-case, 1-64 chars: `^[a-z0-9]+(-[a-z0-9]+)*$`. Normalize topic ("Justice & Fairness" → `justice-fairness`). On collision with a different topic/goal, append `-2`, `-3` and ask the learner to confirm the disambiguation (e.g. "You have `justice.md` about fairness and now justice-as-law — use `justice-2`?").

## Start

1. Glob active state dir + legacy `socratic-state-*.md` in CWD.
2. Collect all matches with `status: active`. Sort by per-topic file frontmatter `updated` descending (file is source of truth; index is stale mid-session).
   - If one match: read it, summarize Goal + Current Hypothesis (with version, e.g. v3) + Next Move + Updated date, ask learner to confirm/correct ("Is that still your view?"), then ask ONE resume question. See `references/dialogue-example.md`.
   - If multiple matches: list at most 5 (Topic + Updated + 1-line hypothesis) and ask the learner to pick one, or offer the most recent as default ("Resume `justice` from 2026-09-20, or pick another?"). Then follow the single-match resume.
   - If state older than ~14 days: offer quick review ("Want a 2-sentence recap or restart fresh?") before resuming.
3. If no match: run staged Opening (see `SKILL.md`), then create file from `templates/state-template.md`. Set `created`/`updated` to today (`YYYY-MM-DD`), `session_count: 1`, `aporia_cycles: 0`, `skill_version: 1`, `last_session_id` to the current chat ID (empty if unavailable).
4. If resuming an existing topic: compare the current chat ID to frontmatter `last_session_id`. If different (or file `updated` is older than the current session start and IDs unavailable), increment `session_count` by 1 once at session start and store the new ID. Never increment per turn or on mid-chat topic switch.
5. If topic switch mid-session: save current file + index before creating new one.

## Session identity

- `session_count` increments once per new conversation, detected via `last_session_id` mismatch (see Start step 4). A new conversation = a different chat ID / resumed chat after a gap, not a topic switch within the same chat. If chat IDs are unavailable, use `updated` date gap + explicit resume confirmation as fallback.
- Log timestamps as `YYYY-MM-DD`. Same-day turns share the date; ordering comes from monotonically increasing `Turn N`. Never reuse or rewrite a turn number.

## Update

After EVERY turn, update the per-topic state file.

Mutation types:

- Rewrite allowed (in place): `Current Hypothesis`, `Agreed Premises`, `Mastery` (up or down, only on observed evidence), `Next Move`, `Remaining Uncertainties`, `updated`, frontmatter `session_count`/`status`/`aporia_cycles`/`last_session_id`.
- Append-only, never edit/delete/reword: `Hypothesis History` (`v1, v2...`), `Misconceptions` lines, and `Dialogue Log` full turns. To add a misconception, append a new line. To mark one resolved, append a new `[resolved]` line — do not edit the old `[active]` line.
- `aporia_cycles`: increment by 1 each turn ending in unresolved contradiction; reset to 0 when the learner revises the thesis. At 3, follow frustrated-learner handling (see `references/edge-cases.md`).
- Stall (no contradiction, no revision): if `Current Hypothesis` is unchanged for 3 consecutive turns (no new `Hypothesis History` version), scaffold down one abstraction level or offer a recap choice. Do not add a counter; derive from history + log.

Single-rewrite preferred per turn (frontmatter + body stay in sync):

1. `read` the file tail (~20 lines) before the update.
2. One `edit` covering frontmatter fields (`updated`, `aporia_cycles`, `last_session_id`, `status` if changed) + body + log append (see Append protocol below). Only split into two `edit`s if the tool cannot cover both regions in one call.

Append protocol (use `edit`, never `write`, for updates):

1. `read` the file tail (~20 lines) before the update.
2. Set `oldString` to the stable tail anchor only (`<!-- end-log -->` plus the 2-3 lines above it). Set `newString` to anchor content + new block(s) inserted before `<!-- end-log -->`. Use `replaceAll: false`. Canonical example in `references/dialogue-example.md`.
3. On `edit` mismatch: re-`read` once and retry once with a fresh tail. On second failure: tutor memory-only for this turn and tell the learner state did not persist.

Verification: trust the `edit` tool result; full tail `read` only on mismatch or every 3rd turn to save tokens.

Turn block format:

```markdown
### Turn N (Session S, YYYY-MM-DD)
Q: <your question>
A: <learner answer summary>
Assessment: <what held, what broke>
```

Log direct explanations inside Assessment: `Direct explanation given: <1-line summary>`. Never log "yes, you're right" as mastery evidence.

Update `index.md` on session start, session end, and status change. Do NOT rewrite the index row on every turn — per-turn `Updated` churn is noise. The per-topic file's `updated` field is the per-turn source of truth; sync it to the index at session boundaries.

Rotation (hard trigger): compact when full `### Turn` count >12 OR file >150 lines. Then:

1. Keep the newest 10 full turns verbatim, in order.
2. Group all dropped turns by session into `### Summary of earlier turns` at the top of the log: one line per session, `Turns A-B (Session S, YYYY-MM-DD): starting thesis -> key refutation -> ending thesis`.
3. Never renumber turns. Never delete `Hypothesis History`. Verify after compaction: 10 full turns remain + summary covers every dropped turn number + history unchanged.

Template uses YAML frontmatter (machine-readable) + Markdown body (human-readable). See quoting rules in `templates/state-template.md`.
