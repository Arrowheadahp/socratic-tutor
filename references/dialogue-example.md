# Dialogue Example (one full turn + resume + procedural)

## Elenchus, one refutation

Learner thesis T: "Courage is endurance."

> You: "So you hold that courage IS endurance — anyone who endures is courageous, correct?"
> Learner: "Yes."
> You: "Would you agree courage is something fine and admirable?"
> Learner: "Yes."
> You: "Consider someone who endures through ignorance — e.g. refuses to flee a burning building because they don't understand the danger. Is that endurance fine?"
> Learner: "No, that's just foolish."
> You: "Then we have a problem: if courage is endurance (T), foolish endurance should be courage. But we agreed courage is fine and foolish endurance is not. So T cannot hold as stated. That is progress — how would you revise T?"
>
> Apply the turn update with append-only `edit` (canonical protocol in `references/state.md` Update): `oldString` is the tail anchor only (`<!-- end-log -->` + 2-3 lines above). Never use whole-file `oldString`, prior Turns, `replaceAll: true`, or `write` to update. Never reword a prior turn.
>
> Minimal shape (insert before `<!-- end-log -->`):
>
> ```
> ### Turn 3 (Session 1, 2026-09-20)
> Q: <next question>
> A: <learner answer summary>
> Assessment: P1 held; T broke on counterexample.
> <!-- end-log -->
> ```
>
> Also rewrite `Current Hypothesis`, append `Hypothesis History` vN + `Misconceptions` line, set `Next Move`.
>
> Then update `updated` frontmatter. Sync `index.md` on session start/end/status change — not every turn.
>
> Compaction (trigger: >12 full turns or >150 lines): keep newest 10 full turns, summarize dropped ones by session at top of log. See `references/state.md` Rotation.

## Procedural (thesis is a step)

Learner rule T: "To divide fractions, divide top by top and bottom by bottom."

> You: "So your rule is (a/b) ÷ (c/d) = (a÷c)/(b÷d) — correct?"
> Learner: "Yes."
> You: "Let's test it on (1/2) ÷ (3/4). Your rule gives (1÷3)/(2÷4) = (1/3)/(1/2) = 2/3. Does that match what you get by invert-and-multiply?"
> Learner: "Invert-and-multiply gives (1/2)·(4/3) = 2/3. Same!"
> You: "It matched this time — good sign. Now try (2/3) ÷ (4/5) both ways. Still matching, or does one case break?"

Note: the move is a test case, not a definition challenge. If the rule breaks, mark aporia and invite revision; if it survives, demand boundary ("When would this NOT hold?").

## Opening (user gave everything upfront — don't re-ask)

> Learner: "Tutor me Socratically on photosynthesis, goal: pass my bio exam, level: novice, I think plants eat soil."
> You (bad): "What topic? What goal? What level?" (all already given)
> You (good): create the state file, then: "So your current view is plants gain mass by eating soil — correct? What makes you think so?"

## Resume (multi-session)

> You: "Welcome back. Last time on **justice** (2026-09-20): goal 'argue about fairness convincingly', hypothesis v3 'justice is giving each what they are owed', stuck on 'what is owed to a thief?'. Is that still your view, or has it shifted?"
> Learner: "Still holds, I think."
> You: "Good — then let's pick up there: if a thief is owed punishment, is giving punishment the same KIND of giving as giving wages?"

Note the pattern: summarize Goal + Current Hypothesis + Next Move, ask for confirmation, then ONE question.
