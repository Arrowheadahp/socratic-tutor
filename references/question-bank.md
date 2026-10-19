# Question Bank

Use exactly ONE per turn. During Opening diagnostics use Diagnostic-probe order first (concrete-use → discrimination → prerequisite); after that pick by `Next Move`: lowest global Mastery dimension first (tie-break Clarity → Evidence → Counterexamples → Implications = dependency order); if any `Mastery by sub-claim` row scores below global, target that row first. Not in fixed order.

## Diagnostic-probe (Opening only — infer understanding, never ask level)
- Concrete-use: "Can you give an everyday example of X?"
- Procedural concrete-use: "How would you solve this tiny case [case]? Walk me through it."
- Discrimination: "Why does that count as X? What would NOT count as X?"
- Keep probing until the mental model is clear (usually 2-3 turns, no fixed max). Log every probe answer as Turn 1, Turn 2, etc. with Assessment of what held / what broke, then set inferred `level` on the first state update. Never ask "are you novice / intermediate / advanced?"

## Clarify (terms vague, fails generality/exclusion)
- "What do you mean by X?"
- "Can you give an example of X in everyday life?"
- "Can you put that another way?"
- "What would NOT count as X?"

## Assumptions (unstated belief doing work)
- "What are we assuming here?"
- "Is that assumption always true?"
- "What would have to be true for your claim to hold?"

## Evidence (asserted without reason)
- "What evidence supports this?"
- "What evidence would weaken it?"
- "How do you know?"

## Viewpoints / Counterexamples (narrow, ignores alternative)
- "What is the counter-argument?"
- "Who would disagree and why?"
- "Consider this case: [concrete case]. Does your definition still hold?"

## Implications (consequences unexplored)
- "If this were true, what else would follow?"
- "What is the best / worst / most realistic outcome if everyone acted on this?"
- "Does that consequence seem acceptable? Why?"

## Procedural (math, physics, code, proofs — thesis is a step, not a definition)
- "What should happen if we test your rule on [concrete numbers / edge input / units]?"
- "Walk me through the next step — what do you do and why?"
- "What would break if this step were wrong? How would you check?"
- "Can you solve a smaller/simpler version first, then generalize?"

## Transfer (example-correct ≠ principle-grasped — mandatory after a correct concrete case)
- "You handled [case A]. What rule are you using that would cover both A and [dissimilar case B]?"
- "Here is a new surface with the same principle: [case B]. What happens here, and is it the same rule as A?"
- "When would this rule NOT hold? Give a case it excludes."
- Single-example correct caps the touched Mastery dimension at 1; Mastery 2 needs two *dissimilar* cases + principle in the learner's own words. Rubric: `SKILL.md` Mastery & Finish (canonical).

## Prerequisite-probe (background gap suspected)
- "Quick check on the foundation: in your own words, what is [Y]?"
- "What does [Y] do in this picture — can you give an everyday example?"
- If solid: resume main line. If shaky: 1-paragraph grounding, then resume. If missing and large: propose a split (see `SKILL.md` Prerequisites).

## About the question (stuck, circling)
- "Why do you think I asked that?"
- "Which of your answers moved us most?"
- "Are we asking the right question? How would you reframe it?"

## Rules
- Never ask leading questions (ones that smuggle the answer), loaded questions (ones with a contested presupposition), or false dilemmas. A concrete parallel case ("Consider someone who...") is allowed: it invites judgment, it does not tell the answer.
- The anti-leading / anti-obvious filter itself is canonical in `SKILL.md` Method > Question quality; the bad→good pairs below are its worked examples:
  - Bad (leading): "So the Earth orbits the Sun, right?" Good: "What moves around what — and how would you tell?"
  - Bad (obvious): "Isn't courage a good thing?" Good: "Consider foolish endurance that harms people — fine or not? What makes it different from courage?"
  - Bad (smuggled): "Don't you think planets stay in orbit because of gravity?" Good: "Planets don't fly off in a straight line — what would have to be true to keep them curving? Give a case."
- One question per turn (output contract, `SKILL.md`). Opening diagnostics are staged one at a time (see `SKILL.md` Opening).
- Prefer concrete cases over abstract definitions. Before asserting a counterexample refutes T, verify it is valid (true premise, genuinely analogous).
