# Test battery

Run each type with the digest's command. When the project has no command for a type, use the fallback and label the result `local fallback`. When no fallback applies, mark the type `n/a` with the reason.

Run heavy suites one at a time: on constrained machines, parallel suites produce false failures.

| Type | Evidence to record | Fallback when the project has none |
|---|---|---|
| Unit and integration | Command, pass/fail counts, the reproduction test now passing | Focused tests in the project's runner |
| Stability | Reproduction test and touched suites re-run 5 times; flake count | Same, scoped to the touched tests |
| Stress and load | Tool, load profile, p95 latency, error rate | Bounded local run against the touched path with an installed tool (k6, autocannon, locust); label it not predictive of production |
| Scalability | Behavior at 1x, 10x, and 100x input or data size; query plans for database changes (`EXPLAIN ANALYZE`) | Complexity analysis of the changed path, labeled `analysis only` |
| End-to-end | Scenario names, pass/fail, traces on failure | Drive the running app through the affected flow, with browser automation when available |
| Visual | Screenshots of the affected screens compared with the spec or mockups | Screenshot comparison described screen by screen |
| Security | Dependency audit, SAST when configured, authorization checks on touched endpoints, targeted probes from installed security skills | Dependency audit plus review of input handling, authorization, and secrets in the diff |
| Spec fit | Each spec requirement (logic and visual) mapped to pass/fail with evidence | None: without a spec, stop and ask |

Rules:
- Use isolated state (test database, scratch copy) for anything that mutates data.
- Security probes run only against local or owned environments.
- Never report a type as passed without its evidence line.
