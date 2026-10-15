---
status: accepted
---

# Attribute Asset Track observations to the Asset

Accepted on 15 October 2026. The user specified that a drone owns the Tracks it detects and that a gateway reports for it, including after an authorized gateway change.

## Context

[ADR-0022](0022-one-publisher-per-track.md) gives each Track one publisher. [ADR-0028](0028-trust-gateways-to-author-bound-asset-reports.md) trusts bound gateways to construct Asset reports. The primary field workflow also needs a drone to publish observed subjects through that gateway.

## Decision

An Asset may own observed Tracks. An authenticated gateway may create and update those observations for an explicitly bound Asset. The Asset remains the publisher source through gateway restart or authorized replacement. Core derives authority from the current binding; a supplied publisher ID alone grants no permission.

This extends trusted gateway reporting to Asset-owned Track observations. Core trusts the bound gateway's supplied observation evidence rather than independently verifying a physical detection. A faulty gateway can therefore submit false detections for its bound Assets, as it can submit false reports. The authority remains limited by binding, publisher ownership and the existing observation rules.

Current permission, identity, timing and prerequisite rules belong to [Asset-owned Track observations](../topics/identity-and-access.md#asset-owned-track-observations). Gateway implementation and radio transport remain outside Core.

## Consequences

Changing the radio route does not create another observation source or transfer a Track to the gateway. Unbound and cross-publisher writes remain rejected. Binding changes and source-scoped duplicate/order correlation need a concrete representation before the first Track relay proof. This decision does not select a radio protocol or gateway implementation.
