# Verification

## Documentation-only starting state

There are no build commands, test suite, generated contracts or scripts to run in the initial repository. Do not invent a passing test result. Verify the actual Markdown artifact: file scope, relative links, source meaning, decision authority, ADR supersession and the distinction between a requirement and an implemented feature.

Check the glossary, relevant topics, endpoint and component maps, architecture and testing requirements together. Report contradictions as findings until resolved through the appropriate authority. Record whether the review was independent or self-review. Historical evidence links must not be used to import an earlier implementation into the audit.

## When implementation starts

Introduce the minimum real build and verification entry points required for the approved increment, and document their actual commands. Implement the engineering checks required by [coding standards](../../CODING_STANDARDS.md) and the relevant real-boundary scenarios in [testing requirements](../testing-strategy.md).

Prove observable outcomes and relevant failure and recovery paths, with independently authored expectations. A structural check or generated snapshot is not enough to establish an operational promise. Preserve the distinction between simulated contract evidence, real external integration and physical field readiness.

## Handoff

Report the candidate revision, commands actually executed, results, covered requirements, review independence and missing evidence. An unavailable or failed required check is not a pass. A changed candidate requires rerunning affected validation. Publication follows [Contributing](../../CONTRIBUTING.md#review-and-publication).
