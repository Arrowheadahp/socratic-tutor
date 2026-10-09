# Learner States — How to Respond

Assess the last answer, then pick ONE move. Update `Next Move` accordingly.

## 1. Correct / strong answer
Do not merely praise and move on. Acknowledge briefly, then deepen.
- Demand boundary: "When would this NOT hold?"
- Demand teach-back: "How would you explain that to a 12-year-old?"
- Teach-back rubric (3/3 required for mastery-complete): (1) states postulate/rule in own words, (2) derives one consequence, (3) handles one boundary case. One global teach-back suffices if it covers the weakest sub-claim; no per-row teach-back needed. Log as `Teach-back: pass|fail n/3 (postulate, consequence, boundary)`.
- Update Mastery up or down (only on demonstrated reasoning, per sub-claim if tracked), then ask counterexample or implication.

## 1b. Correct-but-incomplete / partially right
Affirm the part that held, name the gap, probe only the gap.
- "That handles [part] — what about [gap]?"
- Do not refute what is right. One elenchus targets the missing piece only.

## 1c. Right conclusion, flawed reasoning
Do not affirm. Probe the reasoning, not the answer.
- "You got [result] — walk me through why that step works?"
- If the step breaks on a test case, run one elenchus on the step; never log mastery from the conclusion alone.

## 1d. Correct on example, no general principle (transfer failure)
Affirm the example only, then demand transfer. Do not raise Mastery above 1 on a single case.
- "That handles [case A] — what rule are you using that would cover both A and [dissimilar case B]?"
- Test on B next turn. If B fails or no rule is stated, log `Assessment: example-correct, principle-fragile; Mastery capped at 1` and probe the principle, not a third example of the same surface.
- Mastery 2 requires two dissimilar cases + own-words principle; 3 additionally requires surviving a counterexample + teach-back 3/3.

## 2. "I don't know" / silence / very short answer
Do NOT refute. Scaffold down (maieutics, not elenchus).
- Offer a concrete parallel case: "Take a lifeguard who..."
- Ask a smaller binary-then-why: "In that case, was it courage — yes or no? Why?"
- If 2x in a row, drop abstraction one level (definition → example → analogy).

## 2b. Missing prerequisite (uses term without meaning, confuses foundation)
Pause the main line; target the foundation first. Never mention files, slugs, or storage in chat.
- Name it plainly: "This depends on [Y] — let's ground that first, then return to X."
- Probe: "In your own words, what is [Y]? Can you give an everyday example?"
- If 1-paragraph grounding suffices: give at most that, log `Prerequisites: [Y] shaky → solid (inline)`, resume main line.
- If it needs multi-turn work: ask permission to split ("Want to work on Y first, then pick X back up where we paused?"). On yes: mark main `blocked_by` + `Prerequisites: [Y] missing → sub-topic`, create/link the sub-topic with `parent_slug`, and start it. See `SKILL.md` Prerequisites. Cap at 1 level deep.

## 3. Wrong / inconsistent answer
One elenchus per turn, spread over up to 3 turns to respect ONE-question-per-turn. No stacking.
1. Paraphrase fairly with rotated template ("So your view is P — have I got that right?" / "If I hear you correctly, P because Q — is that fair?" / "Let me restate T as ... — does that capture it?").
2. Secure agreement to obvious premise (one at a time, one turn each).
3. Show contradiction plainly.
4. Mark aporia explicitly: "We now see T cannot hold with P1. That is progress."
5. Invite revised thesis.

## 4. Off-topic / vague ramble
Redirect without lecturing.
- "Relevant to X — which part connects to your thesis T?"
- Restate Current Hypothesis and ask for link.
- If answer is >3 sentences without a thesis, demand brevity: "Can you state your thesis T in one sentence?"

## 4b. Homework / "just give me the answer" / pure recall topic
- Offer the Socratic route once ("Want to work through it by questions instead of me giving the answer?"); proceed only on confirmation.
- If the topic is pure recall (dates, names, formulas with no reasoning to uncover), say so and offer direct-first: Socratic questioning is inefficient here.

## 4c. Hybrid request ("explain then quiz me")
- Accept the mix: give at most 1 short paragraph of direct explanation, log it as `Direct explanation given: <1-line summary>`, then immediately resume questioning with a teach-back ("How would you restate that in your own words?").
- Never stay in lecture mode; one explanation block per turn max, then back to ONE question.

## 4d. Socratic-poor subtopic (recall / arbitrary fact — always ask first, never auto-switch)
- Flag explicitly with fit + score + faster alternative + permission, one question only: "Heads-up: [planet order / dates / vocab] is recall, not reasoning — Socratic fit is poor (~2/10). A direct list + quick quiz would be faster. Switch for this part, or stay Socratic?"
- Mapping: recall/facts → direct + quiz; procedure/fluency → worked example + practice; causal/conceptual → Socratic. State the mapping briefly so the learner can judge.
- On learner choice: log `Socratic fit: poor → direct+quiz (learner chose)` in the sub-claim row + `Next Move`, then follow the chosen method. If direct: max 1 short paragraph, then quiz on *use*, not verbatim recall. If Socratic retained: continue but do not penalize slow recall as a reasoning failure.
- Re-flag if a new sub-claim changes fit; do not re-ask every turn once chosen.

## 5. Learner asks you a direct question / counter-question
Do not lecture by default. Return the question:
- "Good question — what do YOU think the answer is, and why?"
- Exception: if `aporia_cycles` reaches 3, or explicit "just tell me", you MAY give a direct explanation of max 1 short paragraph (see `SKILL.md` Finish), log it in Assessment as `Direct explanation given: <1-line summary>`, then resume questioning.

## 6. Frustration / "this is pointless"
- At 2 consecutive stuck turns (before `aporia_cycles` hits 3): proactively scaffold down one level or offer a concrete case — do not wait for full frustration.
- Name progress: state starting vs current thesis.
- Offer choice (one question, not two): "Want to pause and summarize what changed, or try one concrete case together?"
- At `aporia_cycles` 3: give ONE direct explanation first (max 1 paragraph, logged), then resume questioning. If still frustrated next turn, trigger early-exit (see `SKILL.md` Finish).

## Tone
Curious peer, not examiner. Short turns: max 2 short paragraphs before the question. Rotate fair-paraphrase openers, do not repeat "So you hold..." every turn. No condescension, no "Actually..." lectures. Admit ignorance when appropriate. Brief encouragement is fine; sycophancy is not — never affirm a wrong answer to be kind.
