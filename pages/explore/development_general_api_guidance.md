---
title: General API guidance
keywords: fhir development
tags: [fhir,development,for_providers,for_consumers]
sidebar: overview_sidebar
permalink: development_general_api_guidance.html
summary: "Implementation guidance for developers - focusing on general API implementation guidance"
---


## Overview
The **search** interaction supports parameterised search of an RLS from Consumer systems.Relevant FHIR profiles for the mentioned resource types are as follows:

| Profile     | Description |
| ----------- | ----------- |
| [Bundle](https://hl7.org/fhir/STU3/bundle.html)  | A container for a collection of resources |
| [NRL-DocumentReference-1](https://fhir.nhs.uk/STU3/StructureDefinition/NRL-DocumentReference-1)  |    Points to a record that can be made available to healthcare systems     |
| [Spine-OperationOutcome-1](https://fhir.nhs.uk/STU3/StructureDefinition/Spine-OperationOutcome-1)| Supports a collection of error, warning, or information messages that result from an RLS interaction |

### Notational Conventions
The keywords ‘**MUST**’, ‘**MUST NOT**’, ‘**REQUIRED**’, ‘**SHALL**’, ‘**SHALL NOT**’, ‘**SHOULD**’, ‘**SHOULD NOT**’, ‘**RECOMMENDED**’, ‘**MAY**’, and ‘**OPTIONAL**’ in this implementation guide are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

## RESTful API

## Content Negotiation
The supported content types are as follows:
- JSON
  - `application/fhir+json` (canonical version - Consumers SHOULD use this)
  - `application/json+fhir`
  - `application/json`
  - `text/json`
- XML
  - `application/fhir+xml` (canonical version - Consumers SHOULD use this)
  - `application/xml+fhir`
  - `application/xml`

The content negotiation algorithm is as follows:
1. If `_format` query parameter is present:
   - If valid, use its value.
   - If invalid, return an `INVALID_PARAMETER` operation outcome in XML format.
2. Else, if `Accept` header is present:
   - If valid, use its value.
   - If invalid, return a `MISSING_OR_INVALID_HEADER` operation outcome in XML format.
3. Else, default to XML.

## Error Handling

### Failure
In a failure scenario triggered by a Client error, the RLS MUST return a 4xx response containing an allowed [Spine-OperationOutcome-1](https://fhir.nhs.uk/STU3/StructureDefinition/Spine-OperationOutcome-1) type.
Relevant OperationOutcome types are as follows:

#### Invalid NHS Number
Used to inform a client that NHS Number used in a Consumer search interaction is invalid.

| HTTP Code | issue-severity | issue-type | Details.Code       | Details.Display    | Diagnostics                                                               |
| --------- | -------------- | ---------- | ------------------ | ------------------ | ------------------------------------------------------------------------- |
| 400       | error          | invalid    | INVALID_NHS_NUMBER | Invalid NHS number | The NHS number does not conform to the NHS Number format: `[nhs number]`. |

#### NHS Number Not Found
Used to inform a client that no corresponding patient was found for the supplied NHS number.

| HTTP Code | issue-severity | issue-type | Details.Code    | Details.Display | Diagnostics                                            |
| --------- | -------------- | ---------- | --------------- | --------------- | ------------------------------------------------------ |
| 404       | error          | not-found  | NO_RECORD_FOUND | No record found | The given NHS number could not be found `[nhs number]` |

#### Headers
Used to inform a client that a header is missing or invalid.
- The `Accept` header is invalid

| HTTP Code | issue-severity | issue-type | Details.Code              | Details.Display                               | Diagnostics                   |
| --------- | -------------- | ---------- | ------------------------- | --------------------------------------------- | ----------------------------- |
| 400       | error          | invalid    | MISSING_OR_INVALID_HEADER | There is a required header missing or invalid | Accept HTTP Header is invalid |

- The mandatory `Authorization` HTTP Header is missing in the request

| HTTP Code | issue-severity | issue-type | Details.Code              | Details.Display                               | Diagnostics                          |
| --------- | -------------- | ---------- | ------------------------- | --------------------------------------------- | ------------------------------------ |
| 400       | error          | invalid    | MISSING_OR_INVALID_HEADER | There is a required header missing or invalid | Authorization HTTP Header is missing |

#### Parameters
Raised in relation to request parameters that the client may have specified. This error can be raised in a variety of circumstances.

| HTTP Code | issue-severity | issue-type | Details.Code      | Details.Display   | Diagnostics |
| --------- | -------------- | ---------- | ----------------- | ----------------- | ----------- |
| 400       | error          | invalid    | INVALID_PARAMETER | Invalid parameter | Varies      |
