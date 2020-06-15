---
title: Solution Interactions
keywords: engage about
tags: [overview]
sidebar: overview_sidebar
permalink: overview_interactions.html
summary: Getting involved with NHS Aggregator service
---

## Interactions

There are three main types of systems that need to integrate in order to share [Granular Pointers](overview_concepts.html#pointers) - 

| System | Role in RL | 
|-----------|----------------|
|Consumer|A system that wishes to retrieve Pointers related to a given patient by NHS number and optionally specify the type of Pointers to be retrieved|
|LRL|Local Record Locator which pushes up Patient Pointers to the NRL and also exposes the Record Location Query API to allow the Aggregator service to request more granular pointers in response to a Consumer client’s request|
|NRL|[The National Record Locator](https://developer.nhs.uk/apis/nrl/) that stores and provides access to Pointers for retrieval and maintenance|

In addition the consumer might not be interfacing directly with an LRL or the NRL. Instead it might be using a third type of [Record Locator](overview_concepts.html#actors) - 

| System | Role in RL | 
|-----------|----------------|
|Aggregator service|A service that interfaces with the NRL and LRLs to assemble all relevant Pointers based on the Consumer client’s inbound request|

### Consumer Interactions

#### Pointer Search

A Consumer can perform parameterised searches of the Pointers held within the RL using a [search interaction](api_interaction_search.html):

- A Consumer can ask the RL to return all Pointers that relate to a given patient by supplying the NHS number of that patient.
- A Consumer can combine the Patient search parameter with the [type](https://fhir.nhs.uk/STU3/ValueSet/NRL-RecordType-1) of data that the Pointer references to further narrow the search space

#### Pointer Aggregation

In this scenario instead of directly interacting with the NRL or an LRL the consumer client communicates with an Aggregator. However the Aggregator exposes an RL interface meaning that the client is unaware of the complexity that the aggregator is hiding.

The diagram below illustrates how the aggregator sits in between the consumer and the NRL and LRLs - 

<img alt="Consumer queries RL to get Pointer(s)" src="images/solution/Solution_Interactions_diagram.png" style="width:75%;max-width: 75%;">

1. The consumer client initiates a request with the Aggregator service to find pointers for a given patient (optionally refining the search by type)
2. The Aggregator first calls the NRL requesting Patient Pointers for the given patient
3. The Aggregator iterators through the Patient Pointers and calls down on to the RL endpoint that is referenced passing the original query parameters from the consumer
4. The Aggregator combines the results returned from each RL into one unified result set
5. The Aggregator also queries the NRL passing in the original query and adds any Granular Pointers into the unified result set
6. The Aggregator returns the unified result set to the consumer client