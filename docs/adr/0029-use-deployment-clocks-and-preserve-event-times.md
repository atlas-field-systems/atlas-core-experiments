---
status: accepted
---

# Use deployment clocks and preserve event times

Accepted on 9 October 2026 during the S1 readiness discussion. This supersedes [ADR-0025](0025-use-core-time-as-the-installation-reference-clock.md). The user selected available, correct Asset clocks as an operating assumption and rejected SDK estimation of Core time. Timestamped Commands describe intent; the bounded queued coordinate-target Move To workflow starts when received and the Asset OS's queue permits it, without a start deadline.

## Decision

Deployment provides available, correct Asset clocks and Core's host clock. Engineering uses Protocol date-time values as a shared UTC timestamp representation; this is a representation choice for the agreed clock assumption, not a new operator behavior or an Atlas time service. Original accepted timestamp spellings remain part of signed facts. Atlas does not estimate participant offsets from Core or implement clock synchronization, drift correction or a clock-failure recovery workflow. Clock provisioning is a deployment responsibility and does not require internet access during a Mission under [ADR-0010](0010-operate-without-internet-access.md).

Preserve the time of each event at its source. Core separately stamps receipt, resource creation and update, and accepted changes with its own clock. A later transmission, retry, status update or receipt must not change an earlier measurement or execution time. Resource `created_at` and `updated_at` do not establish the age of every quantity in the resource. Missing original observation or retained execution time remains unknown, even though the reporting process has a correct clock now.

[Asset reporting](../topics/asset-reporting.md) owns the freshness proof. A current-process challenge and timely current report remain required; historical evidence, duplicate reports and a gateway's own connection cannot establish new Asset Contact. Correct clocks do not replace authenticated identity, ordering or execution evidence.

Commands with an accepted age or expiry rule use the supplied event times and correct clocks. The bounded queued coordinate-target Move To workflow has no execution deadline. Later Command variants retain their declared validity behavior; this decision removes offset estimation, not their age, control-ordering or expiry contracts.

Store the times needed to describe distinct events without requiring every stored timestamp in every transfer. Preserve the [SDK](../topics/sdk.md) contract without adding a generic timestamp-table codec or making physical-radio capacity claims. Future constrained gateways require measured packet and airtime budgets.

## Alternatives considered

- Retain SDK Core-time offset estimation. This adds a clock-management mechanism the selected deployment assumption does not require.
- Keep only aggregate resource creation and update times. This loses measurement age when an unrelated field changes and cannot describe delayed execution evidence accurately.
- Repeat all stored metadata in every update. This spends bandwidth on facts the receiver already has.

## Consequences

- SDK consumers use source timestamps directly; the general SDK and gateways do not maintain a Core-time offset estimate.
- The bounded initial direct-IP workflow does not require or compute the former `clock_uncertainty_ms` offset field. Operational schemas may retain it as optional nullable compatibility metadata; it has no age, Contact or uncertainty-budget behavior. Measurement uncertainty in metres remains a separate spatial fact.
- No executable Protocol schema or generated binding is imported into this experiment. Implementations must author and qualify operational definitions under this decision; prior structural qualification is not available implementation evidence here.
- Tests retain fresh, stale and unknown original evidence, replay, loss, ordering and Restart/Reset coverage. Clock drift and host clock discontinuity recovery are outside the selected operating assumptions.
- Older offset-estimation requirements in imported topic or testing prose are superseded. [Documentation context](../documentation-context.md#current-clock-decision) makes that precedence explicit while the initial audit identifies remaining editorial reconciliation.
