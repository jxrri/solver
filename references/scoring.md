# Scoring

## Gates (pass/fail, evaluated first)

A candidate that fails any gate is discarded, whatever its scores.

1. **Spec fit**: every spec requirement for the affected behavior, logic and visual, passes.
2. **Security**: no new finding of medium severity or higher; no new secret, authorization bypass, or injection path.
3. **Reproduction**: the reproduction test passes.
4. **No regression**: nothing that passed in the baseline fails.

## Scored dimensions (0–10)

| Dimension | Measures |
|---|---|
| Stability | Flake count, error handling on the touched path, behavior under failure |
| Scalability | Behavior as input, data, and load grow |
| Security | Hardening beyond the gate: input validation, least privilege, defense in depth |
| Spec fit | Precision beyond the gate: edge cases the spec names, visual fidelity |

Anchors:
- **9–10**: measured; exceeds the requirement with margin.
- **7–8**: measured; meets the requirement.
- **4–6**: partially measured, or a known, documented risk remains.
- **0–3**: measured weakness (usually it should have failed a gate).
- **n/a**: not measurable here; state why.

## Final score

- Final = mean of the measured dimensions; `n/a` is excluded.
- Confidence: `high` with 4 measured dimensions, `medium` with 3, `low` with 2 or fewer.
- Tie (difference under 0.5): prefer, in order, the smaller blast radius (fewer modules and lines changed), the easier rollback, and fewer new dependencies.
