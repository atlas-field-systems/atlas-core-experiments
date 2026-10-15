# System documentation

This is the specification input for a new implementation. Start with [documentation context](documentation-context.md) and [ownership rules](documentation.md). No prior delivery or test result is evidence that anything is implemented here.

| Question | Source |
| --- | --- |
| Users, workload, field assumptions and MVP | [Operating model](architecture/operating-model.md) |
| Vocabulary | [Glossary](../GLOSSARY.md) |
| Responsibility boundaries | [System outline](architecture/system-outline.md) |
| Module collaboration, storage and shared mechanisms | [System design](architecture/system-design.md) |
| Behavior, routes and SDK operations by area | [Topic pages](topics/README.md) |
| Accepted and superseded architectural tradeoffs | [ADRs](adr/README.md) |
| Public API inventory | [Endpoint map](api-endpoints.md) |
| Component inventory and proposed storage mapping | [Components](data-components.md) |
| Required validation scenarios | [Testing requirements](testing-strategy.md) |
| Engineering conventions | [Coding standards](../CODING_STANDARDS.md) |
| PStack runtime and work approach | [Development](development/README.md) |

The first audit produces `docs/reviews/specification-audit.md`. The [build plan](build-plan.md) is an output of that review, not a copy of another implementation's tickets. Historical issue identifiers and slice labels in imported prose do not assign work in this repository.
