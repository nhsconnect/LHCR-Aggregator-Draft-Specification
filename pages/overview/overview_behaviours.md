---
title: Solution Behaviour
keywords: engage about
tags: [overview]
sidebar: overview_sidebar
permalink: overview_behaviours.html
summary: Solution Behaviour
---

## Behaviour

### Pointer Retrieval by Consumers

Consumer RL interactions are read only. A system can search for a collection of pointers. 

The RL search interaction is predicated on the Consumer having a verified NHS number prior to retrieving Pointers.

Once the Consumer has a verified NHS number, the RL can be asked to retrieve a collection of all "current" Pointers that relate to that NHS number. The NRL looks for Pointers for which the Subject (Patient) property matches the NHS number query parameter. On this basis, the NRL will return a collection of zero or more matching Pointers. See the [search interaction page](api_interaction_search.html) for more details.

## Auditing
The RL will capture an audit trail for each request-response interaction that a client has with the system. Once captured, the audit trail can be retrieved.

### Capturing an Audit Trail
The audit trail begins with capturing key data from a client request, and it ends with capturing the RL's response to that request. The request audit trail and response audit trail will be combined to form a full end-to-end audit of a given request and response interaction with the RL.

An audit trail will capture different information about who or what is making the request, depending on whether the client is a person or a system. An example of the latter case is a Provider periodically running a batch job to synchronise its Pointers to the RL. When capturing the audit trail, the system needs to be aware of this difference.
