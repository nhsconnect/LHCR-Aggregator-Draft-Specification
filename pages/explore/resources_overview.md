---
title: Development Overview
keywords: getcarerecord structured rest resource
tags: [development,for_providers,for_consumers]
sidebar: foundations_sidebar
permalink: explore.html
summary: "Overview of the Development section"
---

## 1. NHS Aggregator API Overview

The NHS Aggregator API supports the following operations as detailed in the [Solution Interactions](overview_interactions.html) section of this implementation guide:

|Interaction|HTTP Verb|Actor|Description|
| ------------- | ------------- | ------------- | ------------- | ------------- | 
|[Search](api_interaction_search.html)|GET|Consumer|Parameterised search for pointers on the NRL|

## 2. Prerequisites

### 2.1 RLS Server API Conformance:
- MUST support HL7 FHIR STU3 version 3.0.1.
- MUST implement REST behaviour according to the [FHIR specification](http://www.hl7.org/fhir/STU3/http.html).
- MUST support both XML and JSON formats.

### 2.2 Client System API Conformance:
- MUST support HL7 FHIR STU3 version 3.0.1.
- MUST support at least one of XML or JSON formats.

### 2.3 NHS Number

NHS Numbers used with FHIR API profiles MUST be verified. NHS Numbers can be verified using a full PDS Spine-compliant system (HL7v3), a Spine Mini Services Provider (HL7v3), or a Demographics Batch Service (DBS) batch-traced record (CSV).

The option of using a DBS service is for Provider systems only. Consumers performing a search operation MUST use either a full PDS Spine compliant system or a Spine Mini Services Provider.

{% include note.html content="Note: A verified NHS Number exists on PDS, is still in use, and the demographic data supplied results in the correct degree of demographic matching as per PDS matching rules."%}

The NHS NUMBER is 10 numeric digits in length. The tenth digit is a check digit used to confirm its validity. The check digit MUST be validated using the Modulus 11 algorithm.
