# Contributing

## First review

The initial task is a documentation-only critical review. Read [PSTACK_START.md](PSTACK_START.md), inspect the contracts together, and write prioritized findings plus a proposed build plan. Choose an independent technical sequence; no previous issue schedule is binding here.

Accepted scope, behavior and ADRs remain requirements. Challenge a requirement explicitly when necessary, with the consequences of retaining or changing it. Do not quietly substitute a convenient implementation for the agreed product.

## Implementation work

After implementation is authorized, pick one approved, observable increment. Establish only the toolchain and interfaces it needs, then qualify its behavior and relevant faults through real boundaries. Document real build, formatting and verification commands when they exist. Do not refer to an absent inherited verifier as an executable check.

Give each writer an isolated branch or worktree and bounded scope. Use the installed PStack design, review and delegation mechanisms as warranted. Validate review claims rather than adding abstractions or duplicate tests to satisfy every suggestion. When repeated fixes preserve a faulty premise, reassess the design.

## Work tracking

Use this repository's issues for assigned work once a backlog is authorized. An inherited issue number in system prose refers to a source-system record, not an issue in this repository. Do not copy historical tracker state or close a specification on partial delivery. Reference durable documentation instead of maintaining a second editable contract in every ticket.

## Review and publication

Review the exact candidate against requirements and coding standards separately. Record commands actually run, outcomes, review independence and missing evidence. Changing the candidate invalidates affected evidence.

Publish a PR only when explicitly requested. PStack human landing still publishes a PR, so select it only after publication is authorized. Merging requires separate authorization. Do not change the source repository, its branches, issues or PRs.
