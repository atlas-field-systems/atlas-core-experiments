---
name: verify-atlas
description: Verify Atlas documentation or an authorized implementation increment against actual repository artifacts and accepted requirements.
---

# Verify Atlas

Read [AGENTS.md](../../../AGENTS.md) and [verification](../../../docs/development/verification.md). Classify the actual checkout and diff before selecting checks.

A documentation-only checkout has no application verifier. Check scope, links, requirements, open findings and decision authority. Do not try inherited commands merely because a system document refers to an old test result.

When implementation exists, read all [coding standards](../../../CODING_STANDARDS.md) and relevant [testing requirements](../../../docs/testing-strategy.md). Run the current repository's documented checks and relevant real-boundary evidence. Report the exact revision, commands, outcomes, review independence and missing evidence. Never publish or merge as a side effect of verification.
