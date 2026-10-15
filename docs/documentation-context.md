# Documentation context

## Repository starting state

This is an independent documentation-only experiment. The repository's initial commit, `5e80cb920ffa3a9d8118c09408a487dadb900bd2`, already contained an Atlas documentation snapshot and had no parent commit. The PStack starting point builds on that local snapshot without importing the source repository's Git history.

Nothing is implemented or tested here. A document naming a package, schema, generator, verification script, fixture, supported edition or completed slice does not make that artifact available. Such descriptions refer to source-system history. Do not run an absent command or claim its old result as a result of this checkout.

## Audit inputs

Use the glossary, operating model, system outline and design, behavioral topics, ADRs, endpoint map, component catalog and testing requirements as the specification input. [Documentation ownership](documentation.md) defines their roles. They establish requirements, not proof that the proposed architecture is coherent or complete; that is what the first audit must assess.

The first task is [the PStack specification audit](../PSTACK_START.md#first-run-audit-the-specification-no-code). Its findings and proposed build plan do not already exist. A placeholder build-plan page is not an approved implementation sequence.

## Current clock decision

The snapshot predates the accepted 9 October 2026 clock decision. [ADR-0029](adr/0029-use-deployment-clocks-and-preserve-event-times.md) supersedes ADR-0025 and any older requirement for SDK or gateway Core-time offset estimation, offset-derived timestamp rewriting, or clock-uncertainty budgets. Correct clocks are supplied by deployment. Preserve original source event and observation times separately from Core receipt and change times. This is an existing accepted decision carried into the experiment, not a new product choice by the exporter.

The active reporting, SDK, Task, Track, identity and history topics, endpoint and component maps, and testing requirements are reconciled under ADR-0029. Optional nullable `clock_uncertainty_ms` remains inert compatibility metadata with no required initial-path computation or age, Contact or uncertainty-budget effect. Current-process challenges, monotonic expiry, source-time preservation and defensive future-observation handling remain required. Superseded ADR-0025 and historical research retain their original context; they do not restore the rejected mechanism. This documentation reconciliation is not runtime implementation or test evidence.

## Historical references

The dated decision log, Modernization comparison, research reports and component or test qualification READMEs are historical material, not implementation inputs for the independent build. They may retain external links, old issue numbers, paths to absent source, or descriptions of a previous solution. Do not use them to reconstruct that implementation, import its task schedule, or infer completed work here.

The active implementation sequence is [docs/build-plan.md](build-plan.md), initially unproposed. Historical slice names in the technical prose are coverage groupings only. Preserve their underlying behavioral and validation obligations without adopting the old ordering or status. An inherited issue number does not refer to this repository's issue tracker.

For a clean specification audit, do not inspect another repository's source code, test implementations, generated artifacts, PR diffs or commit history. Public primary technical documentation is available to resolve facts about tools and techniques. A source-system code link is provenance, not permission to fetch the old solution.

## Working records

PStack itself belongs in the development environment. No runtime, coordinator, recurring task, model configuration or workflow has been installed or started by these Markdown files. Keep local decision trails, leases and runtime stores out of committed product documentation. Significant authorized decisions belong in the glossary, topic page or ADR that owns them.

No application code, executable schema, fixture, dependency manifest, build script or CI configuration is added by this setup. Later implementation requires its own authorization and real evidence. Source Atlas Core and its existing PRs remain separate from this experiment.
