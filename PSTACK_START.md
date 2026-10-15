# PStack starting prompts

## First run: audit the specification, no code

```text
$poteto-mode

This repository is a documentation-only starting point for Atlas Core.
There is no implementation to preserve or extend. Read AGENTS.md and the
system documentation, not another repository's source code or PR diffs.
Read docs/documentation-context.md to distinguish current requirements
from historical delivery records and source-system implementation references.

Critically review the system on its own terms. Find contradictions, ambiguous
contracts, missing assumptions, excessive coordination, unbounded resource
behavior, unsuitable module boundaries, weak acceptance criteria, unnecessary
complexity and gaps that would block implementation. Distinguish genuine
contradictions from accepted tradeoffs and design choices that remain open.

Keep accepted behavior and architectural decisions as requirements. You may
challenge them with a specific reason, but do not silently rewrite or waive
them. Resolve technical facts yourself using primary sources or evidence.
Do not interview me about internal engineering decisions you can make.
Ask only about an actual product decision that the written requirements do
not settle. Continue independent analysis around it.

Use the PStack review and design skills appropriate to the task. Report which
reviewers actually ran; do not invent independent or cross-model review.

Deliver docs/reviews/specification-audit.md with prioritized findings, exact
source sections, consequences, and proposed resolutions. Distinguish blockers
from recommendations and already-accepted constraints. Deliver
docs/build-plan.md with your own smallest sequence of verifiable end-to-end
increments, their acceptance evidence, dependencies, and estimated complexity.
Do not reproduce a previous project's ticket sequence or implementation plan.

This first task authorizes documentation changes only. Do not write application
code, bootstrap toolchains, install dependencies, publish a PR or merge anything.
Stop after the findings and plan so I can inspect how you approached the system.
```

## Later run: implement an approved increment

```text
$poteto-mode

Implement the next approved increment in docs/build-plan.md. Follow AGENTS.md,
the accepted contracts and the disposition of the specification audit.
Do not copy the previous Atlas implementation, its tests or generated output.

Choose the concrete internal design and organize the work yourself. Start with
one real end-to-end outcome, not a blanket framework or tests for every helper.
Add independently justified failure, recovery and contract evidence alongside
the behavior. Create the minimum real toolchain and verification entry point
needed for this increment; do not pretend a documented command already exists.

Preserve accepted behavior. A passing test suite cannot justify rewriting the
requirement to match the implementation. Reassess a failed design premise
instead of growing a patch-and-test loop. Keep unrelated suggestions out of
the current increment.

Report the exact changes, commands and results, review independence, remaining
requirements and what was not verified. Commit only the approved increment.
Do not publish or merge a PR unless I explicitly authorize that action.
```
