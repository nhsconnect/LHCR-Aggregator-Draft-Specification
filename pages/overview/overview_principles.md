---
title: Solution Principles
keywords: engage about
tags: [overview]
sidebar: overview_sidebar
permalink: overview_principles.html
summary: Solution Principles
---

## Principles

### The NRL does not guarantee that Records can be retrieved by following a Pointer

There are complexities associated with retrieving data over a network that are outside of the scope of the metadata captured by a Pointer. As an example, consider the need to define firewall rules to allow traffic to flow between a Consumer and a Provider. The RL aims to facilitate record retrieval, reducing the need for point-to-point integration between Consumer and Provider systems. However, Providers have a responsibility to ensure that Pointer metadata accurately reflects the retrieval mechanism and format of the referenced document/record.

### The Consumer controls Pointer access

When a Consumer requests that the RL return the Pointers that it has for a given patient (NHS number) it will return all Pointers. The RL will not perform any filtering before sending that collection of Pointers back to the Consumer. 

Once consequence of this is that the end user on the Consumer side may be exposed to Pointers that reveal sensitive information about the Patient, for example it will be possible to infer through a Pointer that a Patient has a certain kind of record. Even though the user may not be able to retrieve the Record, knowing that it exists is in itself revealing a degree of personal information about that patient that may not be appropriate.

With this in mind, there will most likely be a need to filter Pointers before they are displayed to the end user. This responsibility belongs to the Consumer and should be implemented using local access rules to judge whether a given user should be permitted to know that a given Pointer exists.

### Caching and Storing

It is important that consumers of RL Pointers can view and make clinical decisions based on the most up-to-date information available. For this reason, RL recommends that Pointers and referenced content returned from search and read requests not be cached or stored.
