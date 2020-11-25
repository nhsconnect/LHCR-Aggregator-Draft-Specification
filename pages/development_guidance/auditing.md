---
title: Solution Behaviour
keywords: engage about
tags: [overview]
sidebar: overview_sidebar
permalink: auditing.html
summary: Auditing requirements.
---

## Auditing

In addition to the [regular auditing of calls from the RL to the NRL](https://developer.nhs.uk/apis/nrl/guidance_auditing.html), the RL owner must capture an equivalent audit trail for all other interactions e.g. an incoming consumer request from the NRL.

### Capturing an Audit Trail

The audit trail begins with capturing key data from a client request, and ends with capturing the RL's response to that request. The request and response audit trails will be combined to form a full end-to-end audit of a given request/response interaction with the RL.

An audit trail will capture different information about who or what is making the request, depending on whether the client is a person or a system. An example of the latter case is a provider periodically running a batch job to synchronise its pointers to the RL. When capturing the audit trail, the system needs to be aware of this difference.

### Audit Trail Requests

NHS Digital may request the RL's audit trail data containing NRL interaction records.
