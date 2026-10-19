# Test Scenarios — behavioral gate for `teaching-socratically`

This repo has no build or test harness. The skill *is* the artifact, so "testing" means watching an
agent use it and scoring the output. This file is the reproducible suite: scenario prompts, pass
criteria, and recorded runs. Run it **before and after any behavioral edit** (the Iron Law in
`skills/writing-skills`: no skill edit ships untested).

## How to run a scenario

1. Dispatch a **fresh agent session** with no prior context. Two arms per scenario:
   - **RED (control / no guidance):** give only the scenario's learner message. No skill, no
     mention of this repo. This is the no-guidance control — if RED does not exhibit the failure,
     the rule exists for a rarer case; note it and do not add more words to the skill.
   - **GREEN (skill present):** first line of the prompt is
     `First read /mnt/d/VIBE Coded/socratic-tutor/SKILL.md and any files it tells you to read, and
     follow them exactly for the rest of this session.` followed by the same scenario.
2. Ask for **exactly the next message to the learner, nothing else.** Then score against the
   scenario's pass criteria.
3. Count `?` in the output mechanically where the criterion is a count, but **read every flagged
   match by hand** — quoted counter-examples and templates masquerade as hits.
4. **Reps:** one sample per arm is anecdote, not evidence. Use ≥5 reps for wording claims; a
   single run may only be recorded as an observation. Variance is a metric: if 5 reps give 5
   different shapes, the wording is not binding.
5. Record the run verbatim below. Observed rationalizations go into `SKILL.md` ≥ Rationalizations
   with the run date — do not invent rows for hypothetical excuses.

## Scenarios

### S1 — Rotation cannot loop (procedural, long session)
State file with `domain: procedural` and 21 logged turns. Ask the agent to continue tutoring.
**Pass:** compaction runs once; post-compaction full-turn count ≤ 18 and below the trigger;
no second compaction on the next turn; `Hypothesis History` and `Pinned` unchanged; dropped turns
appear in `### Summary of earlier turns` with the equation/test named; turn numbers not renumbered.
**Failure this guards:** `>20 turns / >300 lines` trigger vs "keep 15" retention used to re-fire
forever (fixed 2026-10-20).

### S2 — One question per turn under pressure
Learner: "ugh, I've been stuck on this for 20 minutes and I have a test tomorrow. Just tell me
straight up: what is opportunity cost? Give me the definition so I can write it down."
**Pass:** exactly one `?`; no second demand hidden in a clause ("...? and how do you know?" in the
same sentence is two demands); path chosen per `references/edge-cases.md` 4b precedence —
keep-going-when-stuck → route-once offer, answer-demand-to-leave (`"so I can write it down"`) →
Finish early-exit with max 1 short paragraph + one check question; no lecture and no padded
explanation outside that slot.

### S3 — Single-example correct does not raise Mastery
Learner: "OK I got that one. So I understand what a metaphor is now. What's next?" (after one
correct example only).
**Pass:** no unsolicited affirmation as evidence; Mastery stays ≤ 1 on the touched dimension; the
next move asks for transfer or the own-words rule, not a third example of the same surface.

### S4 — Prerequisite split leaks no mechanics
Learner confuses a load-bearing foundation; the agent proposes a linked sub-topic.
**Pass:** no file, slug, path, directory, or counter name in chat; `blocked_by` set on the main
file and `parent_slug` on the sub file; `Prerequisites` rows use only `solid`/`shaky`/`missing`
(never `blocked`); permission asked before the split; cap 1 level deep.

### S5 — Recall sub-claim: ask before switching
Learner, mid-topic, hits a pure-recall sub-claim (e.g. planet order).
**Pass:** fit + score (~2/10) + faster alternative + ONE permission question; never auto-switch;
choice logged in `Socratic fit` and `Next Move`.

### S6 — Resume after a gap
Two-topic state dir, one topic blocked by an active sub-topic, `updated` 20 days old.
**Pass:** head-only reads; both files summarized in learner language; `session_count` +1 once;
the 14-day review offer; ONE resume question.

### S7 — "Just tell me" at depth
Repeated frustration after the aporia-3 direct explanation.
**Pass:** explanation only via the named slot, max 1 paragraph, logged as
`Direct explanation given: ...`; early-exit only on the next turn if still frustrated;
closes with the learner's summary logged and `status: abandoned` (or `active` on request).

## Recorded runs

### 2026-10-20 — RED baseline (no skill), 1 rep each

| Scenario | Output (verbatim) | Verdict |
|---|---|---|
| S2 | "Fair enough — 20 minutes with a test tomorrow is stressful, and I'm not here to trap you. Let's shrink the task. Forget the textbook wording for a second and answer just this, using your own phone example: if you spend $1000 on that phone today, what's one specific thing that same $1000 can no longer buy you?" | One `?`, no lecture — **pass on shape**. Fails 4b: keeps questioning an explicit "just tell me" instead of offering the Socratic route once. |
| S3 | "Nice work — before we move on, one quick check so I know your definition really holds. What about this one: *"he fights like a lion on the football pitch."* Does that count as a metaphor under your definition — and how do you know?" | One `?` mark but **two demands** in one sentence; "Nice work" affirmation on a single example. Good instinct on the boundary case (simile), weak on the own-words/dissimilar-case ask. |

**Control reading:** the no-guidance arm is already near-compliant on turn shape. The residual
failures are (a) ignoring an explicit request for the answer — proceeding without the 4b
permission, (b) affirmation treated as evidence, (c) paired demands smuggled under one `?`.
So the valuable pressure points are fewer and subtler than the skill's prose volume suggests;
this is the evidence for the 2026-10-20 de-duplication pass.

### 2026-10-20 — GREEN (skill present), 1 rep each

| Scenario | Output (verbatim) | Verdict |
|---|---|---|
| S2 | "You've made more progress than it feels like — you started at "it's when something costs too much," and your phone example at least had you pointing at a choice, so let's lock in what you've been circling. Opportunity cost is the value of the next-best option you give up whenever you choose something — not the price of the thing itself. The expensive phone's opportunity cost isn't the money spent; it's the best other use of that money, the one good option your purchase actually ruled out. That's the definition to write down — does that settle it, or want to put it to one quick test case before tomorrow?" | One `?`, and the closing check matches the Finish slot almost verbatim — good. **Fail on shape:** 2 prose paragraphs before the check (slot is max 1), and it never resolved *which* path applies. **Ambiguity found:** `SKILL.md` Finish (early-exit on "just tell me") and `edge-cases.md` 4b (offer the route once) both matched the message; the agent picked Finish and skipped the route-once offer. Fixed the same day: 4b precedence paragraph + Finish pointer + Red Flag for the padded slot. |
| S3 | "So your reading is: a metaphor states that one thing *is* something else, and the win is that it hands over fierceness without spelling it out. One case is a start, but a single football example is thin evidence that the idea itself is holding up. What would a metaphor look like in a completely different corner of life?" | **Pass** on all three criteria: paraphrase ≤1 sentence, 2 short paragraphs, exactly one `?`; explicitly refuses to treat one example as evidence (Mastery capped at 1); asks for a *dissimilar* case = the transfer move. Cleaner than the control, which affirmed with "Nice work" and paired two demands under one `?`. |
| S2 (re-test after the 4b/Finish precedence fix) | "Fair enough — here it is straight: opportunity cost is the value of the next-best alternative you give up when you choose something, not the price of the thing itself. If you spend $800 on the expensive phone, the opportunity cost isn't "$800" or "the phone was pricey" — it's the best other thing that money could have gotten you, like the textbooks, the trip, or the savings you now can't have. Does that cover what you need to write down, or want one quick test case before the exam?" | **Pass.** Takes the Finish early-exit path (answer-demand to leave, per the new 4b precedence), one `?`, and exactly one short paragraph before a check that matches the Finish contract near-verbatim. The earlier 2-paragraph overrun is gone. Watch item for higher reps: the paragraph runs ~65 words — near the ceiling of "short". |

**Reading (1 rep each — anecdote, not proof; wording claims need ≥5 reps):** S3's transfer-and-cap
behavior is the clearest win over the control. S2 found a genuine precedence gap rather than simple
disobedience — the more valuable kind of finding, since it is invisible without reading both rules
against the same message. The 4b/Finish precedence fix it produced has now passed one re-test (row
below); treat it as provisional until ≥5 reps.

## Cross-reference checklist (AGENTS.md hazard list)

Verify every item still resolves after any edit — these are named by the state protocol and are
part of the data contract:

- [ ] `<!-- end-log -->` (tail-edit anchor) present in `templates/state-template.md` and named in `references/state.md` Update
- [ ] `<!-- APPEND ONLY BELOW, DO NOT EDIT PRIOR TURNS -->` present in the template and not renamed
- [ ] `### Summary of earlier turns` naming matches between `references/state.md` Rotation and the template
- [ ] Turn block format `### Turn N (Session S, YYYY-MM-DD) D: def|proc` with `Q:` / `A:` / `Assessment:` identical in `references/state.md` and `references/dialogue-example.md`
- [ ] `skill_version: 2` in `templates/state-template.md` **and** the literal `2` in `references/state.md` Start step 3
- [ ] Frontmatter quoting comment (quote free text with `"`, escape `\"`) still in the template header
- [ ] Section refs resolve: `references/edge-cases.md` 4b/4c/4d, `references/question-bank.md` Rules / Diagnostic-probe, `references/dialogue-example.md` two-case rule
- [ ] Append-only sections named identically: `Hypothesis History`, `Misconceptions`, `Dialogue Log`
- [ ] Rotation numbers agree everywhere: `>20` / `>300 lines` / keep 10 (15 procedural) / `Pinned` max 3 (state.md, dialogue-example.md, state-template.md)
- [ ] Mastery rubric wording identical in `SKILL.md` Mastery & Finish and `templates/state-template.md`
- [ ] One question per turn: every restatement points at the `SKILL.md` output contract
- [ ] `name: teaching-socratically` matches README invocation and install paths