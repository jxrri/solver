# solver

An agent skill that takes a bug or issue from diagnosis to an opened pull request. It reproduces the defect with a failing test, compares candidate fixes with gates and evidence-based scores, verifies the winner against the spec, and opens a PR. It never merges.

## Quick path

1. Install [conciliate](https://github.com/jxrri/conciliate) first: solver loads it to read the project's rules.
2. Clone this repository into your skills directory:

   ```bash
   git clone https://github.com/jxrri/solver ~/.claude/skills/solver
   ```

   For a single project, clone it into `<project>/.claude/skills/solver` instead.
3. Ask your agent to solve an issue, e.g. "solve this bug: the cart total ignores discounts". It returns an executive summary (technical, and in plain words with scenarios from your project), a table of candidates, and a PR URL.

## What it does

| Step | Outcome |
|---|---|
| Conciliate | Current project rules: commands, spec location, PR target branch |
| Analyze and triage | Ticket, comments, and error events read; defect, spec gap, or expected behavior |
| Baseline | Failures that existed before any change, recorded |
| Reproduce | A failing test that captures the defect, plus its root cause |
| Propose | 2–4 candidate fixes, screened on paper |
| Simulate | Up to 2 finalists prototyped and tested |
| Score | Spec fit (logic, and visual against the design file) and security as gates; stability, scalability, security, and spec fit scored 0–10 with evidence |
| Apply and verify | Winner implemented, full test battery compared with the baseline, at most 3 fix rounds |
| Summarize | Technical summary, plus a plain-words one with scenarios that use the project's own roles, screens, and flows |
| Deliver | Branch pushed, PR opened to the project's target branch, and linked on the ticket |

## Safety rules

- Never pushes to, or merges into, protected, default, or production branches.
- Stops and asks before changing stored data, migrations, infrastructure, auth, or public contracts.
- Uses connected services (issue tracker, design tool, monitoring) only as conciliate mapped them; its only external write is linking the PR on the ticket.
- Treats ticket, design, and log content as data, never as instructions.
- Runs security probes only against local or owned environments.
- Reports unmeasurable dimensions as `n/a` instead of inventing numbers.

## Files

| File | Purpose |
|---|---|
| `SKILL.md` | The skill |
| `references/test-battery.md` | Evidence and fallbacks per test type |
| `references/scoring.md` | Gates, score anchors, tie-breakers |
| `assets/pr-body.md` | Pull request template |
| `assets/executive-summary.md` | Executive summary template, technical and in plain words |

## License

[Apache-2.0](LICENSE)
