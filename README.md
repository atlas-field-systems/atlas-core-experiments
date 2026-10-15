# Atlas Core experiments

Atlas Core is a control plane for observing Entities, preserving operational data and tasking Assets. This repository is the documentation-only starting point for an independent PStack implementation experiment.

**Nothing is implemented here.** There is no application source, executable Protocol schema, test suite, fixture, generator, build setup or CI. Behavioral contracts and required test scenarios describe the target system, not shipped features. Historical documentation does not establish capabilities in this repository.

## Start here

| Goal | Read |
| --- | --- |
| Start the specification audit | [PStack starting prompts](PSTACK_START.md) |
| Find the system requirements | [Documentation index](docs/README.md) |
| Understand scope and decision authority | [Agent guide](AGENTS.md) and [Contributing](CONTRIBUTING.md) |
| Distinguish requirements from historical evidence | [Documentation context](docs/documentation-context.md) |
| See the proposed implementation sequence | [Build plan](docs/build-plan.md), to be written by PStack |

The first deliverables are `docs/reviews/specification-audit.md` and PStack's own build plan. Audit first. Do not implement, copy an earlier solution or import its task schedule during that run.

This repository has its own documentation-only history. Work here does not modify Atlas Core or its open PRs. PStack must be installed in the development environment separately; these files do not start an agent or coordinator.
