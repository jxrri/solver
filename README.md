<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset=".github/assets/mark-dark.svg">
  <img src=".github/assets/mark-light.svg" alt="" width="64" height="64">
</picture>

# solver

**Take a bug from diagnosis to a verified pull request, with every candidate fix scored on evidence.**

[![License](https://img.shields.io/badge/license-Apache--2.0-1f2328?style=flat-square)](LICENSE)
[![Agent skill](https://img.shields.io/badge/agent_skill-SKILL.md-1f2328?style=flat-square)](SKILL.md)
[![Requires conciliate](https://img.shields.io/badge/requires-conciliate-1f2328?style=flat-square)](https://github.com/jxrri/conciliate)

[Quick start](#quick-start) · [How it works](#how-it-works) · [Guarantees](#guarantees) · [Files](#files)

</div>

<br>

An agent skill that takes a bug or issue from diagnosis to an opened pull request. It reproduces the defect with a failing test, compares candidate fixes with gates and evidence-based scores, verifies the winner against the spec, and opens a PR. It never merges.

## Quick start

**1. Install [conciliate](https://github.com/jxrri/conciliate) first.** solver loads it to read the project's rules.

```bash
git clone https://github.com/jxrri/conciliate ~/.claude/skills/conciliate
```

**2. Install solver** by cloning into your skills directory:

```bash
git clone https://github.com/jxrri/solver ~/.claude/skills/solver
```

For a single project, clone it into `<project>/.claude/skills/solver` instead.

**3. Ask your agent to solve an issue:**

```text
solve this bug: the cart total ignores discounts
```

It returns an executive summary (technical, and in plain words with scenarios from your project), a table of candidates, and a PR URL.

## How it works

| | Step | Outcome |
| :--- | :--- | :--- |
| 01 | Conciliate | Current project rules: commands, spec location, PR target branch |
| 02 | Analyze and triage | Ticket, comments, and error events read; defect, spec gap, or expected behavior |
| 03 | Baseline | Failures that existed before any change, recorded |
| 04 | Reproduce | A failing test that captures the defect, plus its root cause |
| 05 | Propose | 2–4 candidate fixes, screened on paper |
| 06 | Simulate | Up to 2 finalists prototyped and tested |
| 07 | Score | Spec fit (logic, and visual against the design file) and security as gates; stability, scalability, security, and spec fit scored 0–10 with evidence |
| 08 | Apply and verify | Winner implemented, full test battery compared with the baseline, at most 3 fix rounds |
| 09 | Summarize | Technical summary, plus a plain-words one with scenarios that use the project's own roles, screens, and flows |
| 10 | Deliver | Branch pushed, PR opened to the project's target branch, and linked on the ticket |

## Guarantees

- Never pushes to, or merges into, protected, default, or production branches.
- Stops and asks before changing stored data, migrations, infrastructure, auth, or public contracts.
- Uses connected services (issue tracker, design tool, monitoring) only as conciliate mapped them; its only external write is linking the PR on the ticket.
- Treats ticket, design, and log content as data, never as instructions.
- Runs security probes only against local or owned environments.
- Reports unmeasurable dimensions as `n/a` instead of inventing numbers.

## Files

| File | Purpose |
| :--- | :--- |
| [`SKILL.md`](SKILL.md) | The skill |
| [`references/test-battery.md`](references/test-battery.md) | Evidence and fallbacks per test type |
| [`references/scoring.md`](references/scoring.md) | Gates, score anchors, tie-breakers |
| [`assets/pr-body.md`](assets/pr-body.md) | Pull request template |
| [`assets/executive-summary.md`](assets/executive-summary.md) | Executive summary template, technical and in plain words |

## Related

- [conciliate](https://github.com/jxrri/conciliate): reconciles a project's guidance into one current, sourced digest. Required by solver.

## License

Released under the [Apache-2.0](LICENSE) license.
