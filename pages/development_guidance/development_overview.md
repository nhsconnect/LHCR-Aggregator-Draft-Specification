---
title: Development Overview
keywords: getcarerecord structured rest resource
tags: [development,for_providers,for_consumers]
sidebar: foundations_sidebar
permalink: development_overview.html
summary: Overview of the development section.
---

## Interactions

The NRL offers [a variety of interactions](https://developer.nhs.uk/apis/nrl/architecture_overview.html#nrl-interactions) for both consumers and providers. For the [aggregation model](index.html#aggregation-overview) to work and NRL consumers to be able to access granular pointers held on an LRL, any LRL that publishes patient pointers to the NRL **MUST** expose a [Search Interaction API](api_interaction_search.html) (available to the NRL), which is similar to the NRL's [search](https://developer.nhs.uk/apis/nrl/api_interaction_search.html) implementation.

This includes the LRL conforming to the general API principles:
- **MUST** support HL7 FHIR STU3 version 3.0.1.
- **MUST** implement REST behaviour according to the [FHIR specification](http://www.hl7.org/fhir/STU3/http.html).
- **MUST** support both XML and JSON formats.

There are, however, some specific requirements for Record Locators that differ from the NRL's implementation. This documentation details those differences that **MUST** take precedence when designing, coding and testing the RL's search capability, specifically where the NRL acts as its consumer.

{% include note.html content="The NRL documentation acts as a general guide to other interaction APIs that may be useful for an LRL to possess (e.g. for providers to register pointers on the LRL), however, this is outside the scope of this documentation."%}

## Supported pointer types

The NRL has a [published set of pointer types it supports](https://developer.nhs.uk/apis/nrl/supported_pointer_types.html). This is not a fixed list, as new pointer types may be supported in the future.

A LHCR may decide their Record Locator will support a wider variety of pointer types for other purposes. Some pointer types may make sense nationally (in which case, we advise defining these in collaboration with other LHCRs and the NRL team), and some pointers may be internally specific to their LHCR. Furthermore, whilst not advised, if the LHCR has more consumers than just the NRL, they may decide to offer the same pointer types using an alternative FHIR profile specifically for the other consumers (as long as the NRL profile is always used for NRL interactions).

When an incoming RL consumer request, via the NRL, is for a specific pointer type, the RL is guaranteed it's safe to return all pointers of that type (related to the queried NHS Number) as the NRL will only ever request pointer types it supports. If no pointer type is present on the query, the RL has a few options available to it:

|Option|Comments|
|------|--------|
|Return all known pointers for the NHS Number.|The best option, as this maximises the number of pointers returned to the original consumer.|
|Return all known non-LHCR-specific pointers for the NHS Number.|Unlikely to be unsafe.|
|Return all known NRL-supported pointers for the NHS Number.|Potentially unsafe, as the LHCR may not be aware of recently NRL-supported pointer types.|

{% include note.html content="The NRL will ignore any pointers returned that don't match one of its supported formats, so it's critical that the RL keeps its own pointers aligned with the NRL format."%}

## Caching

It is important for all RLs **not** to cache responses to save searching for each request as clinical decisions must be based on the most up-to-date information available.
