---
title: Solution Interactions
keywords: engage about
tags: [overview]
sidebar: overview_sidebar
permalink: overview_interactions.html
summary: Getting involved with NHS Aggregator service
---

## Interactions

There are three main types of systems that need to integrate in order to share data with the NRL.

| System | Role in NRL | 
|-----------|----------------|
|Consumer|A system that wishes to retrieve Pointers related to a given patient by NHS number and optionally specify the type of Pointers to be retrieves.|
|LRL|Local Record Locator which pushes up Patient Pointers to the NRL and also exposes the Record Location Query API to allow the Aggregator service to request more granular pointers in response to a Consumer client’s request.|
|NRL|A system that stores and provides access to Pointers for retrieval and maintenance.|
|Aggregator service|A service that interfaces with the NRL and LRLs to assemble all relevant Pointers based on the Consumer client’s inbound request|

### Provider Interactions

#### Pointer Search

A Consumer can perform parameterised searches of the Pointers held within the NRL using a [search interaction](api_interaction_search.html):

- A Consumer can ask the NRL to return all Pointers that relate to a given patient by supplying the NHS number of that patient.
- A Consumer can combine the Patient search parameter with the ODS code of the organisation that owns a Pointer to further narrow the search space.

### Data-flow

The consumer client initiates a request with the Aggregator service to find pointers for a given patient. The pointers may be of a given type. The Aggregator assembles all relevant pointers based on the Consumer client’s inbound request from the NRL and LRLs and returns the aggregated results with any relevant errors, if any. 