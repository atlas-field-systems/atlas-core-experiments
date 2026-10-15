# Implementation planning

There is no inherited implementation sequence or completed slice in this repository. PStack must produce its own [build plan](../build-plan.md) after the [specification audit](../../PSTACK_START.md#first-run-audit-the-specification-no-code).

The behavioral topics, API and component inventories, architecture decisions and [testing requirements](../testing-strategy.md) retain their requirements. Removing the previous task schedule does not remove any acceptance obligation. Historical slice names elsewhere are coverage groupings, not instructions to reproduce the previous implementation order.

<a id="delivered-foundation-and-proposed-operational-sequence"></a>

## Required-scenario homes

Map every applicable scenario in the testing requirements to a proposed increment in the new build plan. Nothing is marked delivered until this repository contains its implementation and qualifying evidence.

## S1 implementation boundary

The accepted bounded queued coordinate-target Move To workflow is described by the [spatial requirements](../topics/spatial-data.md), [Task contract](../topics/tasks.md) and [clock decision](../adr/0029-use-deployment-clocks-and-preserve-event-times.md). Preserve that scope when planning the corresponding increment. The name S1 does not require it to follow a copied S0 toolchain or ticket sequence.

## Foundation evidence and limits

There is no foundation, test fixture, generated contract or verifier in this repository. Prior qualification is historical, not a completion claim here. Establish a minimal real toolchain only after implementation is authorized.

## Public API and SDK inventory

The [endpoint map](../api-endpoints.md) and [SDK topic](../topics/sdk.md) own the required public surface. The new plan assigns implementation and evidence without treating a previous issue closure as proof.

<a id="reset-consolidation-follow-up"></a>

## Local actions and retained state

The [Dataset lifecycle](../topics/dataset-lifecycle.md) and [system design](system-design.md) own local management and retention requirements. Their placement in a new plan is an engineering decision, not inherited tracker state.
