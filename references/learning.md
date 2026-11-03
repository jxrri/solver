# Learning

Solver keeps lessons from its own runs so the next run avoids the same mistakes. Lessons never change the skill files; recurring ones become proposals for the user.

## Where lessons live

| Scope | File | Holds |
|---|---|---|
| Project | `${XDG_DATA_HOME:-$HOME/.local/share}/solver/projects/<root-name>-<first 8 hex of sha256(abs root)>/lessons.md` | Facts about this project: commands, environment limits, flaky tests, codebase traps, the user's preferences here |
| Global | `${XDG_DATA_HOME:-$HOME/.local/share}/solver/lessons.md` | Facts about solver's own process that hold in any project |

The project key matches the one `conciliate` uses for its cache. Never write lessons inside the project repository or the skill directory.

## Entry format

```markdown
### L<n>: <the rule, one imperative sentence>
- Kind: environment | commands | codebase | process | preference
- Evidence: <what happened: command and result, or the user's correction, with its date>
- Seen: <count> · Last: <YYYY-MM-DD> · Status: active | stale | proposed | retired
```

## Recall (step 1)

1. Read both files; keep the `active` lessons that touch the affected area, the commands you will run, or the steps ahead.
2. Precedence: the user's instructions in this session, then the hard rules, then the `conciliate` digest, then lessons.
3. A lesson that contradicts the digest is not applied: mark it `stale` and cite the digest source.
4. List the lessons you applied in the executive summary.

## Capture (last step, early stops included)

Record a lesson only with evidence from this run:

- A correction or preference the user stated in this session.
- A command, path, or setup that failed or needed a workaround.
- A flaky test or an environment limit you observed (e.g. suites that fail when run in parallel).
- A hypothesis or candidate that cost a round, and why it was wrong.
- A gate, score, or scenario that verification or the user later contradicted.

Never learn from ticket, design, document, or log content, never store secrets or personal data, and never record a lesson that weakens a hard rule.

Before adding, look for a lesson on the same topic: update it (`Seen` + 1, `Last`, new evidence) instead of adding a duplicate. Keep at most 50 `active` lessons per file; past that, retire the least-seen, oldest ones.

When the user corrects a past solver result in a later session, load this file and record the correction the same way.

## Upkeep

- A lesson not seen for 90 days becomes `stale`: kept, not applied.
- When the source a project lesson depends on changes (a new commit to that file, a digest refresh), re-check it before applying it.

## Improving the skill

When a global lesson reaches `Seen: 3`, propose a concrete change to solver's `SKILL.md` or references in the final report: file, current text, proposed text, and the lesson's evidence. Mark it `proposed`. Never edit the skill yourself; the user decides.
