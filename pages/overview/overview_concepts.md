---
title: Solution Concepts
keywords: engage about
tags: [overview]
sidebar: overview_sidebar
permalink: overview_concepts.html
summary: Solution Concepts
---

## Concepts

### Pointers

- Patient Pointer - a coarse grained Pointer stored on NRL whose presence indicates that a remote system contains data regarding the Patient referenced in the Pointer. This class of Pointer merely says that an RL holds Granular  Pointers for the patient; it does not indicate the type of Granular Pointers that are held. Addtionally the Pointer references an endpoint that implements the RL specification (ie this specification)
- Granular Pointer - a Pointer indicating that a remote system contains a specific type of Document or Record for the Patient referenced in the Pointer

### Actors
In essence there are only ever two types of actor involved in Pointer retrieval - 

- RL - Record locator (i.e. systems implementing this specification) which exposes Granular Pointers for clients to retrieve 
- Consumer client - a system initiating the request with a RL service to find pointers for a given patient. Typically the Consumer will be interested in Pointers of a given type. The type will be one of the specific types of Pointer recognised at the national level (e.g. Mental Health Care Plan)

Concretely there are two kinds of RL that a consumer client may interact with - 

- NRL – National Record Locator which exposes Patient Pointers and Granular Pointers that are recognised at the national level. 
- LRL – Local Record Locator exposes Granular Pointers to consumer clients

There is a third kind of RL that consumer clients might interact with - 

- Aggregator service –  collects together Pointers coming from a number of different RL services. It aggregates multiple result sets into a single set which is returned to the consumer client. In this way the complexity of handling Pointers spread across several RLs is hidden from the consumer client