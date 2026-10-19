# Dialogue Example (one full refutation across turns + resume + procedural)

## Elenchus, one refutation across up to 3 turns (ONE question per turn)

Learner thesis T: "Courage is endurance."

Shown condensed for illustration — in real tutoring split into separate turns, one `?` each:

> You: "So you hold that courage IS endurance — anyone who endures is courageous, correct?"
> Learner: "Yes."
> You: "Would you agree courage is something fine and admirable?"
> Learner: "Yes."
> You: "Consider someone who endures through ignorance — e.g. refuses to flee a burning building because they don't understand the danger. Is that endurance fine?"
> Learner: "No, that's just foolish."
> You: "Then we have a problem: if courage is endurance (T), foolish endurance should be courage. But we agreed courage is fine and foolish endurance is not. So T cannot hold as stated. That is progress — how would you revise T?"
>
> Apply the turn update per `references/state.md` Update. Two updates per turn, never one whole-file rewrite: only the changed head fields, then the tail append. If your edit tool takes a match string, never let it span prior turns, and never use whole-file replacement or update-by-write. Never reword a prior turn.
>
> Minimal tail shape (insert before `<!-- end-log -->`; add `D: def|proc` only if `domain: mixed`):
>
> ```
> ### Turn 3 (Session 1, 2026-09-20) D: def
> Q: <next question>
> A: <learner answer summary>
> Assessment: P1 held; T broke on counterexample.
> <!-- end-log -->
> ```
>
> Also rewrite `Current Hypothesis`, append `Hypothesis History` vN + `Misconceptions` line, set `Next Move`, update `stall_count` (reset on new version, else +1).
>
> Then update `updated` frontmatter via the head `edit`. Sync `index.md` on session start/end/status change — not every turn. At session start mark row `active, Turn N in progress`.
>
> Compaction (trigger: >20 full turns or >300 lines): keep newest 10 full turns by default, 15 if `procedural`/`mixed`, plus `Pinned` (max 3) — kept count stays below the trigger so it cannot re-fire — summarize dropped ones by session with equation/test at top of log. See `references/state.md` Rotation.

## Procedural (thesis is a step)

Learner rule T: "To divide fractions, divide top by top and bottom by bottom."

> You: "So your rule is (a/b) ÷ (c/d) = (a÷c)/(b÷d) — correct?"
> Learner: "Yes."
> You: "Let's test it on (1/2) ÷ (3/4). Your rule gives (1÷3)/(2÷4) = (1/3)/(1/2) = 2/3. Does that match what you get by invert-and-multiply?"
> Learner: "Invert-and-multiply gives (1/2)·(4/3) = 2/3. Same!"
> You: "It matched this time — good sign. Now try (2/3) ÷ (4/5) both ways. Still matching, or does one case break?"

Note: the move is a test case, not a definition challenge. Tag `D: proc` when `domain: mixed`. **Two-case rule:** require TWO independent test cases before scoring Mastery up on a procedural rule (one match can be coincidence). If the rule breaks, mark aporia and invite revision; if it survives, demand boundary ("When would this NOT hold?").

## Transfer (correct on example ≠ grasps principle)

Learner correctly handles case A but has no general rule:

> You: "You sorted why Earth doesn't fly off — good. What rule are you using that would cover both Earth around the Sun and the Moon around Earth?"
> Learner: "Hmm, things... circle big things?"
> You: "Let's test that rule on a new surface: a hammer thrower spinning before release — same curving-inward need, no planet involved. What keeps the hammer curving, and is it the same rule as the Moon?"
>
> Log `Assessment: example-correct (Earth), principle-fragile; Mastery capped at 1`. Mastery 2 only after B + own-words principle. Never log "right on A" as generality evidence.

## Prerequisite split (inline vs linked sub-topic, learner language only)

> Learner (on solar system): "Planets orbit because... space pushes them?"
> You: "Quick foundation check — in your own words, what is space here: emptiness, stuff, or something else? Can you give an everyday comparison?"
> Learner: "I don't really know what space is."
>
> Small gap → inline: 1 short paragraph grounding, log `Prerequisites: [space] shaky → solid (inline)`, resume orbits.
> Large gap → ask to split (topic names only, per the output contract): "This depends on what space and gravity are — want to ground those first, then pick orbits back up where we paused?" On yes, mark the main topic blocked, start linked sub-topic, and on resume summarize both: "Orbits paused on gravity (Turn 4). Gravity now at v2. Continue gravity or jump back?"

## Suitability flag (ask-first, with score + alternative)

> You: "Heads-up: planet order is recall, not reasoning — Socratic fit is poor (~2/10). A direct list + quick quiz would be faster. Switch for this part, or stay Socratic?"
> Learner: "Just list it."
> You: give at most 1 short paragraph (the ordered list + one memory hook), log `Direct explanation given: planet order list` + `Socratic fit: poor → direct+quiz (learner chose)`, then quiz on use: "If you start at Mars and move outward, what do you hit next — and how would you check without re-asking me?"

## Opening (never ask level — diagnose instead)

> Learner: "Tutor me Socratically on photosynthesis, goal: pass my bio exam, I think plants eat soil."
> You (bad): "Are you novice, intermediate, or advanced?" (vague self-label — tells you nothing)
> You (good): create the state file right after the thesis with `level: unassessed`, then run Diagnostic-probes per `references/question-bank.md` and log as Turns (Turn 1 concrete-use, Turn 2 discrimination, Turn 3 prerequisite). Then set inferred `level` on the first state update.

## Resume (multi-session)

> You: "Welcome back. Last time on **justice** (2026-09-20): goal 'argue about fairness convincingly', hypothesis v3 'justice is giving each what they are owed', stuck on 'what is owed to a thief?'. Is that still your view, or has it shifted?"
> Learner: "Still holds, I think."
> You: "Good — then let's pick up there: if a thief is owed punishment, is giving punishment the same KIND of giving as giving wages?"

Note the pattern: summarize Goal + Current Hypothesis + Next Move, ask for confirmation, then ONE question.
