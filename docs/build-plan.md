# Proposed independent build plan

Status: proposed on 15 October 2026. Nothing is approved for implementation, implemented or verified. This plan authorizes no publication or merge.

Build one useful workflow at a time through the real SDK, Core and storage. Introduce shared facilities when that workflow needs them. The [specification audit](reviews/specification-audit.md) records contract corrections and engineering gates. The [field-workflow audit](reviews/field-workflow-audit.md) records the drone, gateway and operator workflow and accepted ownership. No historical implementation, test corpus or ticket sequence supplies this plan.

Prove the primary field workflow with simulated external gateway and operator consumers through real SDK/Core/storage boundaries. Gateway implementation and radio transport are outside Atlas Core; a radio emulator is not required for this Core proof.

## Planning basis

Keep the accepted [stack](adr/0016-use-go-sqlite-and-openapi-tooling.md), [deployment](adr/0017-deploy-core-and-plugins-as-docker-containers.md), [ownership](architecture/system-outline.md#atlas-core-responsibility-map) and [deployment clocks](adr/0029-use-deployment-clocks-and-preserve-event-times.md). The supported initial host is Linux with systemd, Docker and specified local storage.

The [initial MVP](architecture/operating-model.md#initial-mvp) comprises independent Move To, Object transfer and Elevation Lookup examples plus their [lifecycle checks](testing-strategy.md#mvp-integration-checks). It can finish in HTTP mode after increment 5. Full synchronization remains required for the complete system, with its own milestone in increment 6. A completed HTTP-mode MVP does not establish Full-mode parity.

Complexity estimates are engineering judgments, not dates. Medium means a bounded workflow using existing collaborators. High adds several transactional or integration boundaries. Very high adds coupled recovery across processes or picture synchronization. Re-estimate from real evidence after increments 1, 4, 5 and 6.

## Sequence and dependencies

| Increment | Observable outcome | Dependencies | Complexity |
| --- | --- | --- | --- |
| 0 | The next workflow has consistent contracts and independent expectations | Audit disposition | Medium |
| 1 | A bound gateway reports drone state retained through a Core process restart | 0 for time, identity, relay authority, edit and producer bounds | High |
| 2 | A drone-owned detection is retained and queued coordinate Move To completes or reconciles cancellation | 1 | High |
| 3 | Known file content publishes once and downloads unchanged | 1 | High |
| 4 | An offline elevation Plugin runs and stops independently under host management | 1 | Very high |
| 5 | The composed installation preserves Restart data and completes ordinary Reset | 2, 3, 4 | Very high |
| 6 | Full-mode SDK reads and recovers a coherent operational picture | 2, 3, 5 | Very high |
| 7 | An administrator withdraws an Asset without inventing execution outcomes | 2, 5 | High |
| 8 | Replacement Asset authority reconciles retained execution without reruns | 2, 7 | High |
| 9 | Local Core settings save/apply with explicit recovery | 3, 5 | High |
| 10 | Plugin settings distinguish saved, active and last-working revisions | 4, 5 | High |
| 11 | Hard Reset clears only owned state and returns to first setup | 5 | High |
| 12 | Explicit release update preserves installation identity and replaces the Dataset | 5, 9, 10, 11 | Very high |
| 13 | Lost-credential repair and exact-binding test-identity cleanup preserve denials | 7, 11 | High |
| 14 | Publishers maintain Tracks and operators edit Geofeatures safely | 4, 6 | High |
| 15 | Requested queue edits and confirmed adoption reconcile coherently | 2, 6 | High |
| 16 | Immediate Commands and Pause/Resume preserve queued execution rules | 2, 6 | High |
| 17 | Live geometry and required Entity guards survive edits and disconnection | 2, 14, 16 | High |
| 18 | Result declarations protect Objects independently of execution outcomes | 3, 17 | High |
| 19 | A Command observes its declared Track-age and stale-data behavior | 0, 2, 14 | Medium |
| 20 | The gateway contract covers the remaining control, recovery and result workflows | 8, 13, 15 through 19 | High |
| 21 | Remaining administrative and documentation operations have explicit contracts | 5, 9, 10, 11 | Medium |
| 22 | The composed release has recorded compatibility, capacity and durability evidence | All applicable implementation increments | High, with external dependencies |

These are independently reviewable increments, not seven module-scaffolding phases. The numeric order is a proposed working order; the dependency column identifies actual prerequisites. Increments 3 and 4 can proceed independently after 1. Any parallel implementation needs separate file ownership and isolated writable checkouts.

When synchronization exists, each new writable resource joins its snapshot/replay and SDK parity scenarios in the same increment. Before then, record complete committed changes and retained-state evidence without claiming a Full-mode implementation. Each state owner joins retained/fresh opening and lifecycle coverage when introduced. Required correctness evidence never waits for increment 22.

## 0. Reconcile contracts needed by the next workflow

The documentation corrections resolve F1 through F5 under the accepted behavior, as recorded in the audit's [disposition](reviews/specification-audit.md#disposition-after-authorized-corrections). Their runtime acceptance evidence remains pending in the corresponding increments. Close G1 and the gateway enrollment/report representation before increment 1. Before increment 2, define Track relay fields, source-to-publisher mapping, binding-change authorization, rejection after binding withdrawal, source-scoped observation correlation across gateway restart/replacement, and Track-write/Contact interaction. Distinguish direct-IP signatures/challenges from trusted gateway attestations. These gates cannot wait until increment 20. Set resource and combined-message admission bounds before writable producers grow. Other engineering details close immediately before their dependent workflow.

Evidence is consistent owning prose, API/component-map agreement, independent request/result examples and explicit finding dispositions. Preserve mixed paragraphs' non-clock requirements and accepted defensive timestamp behavior. No runtime or dependencies belong here. Proposals remain proposals until the appropriate authority accepts them.

## 1. Observe one drone through a gateway

A simulated external gateway uses its own authenticated SDK instance to register a stable bound drone and establish its downstream process authority. It constructs authenticated Core-facing reports from supplied drone evidence with original event times. Gateway attestations follow ADR-0028; the drone need not supply originating Core-format signatures or challenges. A simulated operator reads it through SDK HTTP mode. These are external-consumer test fixtures, not gateway software delivered by Core. Duplicates have no second movement or Contact effect; historical evidence keeps source times and establishes no new Contact.

Use the SDK's supported Asset client with supplied drone evidence. Reject unbound relay and keep gateway liveness separate from downstream Contact. Qualify trusted relay separately from direct-IP signature and challenge proof.

Keep a separate direct-IP fixture in this increment. Its SDK Asset client registers a stable Asset, obtains a candidate-generation challenge and sends a signed current report. A real operator SDK reads it. Prove first-process establishment, fresh Contact and signed-report acceptance on this path; gateway attestation is not a substitute.

Introduce the minimum real build and verification entry point, authored Protocol and SQL, clean generation, SDK HTTP access and Core-owned SQLite opening. Use an isolated real Core process and owned persistent files. Prepare credentials before submission and provision through Core's authenticated private maintenance boundary. A test launcher can supply the local management peer; it never opens SQLite. Exercise real HTTPS trust, without claiming an installed manager or full first-use deployment. Host-managed Start/Stop/Restart arrives in 4 and 5.

The Dataset commit appends complete public changes and any activity records supplied by the owning action atomically. Ordinary registration and telemetry do not become a new activity-log category. Test credential activity where applicable. Include a supported Alias edit using the separate descriptive revision.

Evidence compares independently authored direct-Protocol and SDK scenarios. Cover lost/concurrent registration across Asset restart, invalid initial fields, changed retry facts, two-Asset impersonation rejection, omission/null, Unicode and duplicate-key rejection, original signed spellings on the direct-IP path, current/obsolete generations and unknown times. Interleave telemetry with Alias edits: an unchanged descriptive base still succeeds, a competing edit conflicts, and no edit overwrites reporting. Kill before/after commit and open retained. This proves retained opening, not the later host lifecycle workflow. Check TLS failures and secret redaction. Exclude Tasks, files, Plugins, Full mode and radio encoding.

## 2. Execute coordinate Move To

The operator fixture submits queued Move To through the SDK. The gateway fixture retrieves the assigned Task and submits controlled drone progress and outcome evidence through its SDK instance. Radio delivery and drone execution remain external responsibilities. Use the [independent spatial fixture](topics/spatial-data.md#move-to). This initial coordinate workflow has no execution deadline. Core neither infers arrival from telemetry nor schedules execution.

First publish a minimal detection owned by the drone through its bound gateway and read it through the operator SDK. Preserve observation identity through retries and authorized gateway replacement. Reject unbound relay and cross-publisher overwrite. Withdraw a relay binding and reject that gateway's subsequent observation writes. Verify that Track-only writes do not refresh Asset Contact. This supplies the first Track observation path; full publisher and geometry workflows arrive in 14. Task evidence remains governed separately by the Asset reporting contract.

Add Tasks-owned transitions, creation retry identity, submission sequence, the basic queue aggregate and assigned-work pagination. Task reports pass shared acceptance in the same commit as Task, Entity and public-change effects. Include cancellation request, confirmation and decline, offline issuance/cancellation and ordinary same-process reconnect.

Evidence proves one Task after concurrent/lost-response retries, permitted state-model sequences, delayed outcomes after newer telemetry and both cancellation/outcome orders. Use 5.1 m and 4.9 m fixture progress without making the simulator's arrival threshold a Core rule. Reads do not acknowledge or start work. Retirement/deletion belongs to 7 and uncertain process replacement to 8.

Repeat the Task submission, retrieval, progress, outcome, cancellation and same-process reconnect schedules with a separately authenticated direct-IP Asset SDK client and signed Task reports. Gateway attestation does not substitute for this direct-IP evidence.

Task issuance and cancellation record their attributed activity with the accepted mutation. Matching retries add no second logical action. Verify the actor and retained cancellation history through the activity API.

## 3. Transfer an Object independently

Upload known replayable content and download identical bytes from one ready Object. Add Objects-owned staging, distinct physical attempts, digest verification, file-first durable publication, retry/deletion identities, quota reservations, metadata edits and download leases.

Evidence uses real SQLite and supported filesystems. Interrupt staging, installation and FULL commit at specified barriers. Lose success replies, retry from zero, race identical attempts and reject changed original facts. No partial Object appears. Exercise revocation and Dataset change during streaming, missing committed content, cleanup failure and download/deletion ordering. Upload admission refusal leaves ordinary reporting usable. No Task or Plugin dependency is added.

Propose attachment delivery for initial /view behavior, already permitted by the contract. Inline formats need an explicit contract and safe responses. Process kills do not prove power-loss durability. Required-result holds arrive in 18.

## 4. Invoke and independently stop the elevation Plugin

Install an offline bundle and invoke elevation_lookup through the SDK. Retrieve the same durable Operation after disconnection or a lost acceptance reply. Run its package in a sibling container with SDK access, private dispatch and owned work/reference mounts.

Introduce the installation-specific host manager, authenticated private sockets, local first-use setup and managed Core/Plugin Start/Stop. Its external durable action records support interrupted management from this increment; increment 5 extends them with Reset establishment and cleanup. Complete the offline CA/key bootstrap through this owner. Core owns Plugin policy; the host owns process actions and outlives CLI exit. Neither Core nor Plugins receive Docker control, and Plugins receive no Core storage.

Implement receipt reservation, exposure-before-send, durable unacknowledged evidence, fencing, cancellation, recovered outcomes and no automatic rerun. Add local install/start/stop/update/uninstall with protected draining and the F2 escalation/takeover resolution. Verify confirmed force stop and targeted whole-installation Stop from a second local client during an indefinite drain, including lost replies and manager interruption. After force stop confirms exit, an update or uninstall finishes its original recorded intent; unconfirmed exit leaves the action incomplete.

Evidence checks corners, centre 130 m, interior 125 m and out-of-coverage against the [elevation fixture](topics/spatial-data.md#elevation-lookup). Exercise same-runtime reconnect, full receipt stores, delayed duplicates, invalid output, cancellation/exposure races, runtime loss and manual replacement. Interrupted stays terminal after recovered evidence. Keep Core and another Plugin usable during independent management. Verify historical Operations after same-name reinstall, CLI exit, unexpected Core loss and incomplete writer shutdown. Qualify receipt capacity for the stated workload. Full Plugin configuration recovery belongs to 10.

Plugin changes record the authenticated local actor, accepted request and later known outcome under one action identity. Database effects and their activity commit together; no transaction spans a container effect. Retry and manager-interruption recovery create no duplicate logical activity.

## 5. Complete retained lifecycle and ordinary Reset

Use the real shared CLI/TUI management interface for Start, Stop, Restart and Reset across the three examples. Extend the durable action records from 4 with Core's independent establishment proof, Reset cleanup and stopped-state activity journal import. Provide real profile create/read operations so retained-profile evidence is not a private fixture insertion.

Evidence interrupts each ordinary Reset phase, loses the fresh-commit reply, preserves replacement-Dataset Plugin work and clears every noncurrent Object directory. Check completed-result replay/expiry, unreadable authority, startup faults, writer-exit verification and duplicate journal import. Restart retains records, Objects, setup and logs. Reset clears Dataset state, work, history and managed logs while retaining setup, profiles and denials. Old requests cannot populate the replacement Dataset. In separate schedules, Restart and Reset each take over an indefinitely waiting Plugin stop through the same manager owner, without concurrent cleanup.

All [MVP checks](testing-strategy.md#mvp-integration-checks) must pass together through HTTP mode, real SDK/Core, SQLite/files and relevant containers. The core contract MVP can finish here. Hard Reset, configuration editing, release conversion and Full mode have separate gates below.

## 6. Recover a coherent Full-mode picture

Implement the one SDK picture owner, server capture and ordered catch-up/live workflow, bounded replay, local journal, queries, status and retry control. Cover all then-writable Entities, Tasks and ready Objects. Plugin discovery, Operations and history retain their explicit HTTP read behavior.

Evidence follows the [worked interleaving](topics/sdk.md#worked-synchronization-interleaving) and [snapshot fixtures](topics/sdk.md#snapshot-lifetime-and-recovery). Include writes during paging, atomic Task/Entity batches, deletions, response/feed arrival in both orders, one-capture capacity, lost allocation/subscribe/release replies, slow consumers, expiry and bounded recovery. Check complete staging across reconnect, incomplete captures expiring on Restart and ordinary retained tokens remaining valid. No partial application, write-response picture mutation, application-read HTTP fallback or selective/hybrid mode is added.

## 7. Withdraw Asset participation

Entities owns retirement/deletion admission and commit coordination. Tasks supplies its nonterminal guard; Identity owns denial/revocation and Synchronization owns feed cut-off.

Evidence races assignment, provisioning and reports against both actions. A failed ordinary deletion leaves credentials usable. Retirement succeeds with unresolved work while preserving evidence and guards. Verify first/replay/conflict/deleted-result distinctions, fresh already-retired rejection, buffered-feed cut-off, Restart retention and denial after Reset. Re-run protected-result/reference cases when 17 and 18 add those resources. Neither action establishes physical stop or a Task outcome.

Retirement records its attributed activity in the same commit. Matching retries and fresh already-retired rejections create no duplicate retirement action. Check that activity through the public history read.

## 8. Recover replacement Asset authority and uncertain execution

A replacement process establishes authorized generation once and reconciles supplied Asset OS execution evidence. Ordinary reconnect remains distinct from process replacement.

Evidence races claims, loses the winning response, rejects obsolete signatures/credentials and records previously unknown valid outcomes without rerunning work. Unknown execution holds its queued continuation back for explicit recovery. Core keeps its recorded Task state and exposes uncertainty. A transfer proves authority, not that the old process physically stopped. Use real SDK/Core and retained caller-owned identities.

## 9. Save and apply Core configuration

Local management owns saved/effective settings, reviewed revisions and lifecycle serialization. Evidence rejects invalid merged candidates atomically, conflicts stale saves, distinguishes saved from applied values and recovers after a lost apply result.

Exercise live limits preserving admitted transfers, quota including cleanup/download bytes, deployment edits, failed startup and explicit stopped-state correction/restoration. No automatic fallback, secret disclosure or public configuration API is added. Restart/Reset preserve valid settings.

An accepted settings edit and its attributed activity commit together. A stale/invalid candidate creates neither. Lost-response replay and reapplication add no new edit action; later process outcomes keep the original action identity.

## 10. Save and recover Plugin configuration

Implement the [Plugin configuration contract](topics/plugins.md#configuration) independently of Core configuration. Keep saved, active and last-working revisions and immutable candidate facts separate.

Evidence covers stale/missing save bases, invalid schema/secrets, failed or timed-out apply, matching authenticated readiness and apply while disabled returning saved-not-started. Preserve the failed candidate. Deliberately restore a last-working revision or correct and apply a new one; no automatic fallback, enablement, restart or Operation rerun occurs. Repeat interruption and local result recovery through the management owner. Verify confirmed force stop and whole-installation takeover remain reachable while settings application waits for a protected drain. Confirmed force stop then finishes the recorded settings apply; failed exit verification leaves it incomplete without activating the candidate.

Record attributed Plugin configuration activity with the corresponding accepted database changes and link apply outcomes by action identity. A rejected save or matching action retry creates no additional logical activity and exposes no secret.

## 11. Complete Hard Reset

With Core and Plugins running or stopped, explicitly confirmed Hard Reset clears all and only owned operational/setup/log/artifact resources. The external marker survives interruption and prevents startup until cleanup finishes.

Evidence preserves Core software and unrelated resources, verifies writer exit and interrupted cleanup, then returns to first setup. Old credentials, Dataset submissions and retained Reset results cannot affect the new installation. No physical Asset stop or external-effect rollback is claimed.

Also request confirmed Hard Reset while a protected Plugin stop waits indefinitely. The existing manager takes over through F2's serialized control path rather than returning busy forever or running concurrent cleanup.

## 12. Perform explicit release update

A local update preflights the target image, formats, settings and installed Plugins, then uses the ordinary Reset establishment path. Add a reviewed installation-only converter only for an actual fixture transition.

Evidence covers same-release schema drift, release mismatch, unsupported formats/settings, Plugin incompatibility and interruption on both sides of the conversion/fresh commit. Preserve principal IDs, verifiers, profiles, publisher authority and denials. A lost commit reply cannot authorize repeating cleanup or conversion. Ordinary Start neither migrates operational data nor silently wipes setup.

## 13. Repair credentials and clean test identities

Use local management to replace lost credentials/recovery authority without changing the retained Asset principal. Open enrollment remains off by default and records immutable enrollment provenance.

Evidence follows exact-binding cleanup, including current Entities with unresolved Tasks, identities without Entities after Reset, changed bindings, lost replies and partial multi-target success. Re-provisioning does not clear provenance. Cleanup revokes selected identities without inventing outcomes and preserves unselected ones. A stale pre-Hard-Reset selection cannot revoke a newly provisioned binding. Health's warning count is advisory, not readiness.

Switching Open enrollment and replacing credentials each record attributed activity atomically with their Core-owned changes. Matching retries create no second logical action. Verify both through the public history read, alongside cleanup's existing retirement/revocation activity.

## 14. Maintain Tracks and Geofeatures

Extend the minimal drone-owned observation path from increment 2. A publisher creates and corrects observations without changing their age; a different publisher cannot overwrite them. An operator creates/edits a Geofeature using a separate descriptive revision and issued geometry revision.

Evidence covers sparse quantities, unknown/source times, independent observation groups, identity conflicts, correction history, no duplicate movement and publisher reinstall with fresh local proof and credentials. Preserve attribution, deleted IDs and historical associations. Qualify alias rules, component applicability and spatial/GeoJSON conversion. Extend Full-mode parity and lifecycle coverage. Exclude publisher transfer, automatic expiry, fusion and Plugin-defined components.

## 15. Reorder queued work

An operator edits complete eligible order and the Asset reports adoption or conflict. Tasks owns requested/confirmed state and projects its aggregate into the Asset.

Evidence interleaves queue edits with starts, new Tasks, cancellation/decline and reconnect. Preserve retained slots, reject wrong membership, prevent old confirmations rolling back newer evidence and invalidate changed assigned-work pages. Requested order never implies adoption or execution. Use the existing complete-message and shared-report paths.

## 16. Execute Immediate Commands and Pause/Resume

Define the necessary Protocol Command fixtures before implementation. An immediate lights action can coexist with Move To; advertised dual-mode Commands can also queue. Pause and Resume remain immediate control Tasks with their accepted ordering and expiry.

Evidence covers no-current-work controls, suspension/resumption, failed or unsafe Resume, independent work while paused, duplicate/delayed controls, newer applied intent, cancellation decline on paused work and no universal timeout. An expired Resume cannot release the queue; delayed Pause remains non-expiring unless superseded. Use correct source times, without offset estimation. Asset execution policy remains outside Core.

## 17. Follow live geometry and enforce required Entity references

Tasks owns required-reference meaning and execution evidence. Entities enforces deletion protection and coordinates deletion/admission. Geofeature edits do not create or reissue Tasks.

Evidence exercises both deletion/admission and geometry-edit/finish commit orders, invalidating edits before cutoff, disconnected latest-geometry delivery and actual adoption reports. Accept valid older issued geometry, retain collection-finished evidence and reject unknown/future/unrelated revisions. Preserve pause/cancellation and terminal outcomes. The last relevant terminal Task releases its Entity guard, even if result delivery remains unavailable.

## 18. Declare results independently of outcomes

Assigned Assets append bounded result declarations before or after completion and publication. Tasks retains references; Objects owns required-result holds and publication. Hold accumulation and complete Task/Entity images obey the producer budgets from G2.

Evidence covers both declaration/upload orders, late declarations, mixed ready/unpublished IDs, deletion races, invalid declaration batches and permanently unavailable results. Independent completion remains acceptable. Cancellation does not stop valid uploads. Required Objects remain protected until Reset, independently of released Entity guards. No placeholder Object or publication-driven Task transition is added.

## 19. Use Track age during execution

Specify a concrete Protocol Command fixture's age limit and stale-data behavior under the [Track contract](topics/tracks-and-geofeatures.md#track-data-used-by-commands). This is not a system-wide expiry policy.

Evidence supplies fresh, stale and unknown original observation times through the real SDK/Core/simulated-Asset workflow. Age can cross the declared threshold during disconnection; a permitted last-known policy can continue. Corrections, Alias edits and receipt do not refresh the observation. Core never infers a Task outcome from age. Increment 0 precedes these tests.

## 20. Qualify the Core-facing gateway contract

Extend the gateway fixtures from increments 1 and 2 to the remaining downstream work and recovery cases without replacing Asset authority merely because the gateway restarted. The initial enrollment, delegation, generation, evidence-correlation and freshness representation is established before increment 1 under ADR-0028; complete its later-workflow coverage here.

Evidence rejects unbound relay, obsolete authority and old-Dataset work across reporting paths. Gateway liveness never establishes downstream Contact. Broad operational reads remain distinct from relay authority. Preserve process generations across gateway restart, stable retries and all supported Task/result/control meanings. A faithful integration fixture proves this Core-facing contract, not firmware translation or RF behavior.

## 21. Complete remaining administrative operations

Complete API-key management, Operator CRUD/settings, activity filtering, diagnostics and protected offline documentation. Earlier increments supply only the subsets their real workflows require.

Map every remaining [endpoint](api-endpoints.md), [SDK operation](topics/sdk.md#operations-catalog), [component](data-components.md) and local management action to its caller, input, source, result, retry, pagination and error evidence. Verify deletion attribution, credential secrecy/revocation, exact supported editions and unsupported refusal. List deliberately unwrapped routes. Public generation contains no Core configuration or Plugin-management APIs. Neither /admin/resources nor profile fields inherit an executable schema from historical prose.

## 22. Qualify the composed release

Run the [workload profile](architecture/operating-model.md#workload-fixtures-and-admission-bounds) through real boundaries. Include retained four-hour runs, eight hours, extended accumulation, bursts, backlog, slow consumers and large concurrent transfers. Record latency distributions, picture lag/load time, bytes including retries, SQLite contention, memory, history/index growth, disk/reservation use, refusals and correctness errors. Compare retained accepted identities/outcomes with independent expectations. Lost accepted data fails regardless of speed.

Qualify combined pressure and supported ranges here, after each increment's ordinary failure evidence already passes. Do not meet budgets by dropping observations, deleting protected data, rewriting outcomes or automatically restarting Plugins. Verify actual tools/pins when implementation begins; historical qualification supplies no runnable toolchain.

Prove extension independence with a Plugin built in its own repository using the supported SDK/container contract. Install and invoke that external package, stop/remove it while Core stays usable and inspect retained Operation results. Archive its source separately and verify Core's normal build has no dependency on it. The in-repository example alone cannot pass this milestone.

Record coordinated release identity, version compatibility and partial-publication recovery without publishing absent authorization. Power-loss durability remains unverified until storage-stack validation or controlled power cuts verify acknowledged metadata/content together. Field readiness additionally requires real Asset execution with cancellation, disconnection and process restart. Physical radio claims require firmware/gateway and RF/airtime evidence. Simulators and process kills cannot pass those external gates.

## Evidence at every handoff

Record exact candidate revision, contract editions, real commands, independent expectations, observable outcomes, applicable races/faults and unrun requirements. Clean regeneration leaves generated output unpatched. Apply [coding standards](../CODING_STANDARDS.md) and [verification](development/verification.md) once a real toolchain exists.

Keep a requirement-to-scenario checklist and extend snapshot/replay, authorization, Restart and Reset coverage when new state becomes writable. Complete supported SDK operations have direct-Protocol parity and the required mode-specific evidence. No helper suite or generated snapshot substitutes for the owning end-to-end outcome.

This task changes documentation only. All implementation, capacity, durability, compatibility, external-extension and field-readiness evidence remains pending.
