# Atlas field-workflow specification audit

Audited baseline `c9f4bac1f310112d8d8806e9834acfc7f5b3cff1` on 15 October 2026. This is a documentation review. No application exists here. Line references below describe that baseline; subsequent accepted clarifications are identified separately.

## Status after integration with main

This report preserves findings against the audited baseline, separately from the [other specification audit and its corrections](specification-audit.md#disposition-after-authorized-corrections). Those merged corrections resolve this report's F1 clock inconsistency and F4 protected-stop ambiguity. Their runtime qualification remains pending. The current build plan preserves that work and brings the external gateway fixture into the first Core proof.

The ownership and SDK clarifications from this audit are recorded in governing documentation. Concrete relay fields, storage and capacity qualification, and local activity durability remain engineering work. Finding numbers belong to each report and are not shared identifiers.

## Read this first

**Your mental model largely matches the architecture. I would keep its core shape.**

Core is shared operational state with rules for identity, valid changes, task intent, and execution reports. The gateway translates radio traffic. The drone executes. The SDK keeps consumer state current when full synchronization is selected.

The main problems are narrower than “the foundations are wrong”:

- **Revision residue is real.** At the audited baseline, reporting and tests still required a clock subsystem you had already rejected.
- **Your gateway workflow needs the first proof.** A direct-IP demonstration would leave its most important integration unqualified.
- **Some guarantees need concrete engineering.** Storage isolation, management escalation, and durable local activity are unfinished.
- **Several surprising behaviors are deliberate.** They should be visible in the report, rather than accidentally redesigned.

My recommendation is to prove Core and SDK contracts using simulated external gateway and operator consumers first, then add the independent file, Plugin, and synchronized-picture workflows. Gateway implementation and radio transport remain outside Core. The [proposed build plan](../build-plan.md) preserves the accepted scope.

## What you settled during this audit

| Decision | Result |
| --- | --- |
| Drone generates a Track | The drone owns it. An authorized gateway reports for it, including after gateway replacement |
| Gateway scope | Gateway software and radio transport are outside Core; Core tests use simulated external SDK consumers |
| Geofeature changes during a Task | Existing affected Tasks follow the changed geometry |
| Core-managed Plugin containers | Keep robust independent installation, lifecycle, and processing |
| SDK synchronization | Both modes use the SDK. HTTP mode makes on-demand requests; opt-in Full synchronization mode maintains local state for repeated reads |
| Bandwidth | Adequate IP is available. Avoid waste, but constrained-radio optimization belongs at the gateway |

The Plugin system is an intentional way to add and remove functionality without rebuilding Atlas. Full synchronization is an intentional way to implement client-state maintenance once. Their complexity is justified by those goals, subject to real qualification.

The accepted Track ownership clarification is recorded in [identity and access](../topics/identity-and-access.md#asset-owned-track-observations), the glossary, Track/SDK topics, operating model, endpoint/component maps, and gateway testing requirements. Other accepted behaviors remain intact.

## The actual ownership model

| Owner | Responsibility |
| --- | --- |
| Core | Validate and retain operational facts, record Task intent and reported outcomes, commit related changes together |
| Drone runtime | Execute, schedule, interrupt, and retain evidence of what happened |
| Gateway | Translate radio evidence and instructions while preserving drone identity and correlation |
| SDK | Make on-demand requests through HTTP mode or maintain one coherent local picture through Full synchronization mode |
| Plugin | Own an algorithm or data integration behind an independently managed lifecycle |
| Host manager | Perform container and cleanup actions that must remain possible when Core stops |

The [system outline](../architecture/system-outline.md#atlas-core-responsibility-map) already allows one Go Core process and ordinary internal calls. Its seven responsibility groups do not imply seven services. One SQLite database is a sensible starting point.

A Plugin can add a detection algorithm or external-data integration. A new Asset Command or Entity component still requires a Protocol change. Plugin flexibility currently covers processing and integration, not independently extending every part of the vocabulary. See [Commands](../adr/0004-core-owns-commands-and-assets-execute-tasks.md) and [Components](../topics/tracks-and-geofeatures.md#components).

## Findings that need action

### F1. Clock requirements still describe a rejected design

**Resolved in the specification by the main-branch ADR-0029 reconciliation. Runtime evidence remains pending.**

[ADR-0029](../adr/0029-use-deployment-clocks-and-preserve-event-times.md#decision) selects correct deployment clocks and removes SDK offset estimation. Yet:

- [Report fields and freshness](../topics/asset-reporting.md#shared-report-context), baseline lines 204 to 209 and 239, require offset uncertainty and renewed estimation.
- [SDK connection setup](../topics/sdk.md#connection-setup), baseline line 199, still estimates Core time.
- [Track observations](../topics/tracks-and-geofeatures.md#observation-identities-ordering-and-time), baseline line 105, retain old clock semantics.
- [Fault testing](../testing-strategy.md#fault-and-bandwidth-testing), baseline line 109, explicitly requires implementing offset estimation and uncertainty budgets.

**Consequence.** An agent can rebuild the rejected subsystem and write tests that preserve it.

**Resolution.** Reconcile topics, schemas-to-be-authored, references, and test expectations under ADR-0029. Keep original event times, separate Core receipt/change times, unknown original times, ordering, and freshness proof. Optional compatibility metadata must not restore offset-budget behavior.

This audit identified the affected contract. The subsequent main-branch reconciliation resolved it as recorded in the status above; this report did not perform that clock work.

The reconciliation also covers [SDK startup](../topics/sdk.md#asset-startup) and [compatibility](../topics/sdk.md#compatibility), [Command validity](../topics/tasks.md#command-validity-and-deadlines), [movement timing](../topics/history.md#movement-history), and the contact/Track/control/Restart test rows. Update superseded decision links while preserving each still-valid timing guarantee.

### F2. Drone Track ownership was missing from the supported relay boundary

**Gap against the stated field workflow. Ownership resolved during this audit; concrete relay design remains engineering work.**

[Gateway permissions](../topics/identity-and-access.md#radio-gateways) covered bound Asset reports. [Publisher allocation](../topics/identity-and-access.md#publisher-continuity), baseline line 41, did not broaden Asset/gateway writes. [Track creation](../api-endpoints.md#entities), baseline line 49, described interfaces and Plugins.

An ingestion Plugin or an operator credential could publish Tracks, but neither supplies the specific bounded gateway authority in your workflow.

**Resolution now recorded.** The drone owns its observations. Its authorized gateway can relay them. Changing the gateway must preserve that source.

**Remaining work.** Define the concrete relay observation representation, identity mapping, restart/replacement evidence, and Contact interaction. This is expected integration design work, rather than proof that the whole architecture is inconsistent.

This ownership spans Atlas as a whole. Core enforces attribution; the drone and gateway must supply its source identity and evidence. Recording that boundary here does not implement their firmware.

### F3. Object quota alone cannot protect Core's disk reserve

**Priority P2 before capacity qualification.**

[Object quota](../topics/objects.md#storage-quota-and-integrity-faults) protects upload space. [Plugin work](../topics/plugins.md#private-operational-storage), logs, temporary captures, and database growth also consume storage.

**Scenario.** An ingestion Plugin fills its working directory while Objects remains below quota. SQLite can no longer commit a report.

The [operating model](../architecture/operating-model.md#workload-fixtures-and-admission-bounds) already requires explicit failure when the reserve cannot be honored. That prevents false success; it does not isolate independent writers.

**Resolution.** Choose concrete budgets and enforce quotas or storage separation for those writers. Test exhaustion without silently deleting protected results or evidence. Docker bind mounts directly expose host storage; default logging also needs deliberate configuration. [Bind mounts](https://docs.docker.com/engine/storage/bind-mounts/), [logging](https://docs.docker.com/engine/logging/configure/).

### F4. Force escalation during protected stopping is ambiguous

**Resolved in the specification by the main-branch reconciliation. Runtime evidence remains pending.**

[Host coordination](../topics/dataset-lifecycle.md#host-supervision-and-private-coordination), baseline lines 55 and 59, serializes management actions and may report a waiting stop. [Plugin stopping](../topics/plugins.md#protecting-active-work) permits explicit force stop.

It does not clearly say how that escalation reaches an already-pending action. An implementation could reject it as `management_busy`. The text does not prove that the lock must remain held indefinitely.

**Resolution.** Specify force stop as authenticated escalation of the pending action for the same Plugin. Test a noncooperative Operation through real management. Public Operation cancellation remains a separate available path. This needs clarification, not a new scheduler or workflow engine.

### F5. Successful Plugin work consumes cumulative receipt capacity

**Priority P2, explicitly accepted tradeoff.**

[Dispatch rules](../topics/plugins.md#private-operation-dispatch-and-reconciliation), baseline lines 115 to 119, retain duplicate receipts until runtime exit. Completed and acknowledged work still consumes capacity. Exhaustion refuses fresh Operations until a manual protected restart.

The [continuous eight-hour fixture](../architecture/operating-model.md#workload-fixtures-and-admission-bounds) includes 28,800 lookups. Size receipt capacity for the actual runtime duration, separately from concurrent work.

**Resolution.** Qualify the normal workload, expose capacity consumption, and provide shared Plugin-side bookkeeping. Test saturation while retries, cancellation, and reports remain available. Automatic eviction or rotation would change the accepted behavior.

This is the concrete price of the chosen duplicate-protection design. It does not justify removing robust Plugins.

### F6. Small Entity counts hide accumulating evidence and picture size

**Priority P2 before workload claims.**

The [workload](../architecture/operating-model.md#expected-workload) specifies 6,912,000 reports over eight hours. Report identities, observation/correction bindings, movement, and terminal records survive until Reset.

The [full picture](../topics/sdk.md#contents) also includes retained terminal Tasks and ready Object metadata. Bounded replay does not bound these other records.

**Resolution.** Measure persistent growth, duplicate lookup, initial load, rebuild, and decoded SDK memory. Include old picture, replacement staging, and local journal together. Encoded-byte limits are not RAM measurements.

Keep the maintained picture. Qualify its Dataset envelope and explicit capacity failure. Keep SQLite until measured evidence justifies changing it. SQLite permits one writer; slow snapshot consumers must not hold long read transactions that impede WAL checkpointing. [SQLite WAL](https://sqlite.org/wal.html#concurrency).

### F7. Local activity needs a complete durability contract

**Priority P2 with stopped-state administration.**

[The journal](../topics/history.md#local-actions), baseline lines 43 to 47, has import and deduplication rules. Its durable acceptance point, damaged-tail handling, and separate request/outcome identities are less explicit than [management action records](../topics/dataset-lifecycle.md#owned-storage-and-durable-actions).

**Scenario.** A saved change survives power loss while its buffered activity record disappears.

**Resolution.** Define durable recording, repeat-safe import, uncertain outcomes, and damage handling. Share file-writing utilities while keeping history separate from cleanup authority.

## Deliberate behaviors worth keeping visible

These are not newly discovered contradictions:

| Behavior | Practical consequence and source |
| --- | --- |
| Cancellation requires Asset confirmation | An offline request can remain unresolved; later completion can win. [Cancellation](../topics/tasks.md#cancellation-requests) |
| Completion is separate from files | A completed scan can have unavailable results. [Completion](../topics/tasks.md#scan-completion) |
| Retirement preserves unresolved Tasks | Required referenced Entities can remain undeletable until Reset. Geometry is still editable within its guard. [Required references](../topics/tracks-and-geofeatures.md#required-entity-references) |
| Issued geometry revision is retained | The old polygon payload need not be inspectable. This limits historical reconstruction. [Geometry](../topics/tracks-and-geofeatures.md#geometry) |
| Cancellation keeps an excluded queue slot | Reordering A,C to C,A can later restore declined B as C,B,A. [Queue fixture](../topics/tasks.md#queue-representation-and-coherent-reads), baseline line 243 |
| Write confirmation precedes picture delivery | A successful write can briefly coexist with older local reads. [Write results](../topics/sdk.md#write-results-and-the-picture) |
| Core Restart retains records | Active Mission continuity is excluded; unfinished Plugin Operations become Interrupted. [Lifecycle](../topics/dataset-lifecycle.md#unfinished-work-after-stop-or-restart) |
| SDK has no offline mutation queue | The operator must reach Core to submit. Asset radio disconnection is a separate case. [Task creation](../topics/tasks.md#task-creation) |

If historical geometry inspection or retiring a lost drone without retaining deletion guards matters, those need separate product decisions. They are not prerequisites for finishing this audit.

## Small clarifications, not architectural blockers

- Local full-query recovery should return coherent resources and their paired local cursor atomically. An unpaged in-memory read can satisfy this. Add an interleaving fixture rather than another snapshot framework. [Local subscriptions](../topics/sdk.md#local-subscriptions-and-rebuilds).
- HTTP snapshot allocation uses WebSocket for explicit release. Finite expiry bounds retention when that channel is unavailable. This is transport coupling, not a proven leak. [Snapshot lifecycle](../topics/sdk.md#snapshot-lifetime-and-recovery).
- Generator output is not runtime validation. Prove operational wire behavior and clean regeneration on the first real route. [Generation](../architecture/system-design.md#public-wire-conventions), [oapi-codegen validation](https://github.com/oapi-codegen/oapi-codegen#requestresponse-validation-middleware).
- Imported “delivered foundation” claims describe another starting state. [Documentation context](../documentation-context.md) overrides them. Carry over behavioral obligations, not old successes or ticket ordering.

## Review and verification

Three separate Codex explorers covered domain/reporting, Plugin/lifecycle/storage, and SDK/Protocol/testing. A fourth Codex agent synthesized them. Claude Opus 5.5 independently challenged the draft and decision trail.

That review correctly challenged several severities. This final report treats force escalation as ambiguity, storage as qualification work, and local snapshot/cursor pairing as a small clarification. It preserves the user's confirmed choices.

Two fresh Claude Opus 5.5 review rounds were read-only. The second checked the ownership edits across the governing documents and found them consistent. Its requested separation of Task evidence from Track ownership is incorporated above and in the initial Task workflow. Minor finding labels and the decision trail were corrected too.

Verification used the actual Markdown files at the baseline plus this working-tree diff:

- `git diff --check` checks whitespace and patch formatting.
- `python3 /tmp/atlas-spec-audit-20261015-verify.py` checks permitted file scope, relative file links, heading targets, and long-dash characters in the new deliverables.
- Source checks compared the findings with the owning topics and accepted ADR supersession. Endpoint, component, and testing additions were checked together.
- The current thread's recorded user answers were checked against the decision trail. Its corrections are append-only. The trail is local at `/tmp/atlas-spec-audit-20261015-decisions.tsv`.

Both initial verification commands passed across ten Markdown files, including the two deliverables and the governing documentation for the user's clarifications. Relative file and heading links resolve; the new deliverables contain no long-dash characters. Desktop and mobile previews of the in-chat summary showed no console errors.

The user subsequently approved four documentation clarifications: keep gateway and radio implementation outside Core's proof, make SDK use explicit in both modes, describe adequate IP bandwidth, and keep audit history out of behavioral pages. This follow-up was self-reviewed; the independent reviews above preceded it. `python3 /tmp/atlas-doc-clarifications-20261015-verify.py` checks the expanded documentation scope and links. It and `git diff --check` passed. Exact comparisons also confirmed that the SDK's clock-related sections and the fault-testing section were unchanged by this follow-up.

No application tests, physical-radio experiments, power-loss tests, or benchmarks have run. No historical implementation, test source, generated output, PR diff, or external repository history supplied a solution.

Experience First changed the priority to the actual gateway workflow. Laziness Protocol kept ordinary module owners and avoided new frameworks. Guard the Context Window partitioned source coverage. Sequence Work into Verifiable Units shaped the build plan. Prove It Works requires actual source and artifact checks, without borrowing historical successes.

The independent trail review flagged outdated decisions and overstated severity in the working draft. Those are corrected in the final documents and closing trail entries. At the end of the original audit, concrete gateway fields, clock reconciliation, and runtime qualification remained open work. The integration status above records subsequent contract corrections.

| Delegated step | Actual provider and model |
| --- | --- |
| Domain/reporting explorer | `codex/gpt-6.1-sol` |
| Plugin/lifecycle/storage explorer | `codex/gpt-6.1-sol` |
| SDK/Protocol/testing explorer | `codex/gpt-6.1-sol` |
| Architecture synthesis | `codex/gpt-6.1-sol` |
| Independent draft/trail review | `claudeAgent/claude-opus-5-5` |
| Focused ownership/revision review | `claudeAgent/claude-opus-5-5` |

## PR integration verification

The publication candidate was rebased onto the current documentation on main. Its existing audit is retained separately, along with the clock, protected-drain, edit-precondition and synchronization corrections. The full 23-increment build sequence remains, with the external gateway proof brought into increments 1 and 2 and a separate direct-IP signature/challenge proof retained in increment 1.

A fresh read-only review by `claudeAgent/claude-opus-5-5`, high effort, found mixed direct-IP and relay proof, deferred early relay prerequisites, missing decision provenance, a rewritten historical entry and an incomplete update-route summary. The lead corrected those passages, recorded the accepted ownership in ADR-0030 and restored the dated ADR-0020 history. The review did not choose a new one-gateway-per-Asset restriction. Current bindings still govern authority; withdrawal denies further writes, and gateway changes preserve source-scoped observation identity and ordering. Relay wire and binding-change mechanisms must close before their first proof.

`python3 /tmp/atlas-field-workflow-pr-verify.py` checks the publication scope, local links and headings, exact retention of the upstream clock sections and other corrected files, and preservation of the build sequence. `git diff --check origin/main` checks whitespace. Both passed after the corrections. There is no application runtime to verify. No code comments or suppressions changed, so the comment pass has no code scope. A fresh focused review by the same provider and model confirmed the corrections and reran both checks successfully. It noted two nonblocking consistency issues: the ADR deadline needed to specify the first Track relay proof, and increment 2 needed to name its Contact and binding-withdrawal checks. It also identified direct-IP Task evidence that the lead restored alongside the gateway schedules. These final narrow edits and this review record received lead review and rerun structural checks, not a third independent round. Both review rounds reported runtime could not run because no application exists.
