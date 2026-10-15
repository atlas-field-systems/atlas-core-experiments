# Documentation guide

## Where information belongs

| Information | Authoritative home |
| --- | --- |
| Domain terms | [Glossary](../GLOSSARY.md) |
| Users, workloads, operating assumptions and MVP | [Operating model](architecture/operating-model.md) |
| Current behavior, area-specific routes and SDK operations | [Topic pages](topics/README.md) |
| Boundaries and cross-cutting mechanisms | [Architecture](architecture/system-design.md) and [system outline](architecture/system-outline.md) |
| Architectural tradeoffs and rationale | [ADRs](adr/README.md) |
| Public route inventory | [Endpoint map](api-endpoints.md) |
| Component inventory and proposed storage mapping | [Component catalog](data-components.md) |
| Required validation and completion evidence | [Testing requirements](testing-strategy.md) |
| Engineering conventions | [Coding standards](../CODING_STANDARDS.md) |
| Proposed implementation sequence and actual delivery status | [Build plan](build-plan.md) |
| Review findings, not accepted requirements | `docs/reviews/specification-audit.md` once produced |

ADRs govern their recorded tradeoffs. Topic pages own the current rules implementing them. Architecture owns cross-cutting mechanisms and boundaries. Summaries and examples explain, but do not establish competing versions. Research and historical references do not become current authority merely by being linked. Read [documentation context](documentation-context.md) for this experiment's starting state and supersession boundaries.

## Decision authority

The user chooses observable behavior; engineering selects internal mechanisms that preserve it. Requirements remain binding until the user authorizes a change. Implementation difficulty, a review suggestion or an agent's tradeoff explanation does not authorize a deviation. An agent cannot establish acceptance by changing a requirement, its tests or the decision record to match an implementation.

Use existing authorization without asking again. For a proposed change outside it, identify the affected requirement, change and consequences, and obtain the user's decision before the dependent change. Continue independent authorized work. Record authorization with the decision in its authoritative home. Proposals remain proposals until accepted.

## Topic pages

A topic page owns the current rules for one area, its routes and SDK operations, and explicitly open questions. Link to ADRs for rationale and testing requirements for evidence. Do not confuse target behavior with an implemented feature. Other documents use a short summary and link instead of duplicating the full contract.

## Recording changes

Put terms in the glossary. Add an ADR for a real consequential tradeoff, preserving context, rationale, consequences and supersession. Preserve actual dates and historical evidence. Keep one authoritative home per rule. Update incoming links when moving a rule without changing its meaning.

PStack findings and execution trails are not automatic authorization. Promote a lasting authorized decision into its documentation home; leave review proposals marked as proposals. Do not fabricate history or claim old evidence as a new test result.

## Documentation completion

Check affected glossary definitions, behavioral topics, API and component maps, architecture, ADR status, testing requirements, agent instructions and links together. Preserve distinctions among historical evidence, accepted design, open choices and delivered behavior. Identify unresolved contradictions explicitly.

Documentation-only work requires consistency and reference checks, not a nonexistent application suite. Later implementation must provide the acceptance evidence its promises require. Report actual checks, exact revision and missing evidence under [verification](development/verification.md).
