---
topic: "Topic Name"
slug: "topic-slug"
goal: "learner goal in own words -- escape embedded quotes as \""
level: novice
domain: definitional # or procedural (math/code/proof/step-rule); see SKILL.md Method
skill_version: 1 # bump when template schema changes; old files keep working, new fields default empty. Migration: on read, treat missing fields as empty and fill on next head edit; never rewrite history sections during migration.
session_count: 1
last_session_id: "" # current/last persisting chat; set at session start, used for session_count + concurrency
aporia_cycles: 0 # unresolved contradictions; reset on thesis revision; 3 triggers frustrated handling
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

## Misconceptions
- [active|resolved] <belief> -- <why it failed>

## Mastery (0-3, init all 0, re-score up or down only on observed evidence)
- Clarity: 0
- Evidence: 0
- Counterexamples: 0
- Implications: 0

Rubric: 0 absent, 1 vague, 2 consistent under questioning, 3 survives counterexample + can teach back. "Yes, you're right" is not evidence.

## Next Move
<question type + target weakness, e.g. "Counterexample to test generality">

## Remaining Uncertainties
- <what learner still unsure about>

## Dialogue Log (last 10 turns full, older summarized)

### Summary of earlier turns (if compacted)
<empty until first compaction; then one line per dropped session, e.g. Turns 1-4 (Session 1, 2026-09-20): <starting thesis> -> <key refutation> -> <ending thesis>.>

<!-- APPEND ONLY BELOW, DO NOT EDIT PRIOR TURNS -->
### Turn 1 (Session 1, <YYYY-MM-DD>)
Q: <your question>
A: <learner answer summary>
Assessment: <what held, what broke>

<!-- end-log -->
