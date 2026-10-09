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

- If `index.md` is missing but topic files exist: rebuild it with head-only reads (first ~15 lines: frontmatter `topic`, `slug`, `goal`, `level`, `session_count`, `updated`, `status`). Never full-read every file to rebuild. Tell the learner you rebuilt it.
- If an index row points to a deleted file: drop the row and note it in chat.
- If a topic file exists without an index row: add the row.
- If the index grows past ~20 rows: keep all rows (history is never deleted) but only show the 5 most recent in chat; the rest stays in the file.

## Slug

Lowercase kebab-case, 1-64 chars: `^[a-z0-9]+(-[a-z0-9]+)*$`. Normalize topic ("Justice & Fairness" → `justice-fairness`). On collision with a different topic/goal, append `-2`, `-3` and ask the learner to confirm the disambiguation (e.g. "You have `justice.md` about fairness and now justice-as-law — use `justice-2`?"). For a broad topic that keeps spawning sub-questions, split into sub-slugs (`physics-kinematics`) rather than growing one file; link via `Suggested Next Topics` in the index.

## Start

1. Glob active state dir + legacy `socratic-state-*.md` in CWD. Lazy-load: read `index.md` only if glob finds >1 topic file or a repair is suspected; with 0-1 files skip the index read to save tokens.
2. Collect all matches with `status: active`. Sort by frontmatter `updated` descending using head-only reads (first ~15 lines) — do not full-read every file. The per-topic file's `updated` is source of truth; the index is stale mid-session. If any row still shows `, Turn N in progress` from a crashed session (file `updated` newer than index sync or older than ~1 day with no active chat), clear the marker at session start and note it in chat.
   - If one match: read it, summarize Goal + Current Hypothesis (with version, e.g. v3) + Next Move + Updated date, ask learner to confirm/correct ("Is that still your view?"), then ask ONE resume question. See `references/dialogue-example.md`. If the file has `blocked_by` set, also head-read the blocker file and summarize both in learner language (topic names only, never slugs/paths/files): main hypothesis + blocker status, blocker hypothesis + what it unblocks. If the file has `parent_slug` set, also head-read the parent and note the return point ("this unblocks X where we paused at ..."). Offer the choice in ONE question.
   - If multiple matches: list at most 5 (Topic + Updated + 1-line hypothesis) and ask the learner to pick one, or offer the most recent as default ("Resume `justice` from 2026-09-20, or pick another?"). Then follow the single-match resume. Prefer unblocking order: if a main topic is blocked by an active sub-topic, offer the sub-topic first.
   - If state older than ~14 days: offer quick review ("Want a 2-sentence recap or restart fresh?") before resuming.
3. If no match: run staged Opening (see `SKILL.md`): elicit topic → goal → thesis (unlogged Turn 0, never ask level). Then create file from `templates/state-template.md` right after thesis. Set `created`/`updated` to today (`YYYY-MM-DD`), `session_count: 1`, `aporia_cycles: 0`, `stall_count: 0`, `level: unassessed`, `skill_version: 2`, `last_session_id` to the current chat ID (empty if unavailable). Then run Diagnostic-probes (see `references/question-bank.md`; no fixed max) and log every probe answer as Turn 1, Turn 2, etc.; set inferred `level` at the first state update after the probes.
4. If resuming an existing topic: compare the current chat ID to frontmatter `last_session_id`. If different (or file `updated` is older than the current session start and IDs unavailable), increment `session_count` by 1 once at session start and store the new ID. Never increment per turn or on mid-chat topic switch.
5. If topic switch mid-session: save current file + index before creating new one.

## Session identity

- `session_count` increments once per new conversation, detected via `last_session_id` mismatch (see Start step 4). A new conversation = a different chat ID / resumed chat after a gap, not a topic switch within the same chat. If chat IDs are unavailable, increment once when the learner confirms a resume after a gap AND the file `updated` date differs from today (same-day continuations do not increment).
- Log timestamps as `YYYY-MM-DD`. Same-day turns share the date; ordering comes from monotonically increasing `Turn N`. Never reuse or rewrite a turn number.

## Update

After each tutoring turn, update the per-topic state file. Opening elicitation before the file exists is unlogged Turn 0 — skip the write; Turn 1 starts at the first diagnostic probe after file creation. Skip the write entirely if nothing changed this turn.

Mutation types:

- Rewrite allowed (in place, only fields that changed): `Current Hypothesis`, `Agreed Premises`, `Prerequisites` (concept rows solid/shaky/missing + inline/sub-topic), `Mastery` and `Mastery by sub-claim` (global = min across sub-claims; up or down, only on observed evidence; single-example correct caps touched dimension at 1; `Socratic fit` optimal/workable/poor per row), `Next Move`, `Remaining Uncertainties`, `goal` (if learner refines it), `level` (initial inference from diagnostic probes Turn 1-2+, then on adaptation shift; never asked in chat), `domain` (on reclassification, `definitional|procedural|mixed`), `parent_slug` / `blocked_by` (prerequisite linking), `Pinned`, `updated`, frontmatter `session_count`/`status`/`aporia_cycles`/`stall_count`/`last_session_id`.
- Append-only, never edit/delete/reword: `Hypothesis History` (`v1, v2...`), `Misconceptions` lines, and `Dialogue Log` full turns. To add a misconception, append a new line. To mark one resolved, append a new `[resolved]` line — do not edit the old `[active]` line.
- `aporia_cycles`: increment by 1 each turn ending in unresolved contradiction; reset to 0 when the learner revises the thesis (new `Hypothesis History` version counts as revision). At 3, give ONE direct explanation first per `SKILL.md` priority, then early-exit only if still frustrated next turn (see `references/edge-cases.md`).
- `stall_count`: increment by 1 each turn with no new `Hypothesis History` version (no contradiction, no revision); reset to 0 on any new version. Exempt the required second procedural test case (see `references/dialogue-example.md` two-case rule) from incrementing — it is confirmation, not stalling. At 2, scaffold down one abstraction level or offer a recap choice — do not wait for the 3-turn stall. Derive from history + log; frontmatter counter is source of truth.
- `Pinned`: list of max 3 `Turn N` numbers exempt from Rotation compaction (load-bearing refutations, e.g. key derivation or test). Auto-pin Turn 1 (origin thesis) plus the first key refutation by default; learner may override explicitly. Set explicitly; empty by default.

Two small `edit`s per turn (head and tail are non-contiguous — never use a whole-file `oldString`):

1. Head `edit`: rewrite only the changed frontmatter/body fields above the log. `oldString` is the exact field lines; leave unchanged fields untouched.
2. Tail `edit` (append protocol, use `edit`, never `write`, for updates): `read` the file tail (~20 lines) first. Set `oldString` to the stable tail anchor only (`<!-- end-log -->` plus the 2-3 lines above it). Set `newString` to anchor content + new block(s) inserted before `<!-- end-log -->`. Use `replaceAll: false`.
3. On `edit` mismatch: re-`read` once and retry once with fresh text. On second failure: tutor memory-only for this turn and tell the learner state did not persist.

Verification: trust the `edit` tool result; re-`read` the head every ~5 turns to catch drift, and on any mismatch.

Turn block format:

```markdown
### Turn N (Session S, YYYY-MM-DD) D: def|proc
Q: <your question>
A: <learner answer summary>
Assessment: <what held, what broke>
```

`D:` is required only when frontmatter `domain: mixed` (`def` = definitional move, `proc` = step-rule move tested with a concrete case). For single-domain files omit `D:`. Log teach-back as `Teach-back: <pass|fail> <n/3> (postulate, consequence, boundary)`. Log direct explanations inside Assessment: `Direct explanation given: <1-line summary>`. Never log "yes, you're right" as mastery evidence.

Update `index.md` on session start, session end, and status change. Do NOT rewrite the index row on every turn — per-turn `Updated` churn is noise. The per-topic file's `updated` field is the per-turn source of truth; sync it to the index at session boundaries. At session start, append `Turn N in progress` to the row (e.g. `active, Turn 29 in progress`) so long runs do not look stale; clear it at session end. For prerequisite splits, note the link in `Suggested Next Topics` (e.g. `sub-topic -- unblocks main-topic, paused at Turn N`) so either file resumes with context. In chat, refer to topics by name only — never mention slugs, file names, directories, or storage mechanics.

Rotation (hard trigger): compact when full `### Turn` count >12 OR file >150 lines. Then:

1. Keep the newest 10 full turns verbatim by default, newest 15 if frontmatter `domain` is `procedural` or `mixed`, plus any `Pinned` turns (max 3), in order. `Pinned` defaults already include Turn 1 origin, so early thesis is never lost.
2. Group all dropped turns by session into `### Summary of earlier turns` at the top of the log: one line per session, `Turns A-B (Session S, YYYY-MM-DD): <starting thesis> -> <key refutation> -> <equation/test> -> <ending thesis>`. Each summary line must name the dropped equation/test so procedural derivations survive compaction.
3. Never renumber turns. Never delete `Hypothesis History` or `Pinned` turns. Verify after compaction: correct full-turn count remains + summary covers every dropped turn number + history unchanged.

Template uses YAML frontmatter (machine-readable) + Markdown body (human-readable). See quoting rules in `templates/state-template.md`.
