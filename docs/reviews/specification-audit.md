# Atlas Core specification audit

Status: initial findings recorded on 15 October 2026. The user subsequently authorized fixing F1 through F5 and opening a PR. The [disposition](#disposition-after-authorized-corrections) records those documentation corrections. Accepted behavior is preserved. The [build plan](../build-plan.md) remains a proposal, not application implementation authorization.

## Assessment

The specified system appears implementable within its accepted boundaries. Keep one Core process and SQLite database, independently managed Plugin containers, and the sibling SDK and Protocol. I found no reason to add a scheduler, execution-session service, general event bus or reusable module framework. This is an engineering assessment of the documents, not a runtime result.

The initial audit found stale clock requirements, an unspecified path for escalating a waiting Plugin stop, inconsistent edit-precondition wording and two stale verification summaries. The documentation corrections close those five findings before their dependent workflows. Other open wire formats, resource budgets and qualification work remain engineering tasks. No new product decision blocks the proposed initial direct-IP workflow.

The inspected revision is `c9f4bac1f310112d8d8806e9834acfc7f5b3cff1`. The checkout contains documentation only. Historical command names, pins, fixture descriptions and delivered-slice claims do not establish available software under [documentation context](../documentation-context.md#repository-starting-state). No application tests or capacity measurements ran.

## Scope and method

Read the glossary, documentation ownership, operating model, responsibility map, system design, ADR index and relevant accepted ADRs. Review coverage includes every current behavioral topic and the endpoint map, component catalog and testing requirements together. Exclude historical research reports, prior delivery plans, source-system code, generated output, test implementations, PR diffs and commit history. External provenance links were not used to reconstruct a solution.

Use three read-only investigations for task/report/identity behavior, storage/Plugin/lifecycle behavior and SDK/API/component consistency. The lead checks findings against their cited sections and writes both deliverables. Review records below distinguish separate-agent analysis from independent model-family review.

Throughput checkpoint: n/a, documentation investigation. There is no runtime to benchmark.

## Disposition after authorized corrections

The user authorized the five specification fixes and a ready-for-review PR after inspecting the audit and proposed plan. The corrections below change governing documentation, not application behavior. Original findings and line references below describe the inspected baseline revision, before reconciliation.

| Finding | Documentation disposition | Owning contract |
| --- | --- | --- |
| F1 | Reconcile report, observation, SDK, history and testing clocks under ADR-0029; retain original times, defensive future-time handling and freshness proof | [Shared report context](../topics/asset-reporting.md#shared-report-context), [Contact proof](../topics/asset-reporting.md#contact-proof-and-clock-uncertainty), [Track timing](../topics/tracks-and-geofeatures.md#observation-identities-ordering-and-time), [fault requirements](../testing-strategy.md#fault-and-bandwidth-testing) |
| F2 | Keep inspection, confirmed force stop and whole-installation takeover reachable through the existing serialized owner, with durable retry/handoff recovery | [Waiting-action controls](../topics/dataset-lifecycle.md#controls-during-protected-plugin-drain), [Plugin stopping](../topics/plugins.md#protecting-active-work), [local coordination](../architecture/system-design.md#local-lifecycle-coordination) |
| F3 | Preserve the actual resource precondition; Entity subtypes use expected_edit_revision rather than unconditional If-Match | [Mutation retry summary](../topics/sdk.md#mutation-outcomes-and-retries), [mutation classes](../topics/asset-reporting.md#mutation-classes-and-atomic-validation) |
| F4 | Separate Reset invalidation from retained same-release Restart, including temporary capture exceptions | [SDK evidence](../topics/sdk.md#test-evidence), [snapshot lifetime](../topics/sdk.md#snapshot-lifetime-and-recovery) |
| F5 | Test interruption of one complete data message with no partial image or cursor application | [Queue fixture](../topics/tasks.md#queue-representation-and-coherent-reads), [synchronization contract](../topics/sdk.md#synchronization-wire-and-application-boundary) |

The user further specified that confirmed force stop finishes the original update, uninstall or settings change. The contract requires confirmed Plugin exit before continuation; an unconfirmed exit leaves the action incomplete. Whole-installation takeover supersedes the earlier action instead.

F1 through F5 are resolved in the specification. Their required runtime scenarios are still unexecuted. G1 through G3 remain implementation and measurement work; the build sequence is not automatically approved. G4 remains a historical-labeling recommendation outside these five fixes.

## Prioritized findings

Priority P1 means close the contract before its dependent increment. It does not block unrelated analysis or all implementation. P2 means a local clarification or recommendation. Proposed mechanisms below preserve accepted behavior and are not new requirements by virtue of appearing here.

### F1. P1. Clock supersession has not reached behavioral and testing contracts

Classification: superseded requirements requiring reconciliation, not two equally valid designs.

[ADR-0029, Decision and Consequences](../adr/0029-use-deployment-clocks-and-preserve-event-times.md#decision) assumes correct deployment clocks, preserves original source times and excludes offset estimation, drift correction and clock-failure recovery. [Asset reporting, Shared report context](../topics/asset-reporting.md#shared-report-context), lines 204-209, still makes offset uncertainty behavioral. [Contact proof and clock uncertainty](../topics/asset-reporting.md#contact-proof-and-clock-uncertainty), line 239, requires another offset estimate after a discontinuity. [SDK, Compatibility](../topics/sdk.md#compatibility), line 445, still requires maintaining that estimate. [Testing, Fault and bandwidth testing](../testing-strategy.md#fault-and-bandwidth-testing), line 109, requires its implementation and clock-adjustment fixtures.

Following those passages would restore rejected clock machinery and use the wrong acceptance oracle. Documentation context already settles precedence. This is not a request to reopen the clock decision.

Proposed resolution before increment 1 is one coordinated reconciliation of topics, API links and testing criteria. Use source UTC event/observation times and separate Core receipt/change times. Retain original signed timestamp spellings, unknown historical times, current-process challenge proof, ordering and duplicate rejection. Remove offset estimates, offset-derived rewriting and uncertainty budgets. Optional nullable compatibility metadata may remain behaviorally inert. Spatial measurement uncertainty in metres remains valid.

Preserve the existing defensive treatment of a supplied future observation. Record its source time and value, retain `time_quality: uncertain` and expose no usable fresh age. Do not turn clock reconciliation into rejection of that observation or substitute receipt time. This is boundary handling, not a promise to recover faulty deployment clocks. Unknown original time remains unknown. Retain monotonic token expiry and challenge invalidation on Restart; neither requires offset estimation.

The reconciliation inventory is:

| Source section | Affected lines | Reconciliation |
| --- | --- | --- |
| [Asset reporting, Contact and freshness](../topics/asset-reporting.md#contact-and-freshness) and [Fresh, duplicate and historical reports](../topics/asset-reporting.md#fresh-duplicate-and-historical-reports) | 91, 115 | Replace the superseded clock authority and preserve separate freshness evidence |
| [Shared report context](../topics/asset-reporting.md#shared-report-context) and [Ordering and independent effects](../topics/asset-reporting.md#ordering-and-independent-effects) | 204-209, 231 | Source times and per-quantity age; no offset-budget semantics |
| [Contact proof](../topics/asset-reporting.md#contact-proof-and-clock-uncertainty), [packet fixtures](../topics/asset-reporting.md#independent-packet-fixtures) and Decisions | 235-243, 270-271, 331 | Keep challenge freshness; remove re-estimation and clock-discontinuity qualification |
| [SDK, Connection setup](../topics/sdk.md#connection-setup), [Asset startup](../topics/sdk.md#asset-startup), [Compatibility](../topics/sdk.md#compatibility) and Decisions | 199, 437, 445, 505 | Remove estimation ownership and update startup/ADR wording |
| [SDK, Snapshot allocation retries](../topics/sdk.md#snapshot-allocation-retries) | 236 | Keep finite per-run monotonic expiry and no expired-context resurrection; remove any implication of a clock-discontinuity recovery workflow |
| [Tasks, Command validity](../topics/tasks.md#command-validity-and-deadlines), [Control ordering and expiry](../topics/tasks.md#control-ordering-and-expiry), Open questions, Decisions and Test evidence | 110, 158, 294, 310, 317 | Correct clocks retain Command age/expiry rules; bounded queued coordinate Move To has no execution deadline |
| [Tracks, Silence and observation age](../topics/tracks-and-geofeatures.md#silence-and-observation-age), [Observation identities](../topics/tracks-and-geofeatures.md#observation-identities-ordering-and-time), [Correction acceptance](../topics/tracks-and-geofeatures.md#correction-acceptance), [Movement ingestion](../topics/tracks-and-geofeatures.md#movement-ingestion-and-independent-observation-fixtures) and Decisions | 93, 105-117, 123, 133, 218 | Preserve source identity/time, correction age and defensive future-time acceptance with no usable age; remove offset-budget qualification |
| [Identity, Registration fixtures](../topics/identity-and-access.md#registration-equality-and-independent-fixtures) | 131 | First fresh check-in needs no offset-uncertainty prerequisite |
| [History, Sample contents and timing](../topics/history.md#sample-contents-and-timing) and Decisions | 68, 129 | Keep sparse source times and Core receipt times; replace ADR-0025 dependency |
| [Testing, Required scenario coverage](../testing-strategy.md#required-scenario-coverage) and [Fault and bandwidth testing](../testing-strategy.md#fault-and-bandwidth-testing) | 59, 69-70, 73, 75, 109, 119 | Remove only offset estimation, uncertainty-budget and mandatory host-clock-adjustment qualification clauses; retain all other scenario obligations |
| [Endpoint map, Health and documentation](../api-endpoints.md#health-and-documentation) | 196 | Update the contact-proof link/description after heading reconciliation |

Acceptance evidence for closure is consistent prose and independent correct-clock fixtures for delayed source events, unknown original times, fresh challenge reports and duplicates. A source event never becomes newer because of transmission, correction, process replacement or an unrelated resource edit. ADR-0025 remains historical with its superseded status. User input is unnecessary.

These are clause-level edits, not paragraph deletions. Keep line 59's identity, revocation, gateway and Alias/report cases; lines 69-70's Track-age, Immediate Command and Pause/Resume cases; line 73's synchronization expiry, capacity and recovery cases; line 75's Restart/Reset retention; and line 119's edit/report interleaving and process-claim proof. Explicitly remove line 70's participant-skew operating-mode qualification alongside offset-budget and clock-adjustment recovery clauses. Defensive future-event handling above remains distinct from promising operation with faulty clocks. In asset-reporting line 239 keep current-process proof, monotonic expiry and Restart invalidation while removing the offset interval/re-estimation and clock-discontinuity recovery clauses. In SDK line 236 and testing line 73 keep bounded allocation-context expiry and no resurrection; host-clock rollback need not become a deployment recovery milestone.

### F2. P1. A protected Plugin stop needs an escalation path through the management lock

Classification: missing coordination contract before Plugin lifecycle implementation.

[Dataset lifecycle, Host supervision and private coordination](../topics/dataset-lifecycle.md#host-supervision-and-private-coordination), lines 55 and 59, serializes management behind one installation lock. A second invocation joins the active action or receives `management_busy`. The same section permits indefinite protected draining and calls force stop a separate action. [Plugins, Protecting active work](../topics/plugins.md#protecting-active-work), lines 176-178, promises explicit force stop if cooperation fails.

A literal implementation can hold the lock while waiting and reject the force-stop action needed to end that wait. Whole-installation Stop/Restart/Reset also needs a reachable path, because the same section allows it to classify unfinished work rather than wait for successful completion. This is a specification gap, not an observed deadlock. Unrelated administration receiving `management_busy` follows the explicit global serialization policy.

Proposed resolution is an authenticated, explicitly confirmed escalation addressed to the active management action. The same owner can durably record force stop or whole-installation Stop/Restart/Reset/Hard Reset takeover, resolve the earlier waiting action honestly and execute one serialized lifecycle sequence. Hard Reset retains its explicit destructive confirmation. Inspection and those controls remain callable during draining. Do not introduce an automatic force timeout or concurrent cleanup owner. Other independent management actions may remain busy. Per-Plugin serialization does not itself promise concurrent administration across Plugins.

Acceptance evidence uses separate schedules that hold work indefinitely and then request confirmed force stop, whole-Core Stop, Restart, Reset and Hard Reset from another local client. Each reaches the existing owner without deadlock or concurrent cleanup. Verify honest Interrupted/unknown outcomes, manager-interruption recovery and the documented busy result for unrelated management. The build plan assigns force/Stop and durable actions to increment 4, Restart/Reset to 5, settings-apply controls to 10 and Hard Reset to 11. Core and unrelated Plugins remain usable during independent draining; an explicitly accepted whole-installation action follows its own stopping rules. Core's Plugin policy still owns the domain decision. Reachable controls preserve the accepted lifecycle behavior. The later user decision above settles continuation after a confirmed force stop.

### F3. P2. Entity edit preconditions have two unexplained wire representations

Classification: contract clarification before descriptive editing.

[Asset reporting, Mutation classes and atomic validation](../topics/asset-reporting.md#mutation-classes-and-atomic-validation), line 167, requires `expected_edit_revision`, separate from synchronized resource version. [SDK, Mutation outcomes and retries](../topics/sdk.md#mutation-outcomes-and-retries), line 377, says descriptive patches preserve `If-Match` without defining how it represents that revision.

Separate teams could choose different fields or incorrectly bind the header to the aggregate version. Telemetry would then invalidate an operator's reviewed Alias edit, contrary to the accepted concurrency policy.

Propose retaining `expected_edit_revision` for all Entity subtypes, including Geofeatures. Correct the generic SDK summary to preserve each resource's specified precondition rather than impose unconditional `If-Match`. Other descriptive resources keep their own defined revision contracts. If engineering chooses a header adapter, state its exact mapping and avoid competing preconditions. Verify that an Alias edit based on the same descriptive revision succeeds after telemetry, while a competing Alias edit conflicts. No product decision is required.

### F4. P2. The SDK verification summary conflates Restart retention with Reset invalidation

Classification: contradictory acceptance summary with a settled detailed contract.

[SDK, Test evidence](../topics/sdk.md#test-evidence), line 514, groups Reset and Restart as rejecting old responses, submissions, retries and cursors. [Snapshot lifetime and recovery](../topics/sdk.md#snapshot-lifetime-and-recovery), line 256, and [Query and status contract](../topics/sdk.md#query-and-status-contract), line 324, preserve unexpired ordinary Core paging tokens, complete recovery cursors and retained replay across same-release Restart. Temporary snapshot captures and capture-bound continuations are the explicit exception. Mutation identities also retain their Dataset lifetime.

A literal summary-based test could reject legitimate retries and force unnecessary rebuilds. Split the summary into Reset invalidation, Restart retention and the temporary-capture exception. Verify a retained mutation replay and an unexpired ordinary token after Restart, while the old incomplete capture expires. No product decision is required.

### F5. P2. One queue fixture still describes application-level commit fragmentation

Classification: stale fixture language with a settled delivery contract.

[Tasks, Queue representation and coherent reads](../topics/tasks.md#queue-representation-and-coherent-reads), line 248, describes a related Task/Entity commit split into resource frames. [SDK, Synchronization wire](../topics/sdk.md#synchronization-wire-and-application-boundary), lines 209 and 218, requires a complete commit in one Protocol data message.

The fixture could encourage rebuilding superseded fragment assembly. Replace it with interruption of a single whole data message before validation. Neither resource becomes visible and no cursor advances until the complete message is validated and applied atomically. Transport-level segmentation remains a different concern. No product decision is required.

## Engineering gates and recommendations

These are explicitly unfinished contracts or qualification risks, not newly discovered contradictions.

### G1. Close bootstrap and signed-report wire details with the first workflow

[Identity, Enrollment](../topics/identity-and-access.md#enrollment), [Registration retries](../topics/identity-and-access.md#registration-retries) and [SDK, Asset client](../topics/sdk.md#asset-client) require first registration before a normal Asset credential exists, atomic provisioning and secure recovery after a lost reply. The API map binds this to ordinary Entity creation. Specify deployment-authorization transport, Dataset discovery before first enrollment, credential preparation/retention and retry recovery together. Do not create a one-time secret response that can be lost. These are engineering choices within accepted authorization and retry behavior.

Qualify original validated facts for signatures rather than re-encoding Go UUID/date-time values. [Protocol, Binding representation limits](../../Atlas%20Protocol/README.md#binding-representation-limits) documents that hazard but supplies no executable qualification here. Independent vectors must include timestamp/identifier spellings, omission/null, array order and altered route/target facts.

### G2. Bound accumulated resource images as well as incoming requests

[SDK, Synchronization wire](../topics/sdk.md#synchronization-wire-and-application-boundary) bounds complete commits and requires final resource images. [Tasks, Result declarations](../topics/tasks.md#result-declarations-and-execution-fixtures) bounds each declaration array but allows append-only accumulation and returns all declarations in Task reads. [Queue representation](../topics/tasks.md#queue-representation-and-coherent-reads) includes complete queue state in the Asset image.

Per-request limits alone do not bound accumulated images or the combined Task/Entity message needed by a report. The inferred risk is accepting individually fitting growth and later refusing a supported outcome or cancellation because its complete batch no longer fits. The global precommit guard prevents partial commits but does not itself establish future representability.

Before the relevant schemas admit growth, choose bounded aggregate fields/cardinalities and a combined-message budget that preserves room for supported lifecycle changes. Exercise exact-fit and maximum admitted Task/Entity pairs, terminal reports, cancellation and late declarations. Preserve one complete commit per message. Do not fragment, split atomic effects, drop declarations or delete protected data to fit. Exact limits remain engineering proposals requiring measurement.

### G3. Qualify whole-installation storage and Plugin receipt capacity

[Operating model, Admission bounds](../architecture/operating-model.md#workload-fixtures-and-admission-bounds) already requires retention and extended accumulation. [Plugins, Private dispatch](../topics/plugins.md#private-operation-dispatch-and-reconciliation) keeps exposed live receipts until runtime end and explicitly refuses new invocations at capacity. This is bounded behavior, not an unbounded leak.

Size that capacity against the stated Operation workload before promising sustained lookup availability. Measure Plugin work/evidence, logs, database/index/history growth and active download content alongside Object reservations. Object quota alone does not bound all installation disk use. Retention until explicit Reset is accepted. A failed budget requires an explicit capacity or scope decision, not silent eviction or automatic Plugin restart.

### G4. Keep provisional mechanisms and historical claims out of implementation evidence

The opening paragraphs of [system outline](../architecture/system-outline.md#what-is-decided), [system design](../architecture/system-design.md), [testing requirements](../testing-strategy.md#sdk-and-core-integration-requirements) and [Protocol README](../../Atlas%20Protocol/README.md) still describe delivered foundation work. Documentation context makes their historical status explicit, so this is not an unresolved authority conflict.

Recommend replacing prominent delivery claims with a short historical label when their owning page is next edited. Link current planning references to the new build plan rather than the historical sequence. Do not import old pins, scripts or proofs to satisfy qualification. This recommendation does not authorize changing accepted stack or generation policy.

## Accepted constraints retained

These consequences are intentional and do not justify redesign by omission:

- Core records Task intent and reported outcomes. The Asset OS owns scheduling, interruption and physical execution. Cancellation requested is not proof of cancellation.
- Completion is independent of required-result availability under ADR-0026. Required Object holds last until Reset. Required Entity guards last while relevant Tasks remain nonterminal.
- Retirement can revoke an Asset with unresolved work. It cannot manufacture a terminal outcome or release unresolved required Entity references.
- Publisher ownership, correction history and live geometry are accepted contracts. Publisher transfer and automatic Track expiry remain excluded.
- Operations have their own lifecycle. Interrupted remains terminal even when recovered evidence later arrives. Retry identity does not promise exactly-once external effects.
- One Core process and database permit atomic cross-module effects through explicit interfaces. Seven responsibility groups do not mean seven deployed services.
- Host management must outlive Core to recover Reset/Hard Reset. Its extra process and durable action record follow accepted recovery behavior.
- SDK HTTP mode and Full mode have distinct read sources. Write responses never update the picture. Snapshot captures, replay and local history have different lifetimes.
- An installed system works offline. Lifecycle/configuration administration is local. Broad operational reads do not grant report authorship or administrative authority.
- Retained data survives Restart. Core restart does not promise Mission continuity. Power-loss durability needs evidence beyond process-kill tests.

## Cross-document coverage

| Promise | Behavior and boundary | API/component agreement | Required evidence |
| --- | --- | --- | --- |
| Asset identity and reports | Identity, Asset reporting, shared acceptance | Entity/check-in/status routes; binding, report and movement records | Wrong-authority rejection, original times, deduplication, registration/report races, Restart/Reset |
| Tasking and queue state | Tasks, spatial contract, Entity guards | Task/status/result and assigned-work routes; Tasks-owned queue and result records | Cancellation, reconnect, controls, geometry races, no inferred physical outcome |
| Objects | Objects publication/recovery owner | Upload/content/metadata routes; successful identities, tombstones and holds | Real files/SQLite, crash barriers, retries, quota, download/deletion races |
| Plugins | Plugin policy and host process control | Discovery/Operation routes; Core-owned Operations and historical identity | Real container dispatch, receipt bounds, draining, loss and recovered evidence |
| Synchronization | Core change publication and one SDK picture owner | Full/changed-since/feed; complete changes, capture and retention records | Snapshot/handoff, bounded recovery, capacity negotiation, atomic application and local parity |
| Lifecycle and administration | Dataset lifecycle, local actions, Identity | Local-only management; retained installation records, external action records and activity journal | Real host interruption, establishment proof, setup/Reset/Hard Reset and revocation |

No missing public route was established by the scoped investigations. Complete endpoint schemas and SDK signatures are intentionally future work. The plan must still account for the entire maps, not just the MVP examples. Documentation consistency cannot establish runtime correctness or measured capacity.

## Review and verification record

Three read-only investigations completed on `codex/gpt-6.1-sol`, each dispatched with xhigh effort. A fourth read-only how explainer ran on the same model with high effort and corrected required-reference ownership in the plan. Its synthesis was not another clock-inventory audit. These are separate investigation paths within one model family.

The initial adversarial panel completed on `claudeAgent/claude-opus-5-5`, `codex/gpt-6.1-sol` and `grok/grok-4.7`, all dispatched with high effort. Each reviewed the two documents against current sources and inspected the local decision trail. They returned warnings rather than a runtime verdict.

Accepted review findings changed the documents in five groups:

- Preserve defensive future-time acceptance, monotonic expiry and every non-clock clause while reconciling F1. Claude and Grok identified these gaps.
- Add reachable whole-installation takeover to F2's confirmed force-stop path. Claude identified that escalation alone left Stop/Reset reachability unclear.
- Move Full synchronization after the HTTP-mode MVP and split the large administration/tasking phases into individual workflows. All three reviewers challenged increment sizing or dependencies.
- Name Plugin configuration, Alias/report interleaving, Immediate Commands, Track-age behavior and actual runtime exercise of an externally built extension. The panel found these missing or insufficiently explicit.
- Complete the review/explainer record and append missing trail checkpoints. Claude identified the recording gaps. The original trail entries remain unchanged.

Two proposed extensions were not adopted. Claude's request for concurrent unrelated Plugin administration is not required by the explicit installation-wide `management_busy` policy; F2 instead preserves that policy and adds reachable confirmed controls through one owner. Grok's suggestion to record registration and ordinary reports as activity would expand [the limited activity categories](../topics/history.md#what-is-recorded). Increment 1 now states atomic recording facilities and owner-supplied activity without inventing a telemetry log.

A targeted Claude follow-up confirmed the earlier substantive corrections and identified three remaining groups. The lead corrected the dependency table and introduced durable management records in increment 4, assigned the existing activity categories to their owning workflows, and explicitly classified participant-skew qualification. Hard Reset takeover was added to F2 and increment 11. A final narrow pass checked those edits. No code-comment review is applicable because the change contains no code comments or suppressions.

The second fresh Claude correction pass confirmed those changes and left one activity warning for increment 13. The lead added explicit attributed, atomic, no-duplicate activity evidence for switching Open enrollment and replacing credentials, following [the specified activity categories](../topics/history.md#what-is-recorded) and [Open enrollment](../topics/identity-and-access.md#open-enrollment). This last sentence received lead source review and structural checks, not another independent round. No accepted audit-draft review warning remained unaddressed at that handoff. The later user-authorized specification corrections have their separate disposition above.

### Initial audit verification

The rigor was source-level contract tracing, separate-agent coverage, three model families challenging the draft, two fresh targeted Claude follow-ups and direct artifact checks. This fits a cross-cutting specification audit in a documentation-only repository. It establishes a reviewable proposed plan, not an executed architecture.

The lead inspected the resulting files and changed-file list. The local checker at `/tmp/check-atlas-audit-efa109f6.py` checks local Markdown targets/anchors, whitespace in both documents, long dashes and exact changed-file scope. `python3 /tmp/check-atlas-audit-efa109f6.py` passed for both requested documents and 73 local links/anchors. `git diff --check` also passed for the tracked build-plan diff; it does not inspect the untracked audit. Checks ran again after the final metadata edit. External URL availability, semantic correctness and runtime behavior are outside this structural check.

The append-only local decision trail is `/tmp/atlas-specification-audit-efa109f6.tsv`. Initial cross-model review found recording gaps; later rows supplement them without rewriting earlier entries. The lead re-read this run's transcript and explorer results and checked the trail against the actual actions. The trail and checker stay outside the repository and add no product requirements or toolchain.

Applied principles are Foundational Thinking for the shared commit and data-lifetime prerequisites, Sequence work into verifiable units for workflow ordering, and Prove It Works for direct document/reference checks and explicit unavailable-runtime status. The initial audit changed only this report and the build plan. The later authorized corrections include the governing specification pages listed above.

All application implementation, generated-contract, SDK/Core parity, Docker lifecycle, performance, power-loss and field-readiness results remain unverified. Publication of the documentation PR is now authorized. Installation, deployment and merging remain outside this work.

### Authorized correction review and verification

The user authorized documentation fixes and publication after the initial audit. A separate writer on `codex/gpt-6.1-sol`, xhigh effort, reconciled F1 and F3 through F5 in an isolated worktree. The lead reviewed and imported its ten-file patch after stopping the task, then removed that worktree. The lead wrote the management contract and updated the audit and proposed plan in-thread. Laziness Protocol kept management behind its existing owner and action records. Make Operations Idempotent kept completed replay distinct from resuming incomplete work.

A fresh whole-candidate panel reviewed the corrections, audit and plan. It used the same intent, accepted constraints and rubric. The fourth reviewer started after a child slot became available.

| Delegated step | Provider and model | Result |
| --- | --- | --- |
| Writer | `codex/gpt-6.1-sol`, xhigh | Ten-file reconciliation, reviewed by the lead |
| Reviewer A | `claudeAgent/claude-opus-5-5`, high | Two warnings and six nits; dispositions below |
| Reviewer B | `codex/gpt-6.1-sol`, high | No findings on the captured r1 candidate |
| Reviewer C | `grok/grok-4.7`, high | No findings on the captured r1 candidate |
| Reviewer D | `antigravity/gemini-3.8-flash-high` | Two nits; dispositions below |
| Fresh A correction pass | `claudeAgent/claude-opus-5-5`, high | Original warnings resolved; one incomplete-stop warning and one fixture nit corrected |
| Fresh A final pass | `claudeAgent/claude-opus-5-5`, high | No critical or warning findings; one planning nit noted |
| Final artifact verifier | `codex/gpt-6.1-sol`, xhigh | Documentation artifact pass; runtime could not run |

A identified the substantive gaps; B and C returned no findings, and D returned terminology nits. The fresh A passes and separate final artifact verifier cover the later corrections; B, C and D do not establish independent approval of those later edits.

The lead acted on A's missing build-plan evidence and force-stop continuation ambiguity. Increments 4, 5, 10 and 11 now name their drain-control workflows. The user chose to finish the original update, uninstall or settings application after confirmed Plugin exit. The follow-up warning exposed an incomplete force stop losing its takeover path. The final contract preserves inspection, resumable recorded work and whole-installation takeover through the same owner; unverified exit still blocks startup and destructive cleanup. A's final targeted pass confirmed both corrections. No Act on finding remains from A.

Accepted nits clarify explicitly targeted controls, manager recovery during a waiting drain, concrete out-of-window report timing, increment-0 dependencies and current build-plan links. A's historical-page nit is dismissed because documentation context explicitly supersedes those pages and this reconciliation covers active contracts. A's final planning nit is noted because the authoritative lifecycle fixture already requires takeover after an incomplete force stop; the increment overview does not duplicate every fixture. D's two terminology nits are dismissed. The complete-message rule is explicit in system design and SDK, so its linked "commit frames" summary does not authorize fragmentation. "Core time" still correctly names a Core-owned receipt timestamp, separately from source observation time; it does not restore offset estimation or an installation reference-clock mechanism.

The separate verifier executed `python3 /tmp/check-atlas-spec-fixes.py` and `git diff --check`, and independently checked source contracts and the in-memory application of r1, r2 and r3. All 15 current files matched those patches. Its content digest was `05f0bfdf6f48e25a69844418768d4ee8ae01656f4b08309086f90d84cdc278b0` before this review-record metadata was appended. It reported documentation pass and runtime could not run. The interrupted r2 verifier is not counted.

The lead reran the structural checker after adding this record. It checked 15 changed Markdown files and 966 local link targets, with seven historical fragment checks explicitly skipped. Current-authority anchors, whitespace, scope and active ADR-0025 supersession checks passed. `git diff --check` passed. These checks do not establish external URL availability, measured capacity or runtime correctness. No application check was available. The later review-record metadata received lead review and affected structural checks, not another independent semantic round.

The final candidate remains documentation only. Prove It Works drove direct artifact checks and explicit unavailable-runtime reporting. No code-comment review applies because no code comments or suppressions changed. The append-only correction trail is `/tmp/atlas-spec-fixes.tsv`; the initial audit trail remains separate. Neither trail is a new requirements authority. No application, generated-contract, SDK/Core, Docker, resource-budget or field qualification result is claimed.
