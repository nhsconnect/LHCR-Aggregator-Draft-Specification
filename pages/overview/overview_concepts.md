---
title: Solution Concepts
keywords: engage about
tags: [overview]
sidebar: overview_sidebar
permalink: overview_concepts.html
summary: Solution Concepts
---

## Concepts

<img alt="Consumer queries RL to get Pointer(s)" src="images/solution/Solution_Concepts_diagram.png" style="width:75%;max-width: 75%;">

### Actors
- Consumer client – system initiating the request with the Aggregator service to find pointers for a given patient. Typically the Consumer will be interested in Pointers of a given type. The type will be one of the specific types of Pointer recognised at the national level (e.g. Mental Health Care Plan)
- Aggregator service – interfaces with the NRL and LRLs to assemble all relevant Pointers based on the Consumer client’s inbound request
- LRL – Local Record Locator which pushes up Patient Pointers to the NRL and also exposes the Record Location Query API to allow the Aggregator service to request more granular pointers in response to a Consumer client’s request
- NRL – National Record Locator which exposes Patient pointers and more granular Pointers that are recognised at the national level. The Aggregator service retrieves Patient Pointers from the NRL.

### Pointers

- Patient Pointer - a coarse grained Pointer stored on NRL whose presence indicates that a remote system contains data regarding the Patient referenced in the Pointer
- Granular Pointer - a Pointer indicating that a remote system contains a specific type of Document or Record for the Patient referenced in the Pointer