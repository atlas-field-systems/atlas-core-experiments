# Atlas Core agent guide

Use the installed PStack `poteto-mode` workflow. Read a supporting skill before invoking it, and use only runtime capabilities and reviewers actually available. Do not invent a model, tool, independent review or passing check.

## Starting state and first task

The initial repository contains documentation only. Nothing is implemented or tested here. No referenced source path, command, fixture or historical success constitutes an available implementation. Read [documentation context](docs/documentation-context.md) before interpreting the imported system prose.

The initial task is a specification audit and a proposed build plan, not implementation. Follow the first prompt in [PSTACK_START.md](PSTACK_START.md). Do not copy, inspect or use the previous implementation, PR diffs, generated output or tests to supply a solution. External primary technical sources may establish technical facts. Historical links are provenance, not an instruction to fetch old code.

## Read before deciding

Read [GLOSSARY.md](GLOSSARY.md), [documentation ownership](docs/documentation.md), the [operating model](docs/architecture/operating-model.md), [system outline](docs/architecture/system-outline.md), [system design](docs/architecture/system-design.md), [ADR index](docs/adr/README.md), and all relevant [topic pages](docs/topics/README.md). Audit the [endpoint map](docs/api-endpoints.md), [component catalog](docs/data-components.md) and [testing requirements](docs/testing-strategy.md) together.

For implementation, read all of [CODING_STANDARDS.md](CODING_STANDARDS.md). Verification follows [the local verification skill](.agents/skills/verify-atlas/SKILL.md). The current repository has no inherited verifier to run.

## Authority

The user chooses observable behavior; engineering chooses internal mechanisms that preserve it. Accepted requirements and architectural decisions remain binding. An implementation difficulty or reviewer suggestion cannot authorize weakening them. Identify proposals and contradictions; do not silently change a requirement to match code.

Resolve engineering facts independently. Ask only about an actual product choice not settled by the documentation or an irreversible action outside authorization. Continue independent authorized work around that decision. Existing authorization remains valid.

An audit request does not authorize implementation. An implementation request does not by itself authorize publishing or merging a PR. Do not modify the source repository or its issues. Publish a PR only when requested. Merge only when explicitly authorized.

## Evidence

Prefer verifiable end-to-end outcomes and independently authored expectations over scaffolding and test counts. Keep findings, speculation, accepted design and observed behavior distinct. Log significant decisions without turning audit trails into another requirements authority. Update governing documentation when an authorized decision changes it.
