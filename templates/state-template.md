---
topic: "Topic Name"
slug: "topic-slug"
goal: "learner goal in own words -- escape embedded quotes as \""
level: unassessed # inferred from diagnostic probes (Turn 1-2+), never self-reported; novice | intermediate | advanced set after probes, never asked in chat
domain: definitional # definitional | procedural (math/code/proof/step-rule) | mixed; see SKILL.md Method. If mixed, tag each Turn with D: def|proc.
parent_slug: "" # slug of main topic if this file is a prerequisite split; empty otherwise. Never mention slugs or files in chat.
blocked_by: "" # slug of prerequisite sub-topic blocking this file; empty when unblocked. Never mention slugs or files in chat.
skill_version: 2 # bump when template schema changes; old files keep working, new fields default empty. Migration: on read, treat missing fields as empty and fill on next head edit; never rewrite history sections during migration.
session_count: 1
last_session_id: "" # current/last persisting chat; set at session start, used for session_count + concurrency
aporia_cycles: 0 # unresolved contradictions; reset on thesis revision; 3 triggers frustrated handling
stall_count: 0 # turns with no new Hypothesis History version; reset on new version; 2 triggers scaffold-down
created: 2026-01-01
updated: 2026-01-01
status: active
---
<!-- Quote all free-text frontmatter values with double quotes; escape embedded " as \" (goals often contain colons). -->

# Socratic State - <Topic>

## Current Hypothesis
<latest thesis T, 1-2 sentences. If two live candidates, list T1 vs T2 and test one per turn.>

## Hypothesis History (append-only, never rewrite)
- v1 (Session 1, <YYYY-MM-DD>): <first thesis>
- v2 (Session S, <YYYY-MM-DD>): <what changed and why>

## Agreed Premises
- <P1>
- <P2>

## Prerequisites (background concepts this topic depends on)
- [solid|shaky|missing] <concept> -- <evidence> -- <inline|sub-topic: Name>
- Keep to 1-3 load-bearing concepts only. Mark `blocked` here when a `missing` prerequisite needs its own sub-topic before this topic can proceed.

## Misconceptions
- [active|resolved] <belief> -- <why it failed>

## Mastery (0-3, init all 0, re-score up or down only on observed evidence)
- Clarity: 0
- Evidence: 0
- Counterexamples: 0
- Implications: 0

Rubric: 0 absent, 1 vague, 2 consistent under questioning, 3 survives counterexample + can teach back. "Yes, you're right" is not evidence.

## Mastery by sub-claim (global = min across rows; all rows ≥2 required for mastery-complete)
| Sub-claim | Clarity | Evidence | Counterexamples | Implications | Socratic fit |
|-----------|---------|----------|-----------------|--------------|--------------|
| <e.g. time dilation> | 0 | 0 | 0 | 0 | optimal |

Socratic fit per row: `optimal` (reasoning/causal — stay Socratic) | `workable` (mix — Socratic on use, direct on facts) | `poor` (recall/arbitrary fact — direct + quiz is faster). Set on first contact with the sub-claim, revise on evidence. Never auto-switch method on `poor` — ask learner first (see `references/edge-cases.md` 4d).

## Pinned (max 3 Turn numbers exempt from compaction)
- <empty by default, e.g. Turn 7 light-clock derivation>

## Next Move
<question type + target weakness, e.g. "Counterexample to test generality">

## Remaining Uncertainties
- <what learner still unsure about>

## Dialogue Log (last 10 turns full by default, 15 if procedural/mixed, plus Pinned — older summarized)

### Summary of earlier turns (if compacted)
<empty until first compaction; then one line per dropped session, e.g. Turns 1-4 (Session 1, 2026-09-20): <starting thesis> -> <key refutation> -> <equation/test> -> <ending thesis>.>

<!-- APPEND ONLY BELOW, DO NOT EDIT PRIOR TURNS -->
### Turn 1 (Session 1, <YYYY-MM-DD>) D: def
Q: <your question>
A: <learner answer summary>
Assessment: <what held, what broke>

<!-- end-log -->
