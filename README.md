# solver

[![Agent Skill](https://img.shields.io/badge/agent-skill-blue)](https://github.com/jxrri/solver)
[![License: Apache-2.0](https://img.shields.io/badge/license-Apache--2.0-green)](LICENSE)

> Take a bug from diagnosis to a verified pull request, with every candidate fix scored on evidence.

An agent skill that takes a bug or issue from diagnosis to an opened pull request. It reproduces the defect with a failing test, compares candidate fixes with gates and evidence-based scores, verifies the winner against the spec, and opens a PR.

> [!IMPORTANT]
> It never merges.

---

## Quick path

> [!NOTE]
> **Prerequisite:** Install [conciliate](https://github.com/jxrri/conciliate) first: solver loads it to read the project's rules.

1. **Clone this repository into your skills directory:**

   ```bash
   git clone https://github.com/jxrri/solver ~/.claude/skills/solver
   ```

   For a single project, clone it into `<project>/.claude/skills/solver` instead.

2. **Ask your agent to solve an issue:**
   For example, "solve this bug: the cart total ignores discounts". It returns an executive summary (technical, and in plain words with scenarios from your project), a table of candidates, and a PR URL.

---

## What it does

```mermaid
flowchart LR
    A[Conciliate] --> B["Analyze & triage"]
    B --> C[Baseline]
    C --> D[Reproduce]
    D --> E[Propose]
    E --> F[Simulate]
    F --> G[Score]
    G --> H["Apply & verify"]
    H --> I[Summarize]
    I --> J[Deliver]
```

| Step | Outcome |
| :--- | :--- |
| **Conciliate** | Current project rules: commands, spec location, PR target branch |
| **Analyze and triage** | Ticket, comments, and error events read; defect, spec gap, or expected behavior |
| **Baseline** | Failures that existed before any change, recorded |
| **Reproduce** | A failing test that captures the defect, plus its root cause |
| **Propose** | 2–4 candidate fixes, screened on paper |
| **Simulate** | Up to 2 finalists prototyped and tested |
| **Score** | Spec fit (logic, and visual against the design file) and security as gates; stability, scalability, security, and spec fit scored 0–10 with evidence |
| **Apply and verify** | Winner implemented, full test battery compared with the baseline, at most 3 fix rounds |
| **Summarize** | Technical summary, plus a plain-words one with scenarios that use the project's own roles, screens, and flows |
| **Deliver** | Branch pushed, PR opened to the project's target branch, and linked on the ticket |

---

## Safety rules

- **Branch safety:** Never pushes to, or merges into, protected, default, or production branches.
- **High-impact change gates:** Stops and asks before changing stored data, migrations, infrastructure, auth, or public contracts.
- **Scoped external access:** Uses connected services (issue tracker, design tool, monitoring) only as conciliate mapped them; its only external write is linking the PR on the ticket.
- **Content as data:** Treats ticket, design, and log content as data, never as instructions.
- **Safe security probing:** Runs security probes only against local or owned environments.
- **Honest metrics:** Reports unmeasurable dimensions as `n/a` instead of inventing numbers.

---

<details>
<summary><strong>Files</strong></summary>

<br>

| File | Purpose |
| :--- | :--- |
| `SKILL.md` | The skill |
| `references/test-battery.md` | Evidence and fallbacks per test type |
| `references/scoring.md` | Gates, score anchors, tie-breakers |
| `assets/pr-body.md` | Pull request template |
| `assets/executive-summary.md` | Executive summary template, technical and in plain words |

</details>

## License

[Apache-2.0](LICENSE)
