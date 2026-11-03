# Executive summary

Write it in the user's conversation language. Use the project's own names for roles, screens, entities, and flows (from the spec, the digest, and the UI), never generic placeholders such as "the user clicks the button". Keep code identifiers out of the plain-words part.

## Technical summary

- **Root cause**: <one or two sentences: what failed, where, and why>
- **Change**: <modules, data, infrastructure, and UI touched, by layer, not line by line>
- **Chosen fix**: <candidate, final score, and confidence; why it beat the others, in one sentence>
- **Verification**: <test types run and their results compared with the baseline>
- **Not measured**: <dimension: reason>, or "none"
- **Risk and rollback**: <what could break, how to detect it, how to revert>
- **Follow-ups**: <baseline failures, spec gaps, pending decisions>, or "none"
- **Sources used**: <ticket, design frames, documents, and meeting notes the work relied on, with links>
- **Lessons**: <past lessons applied, new or reinforced lessons recorded, and any skill change proposed>, or "none"

## In plain words

Keep this part under 250 words, scenarios excluded. Explain any unavoidable technical term in parentheses.

**What was wrong**: <one or two sentences anyone in the business understands>

**What changes for people**: <one or two sentences>

### Scenarios

Write 2–4 scenarios: the reported case first, then the closest related cases the fix touches. Use concrete example values.

**<Scenario title in everyday words>**
- **Who**: <role as the project names it>
- **Situation**: <what they are doing, with an example value>
- **Before**: <what happened>
- **Now**: <what happens>
- **Verified by**: <e2e test | visual check | manual run | not verified end-to-end>

**What stays the same**: <behavior people might worry about that did not change>

**What to watch for**: <what users or support should notice in the first days>, or "nothing"

## Early stop

When the work stops before delivery (not a defect, not reproducible, no candidate passes, decision needed), replace **Chosen fix**, **Change**, and **Scenarios** with:

- **What was found**: <evidence so far>
- **Why it stopped**: <the decision-table row or hard rule that applied>
- **Decision needed**: <the question, who should answer it, and what each answer would unlock>
