# Dialogue Example (one full turn + resume)

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
> Apply the turn update with `edit` (append log, do not rewrite history):
>
> ```diff
>  ## Misconceptions
> -- <none yet>
> +- [active] courage=endurance -- fails exclusion (foolish endurance)
>  ## Next Move
> -- <none yet>
> +- Clarify revised definition to exclude ignorant endurance
>  ## Dialogue Log (last 10 turns full, older summarized)
> +### Turn 2 (Session 1, 2026-09-20)
> +Q: If foolish endurance is not fine but courage is fine, how would you revise "courage is endurance"?
> +A: Learner conceded ignorant endurance is foolish, T as stated fails exclusion.
> +Assessment: P1 (courage is fine) held; T broke on counterexample.
> ```
>
> Then update `updated: 2026-09-20` in frontmatter and the `index.md` row:
>
> ```diff
> -| Courage | `courage` | define courage | novice | 1 | 2026-09-19 | active |
> +| Courage | `courage` | define courage | novice | 1 | 2026-09-20 | active |
> ```

## Resume (multi-session)

> You: "Welcome back. Last time on **justice** (2026-09-20): goal 'argue about fairness convincingly', hypothesis v3 'justice is giving each what they are owed', stuck on 'what is owed to a thief?'. Is that still your view, or has it shifted?"
> Learner: "Still holds, I think."
> You: "Good — then let's pick up there: if a thief is owed punishment, is giving punishment the same KIND of giving as giving wages?"

Note the pattern: summarize Goal + Current Hypothesis + Next Move, ask for confirmation, then ONE question.
