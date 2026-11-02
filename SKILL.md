---
name: solver
description: "Trigger: solve this bug, fix this issue, error, failing behavior, solver. Diagnose, reproduce, compare scored fixes, verify against the spec, and open a PR."
license: Apache-2.0
metadata:
  author: "David Reyes"
  version: "1.0"
---

## Activation Contract

Load when the user asks to solve a defect, error, failing behavior, or issue ticket whose cause or fix is not yet settled. Skip for an already-understood one-line change.

## Hard Rules

- Load the `conciliate` skill first, scoped to the affected area. Take commands, spec locations, branch target, and conventions from its digest; never guess them.
- Never push to, merge into, or force-push a protected, default, or production branch. Deliver by opening a PR to the target branch the digest names; if it names none, ask. Never merge the PR.
- Stop and ask when the spec is missing or contradicts itself or the code; when a digest `Needs decision` item touches this issue; or when a candidate changes stored data or migrations, infrastructure, auth or permissions, or a public contract.
- Spec fit and security are gates, not scores. Scores only rank candidates that passed every gate.
- Every score and test claim cites observed evidence (command and result). A dimension that cannot be measured here is `n/a` with the reason; never invent a number or a result.
- Security probes, including offensive-security skills, target only local or owned test environments, never production or third parties.
- Report baseline failures; do not fix them unless the user asks.
- The fix loop runs at most 3 rounds; then stop and report what still fails.

## Decision Gates

| Situation | Action |
|---|---|
| Behavior matches the spec | Report "not a defect" with the spec reference; stop |
| The spec itself is wrong | Report the spec gap; stop and ask |
| Not reproducible after 3 distinct attempts | Report attempts, environment, and hypotheses; stop |
| Only one viable candidate | Run gates and full battery; skip ranking |
| No candidate passes the gates | Report why each failed; stop and ask |

## Execution Steps

1. **Conciliate**: load `conciliate` scoped to the affected area; read its digest.
2. **Analyze**: state what is asked, expected vs actual behavior (logic and visual), what is affected (modules, data, users, adjacent systems), and what it blocks. Triage with the decision table.
3. **Baseline**: on unchanged code, run the digest's test commands; record pre-existing failures.
4. **Reproduce**: write a failing test through the public interface that captures the defect. Find the root cause and how it got there (`git log -L`, `git blame`, linked tickets).
5. **Propose**: 2–4 candidates that fix the root cause, each listing the adjacent modules it must touch. Screen them on paper against the gates; discard with reasons.
6. **Simulate**: prototype at most 2 finalists in isolated worktrees; run the failing test and `references/test-battery.md` on each.
7. **Score**: apply `references/scoring.md`; pick the top candidate.
8. **Apply**: implement the winner on a new branch named per the digest; the failing test now passes; update the docs and help text the change affects.
9. **Verify**: run the full battery on the whole system and compare with the baseline. Fix only failures this change caused, then re-run.
10. **Deliver**: commit per the digest's convention, push the branch, open a PR to the target branch with `assets/pr-body.md`, link the ticket when the digest names a tracker, and remove the prototype worktrees.

## Output Contract

Return: root cause, candidates table (gates, scores, evidence), chosen fix and why, verification against the baseline, `n/a` dimensions, PR URL, and open decisions.

## References

- `references/test-battery.md` — evidence and fallbacks per test type.
- `references/scoring.md` — gates, score anchors, averaging, tie-breakers.
- `assets/pr-body.md` — PR description template.
