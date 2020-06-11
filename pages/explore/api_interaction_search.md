---
title: API Search Interaction
keywords: structured rest documentreference
tags: [fhir,pointers,for_consumers]
sidebar: accessrecord_rest_sidebar
permalink: api_interaction_search.html
summary: To support parameterised search of the NHS Aggregator service
---

## Search

Consumer interaction to support parameterised search of the NRL. 

## Prerequisites

In addition to the requirements on this page the general guidance and requirements detailed on the [Development Guidance](explore.html#2-prerequisites-for-nrl-api) page MUST be followed when using this interaction.


## Search Request Headers
Consumer and Provider API search requests support the following HTTP request headers:

| Header        | Value                                                                                                                                                                                            | Consumer System | RLS            |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------- | -------------- |
| Accept        | The format of the response the client is able to understand. See the [Content Negotiation](development_general_api_guidance.html#content-negotiation) section. | MAY include     | MUST implement |
| Authorization | The base64url-encoded JSON web token required for audit on the spine - see [Access Tokens (JWT)](https://developer.nhs.uk/apis/nrl/integration_access_tokens_JWT.html) for details.              | MUST include    | MUST implement |

## Search DocumentReference
<div markdown="span" class="alert alert-success" role="alert">
`GET [baseUrl]/DocumentReference?[searchParameters]`
</div>

## Search Parameters
This implementation guide outlines the search parameters for the DocumentReference resource in the table below.

| Name    | Type | Description                                | Consumer System | RLS                                                                  | FHIR Path |
| ------- | ---- | ------------------------------------------ | --------------- | -------------------------------------------------------------------- | --------- |
| subject | URI  | Patient who is the subject of the document | MUST include    | MUST implementDocumentReference.subject(verified patient NHS Number) |

The format of the parameters is as follows:
- subject: https://demographics.spineservices.nhs.uk/STU3/Patient/[NHS Number]
- type.coding: `[system]|[value]`, where system is a URI such as http://snomed.info/sct and value is a code corresponding to a valid document type from that system
- _format: Any of the allowed values listed in the [Content Negotiation](development_general_api_guidance.html#content-negotiation) section
{% include note.html content="Note: Consumers MUST make sure that all search parameters are correctly URL encoded as per the [IETF URI spec](https://tools.ietf.org/html/rfc3986#section-3.4)."%}

### Example Queries
```
GET [baseUrl]/DocumentReference?subject=https%3A%2F%2Fdemographics.spineservices.nhs.uk%2FSTU3%2FPatient%2F9876543210
```

Returns all pointers for a patient with NHS Number 9876543210.

```
GET [baseUrl]/DocumentReference?subject=https%3A%2F%2Fdemographics.spineservices.nhs.uk%2FSTU3%2FPatient%2F9876543210&type.coding=http%3A%2F%2Fsnomed.info%2Fsct%7C736253002
```

Returns all pointers for a patient with NHS Number of 9876543210 and a pointer type of `http://snomed.info/sct|736253002` (Mental Health Crisis Plan).

## Search Response
### Success
- MUST return a `200` **OK** HTTP status code on successful execution of the interaction.
- MUST return a `Bundle` of type searchset, containing either:
  - A total value of 0, indicating no record was matched, i.e. an empty `Bundle`.
  {% include note.html content="Note: The RLS Service will ONLY return an empty bundle if the searched patient exists but has no corresponding records." %}
  - A non-zero total, with a corresponding number DocumentReference resources that conform to the `NRL-DocumentReference-1` FHIR profile and that have a status of “current”.
  {% include note.html content="Note: The version of the pointer model (FHIR profile) will be indicated in the DocumentReference.meta.profile metadata attribute for each pointer (see Overview for a description of profiles). A Bundle may contain pointers which conform to different versions of the pointer model." %}

### Partial Success (Distributed Systems)
Where an RLS must connect to remote systems in order to assemble its response there is the possibility that one of those remote system requests may fail.

In this scenario, it may be that the RLS can only return a partial searchset to the Consumer client.
Rather than aborting the entire request and returning an error to the client, the RLS SHOULD return the DocumentReferences that it has been able to identify. However, in order to inform the client that the searchsetBundle contains only a subset of the full searchset, the RLS SHOULD additionally include an OperationOutcome as an entry in the searchset Bundle.

There MUST only be one OperationOutcome included in the searchsetBundle under these circumstances, which can contain one or multiple issues (see below). Each issue represents a problem encountered by the RLS when interacting with a single remote system. For example, if there are problems interacting with two remote systems, there would be two issues within the OperationOutcome, each carrying details pertinent to the given system.

{% include note.html content="Note: Any OperationOutcome entry MUST NOT increment the totalvalue in the searchsetBundle. For example, a searchset Bundle containing entries [ OperationOutcome, DocumentReference, DocumentReference ] would have a totalvalue of 2."%}

This type of response MUST NOT be used to represent failures within the RLS itself.

In the event of a partial error the RLS MUST NOT return a searchset that does not contain an OperationOutcome as described above. If the RLS does not wish to return partial results then it MAY return an OperationOutcome along with a `500` HTTP error to the client.

Partial success is indicated with a `200` **OK** HTTP status code, plus a success response, plus the following addition:
- The `Bundle` additionally contains an `OperationOutcome` indicating details of failures that occurred when calling out to external systems.
- Issues in this `OperationOutcome` have one of the following structures, depending if the error returned from the external system is a 4xx client error or a 5xx server error:

| issue-severity | issue-type                           | Details.Code                         | Details.Display                      | Diagnostics                          |
| -------------- | ------------------------------------ | ------------------------------------ | ------------------------------------ | ------------------------------------ |
| warning        | `[as returned from external system]` | `[as returned from external system]` | `[as returned from external system]` | `[as returned from external system]` |
| issue-severity | issue-type | Details.Code                                                          | Details.Display                                                               | Diagnostics                                                                                                                                                                  |
| -------------- | ---------- | --------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| warning        | exception  | Unable to complete search request - %s (Where %s is the query string) | The request exists but is not in an appropriate state for the call to succeed | _Optional - information that the consumer client can relay back to the aggregator support team in order to help them trace the issue that led to the given OperationOutcome_ |

### Failure 

The following errors can be triggered when performing this operation:

- [Invalid NHS number](development_general_api_guidance.html#invalid-nhs-number)
- [Invalid parameter](development_general_api_guidance.html#parameters)
- [No record found](development_general_api_guidance.html#nhs-number-not-found)
- [Invalid header](development_general_api_guidance.html#headers)


## Example Scenario
An authorised RLS Consumer searches for a patient’s relevant health record using the RLS to discover potentially vital information to support a patient’s emergency crisis care.

### Request Query
Return all DocumentReference resources (pointers) for a patient with a NHS Number of 9876543210. The format of the response body will be XML.

### cURL
```sh
curl -X GET
    -H 'Accept: application/fhir+xml'
    -H 'Authorization: BEARER [token]'
    '[baseUrl]/DocumentReference?subject=https://demographics.spineservices.nhs.uk/STU3/Patient/9876543210&_format=xml'
```

### Query Response Http Headers
```
HTTP/1.1 200 OK
Server: nginx/1.10.0 (Ubuntu)
Date: Mon, 19 May 2020 08:36:12 GMT
Content-Type: application/fhir+xml;charset=utf-8
Last-Modified: Mon, 19 May 2020 08:36:12 GMT
```

### Query Responses
#### No Pointer Matched
- HTTP 200 - Request was successfully executed
- Empty bundle resource of type searchset containing a total value of 0 indicating no record was matched

<div class="github-sample-wrapper scroll-height-350">
{% highlight XML %}
<Bundle xmlns="http://hl7.org/fhir">
  <id value="de258b7f-2f0b-4b41-afcb-964d9995fd4f-65447656980012115439" />
  <type value="searchset" />
  <total value="0" />
  <link>
    <relation value="self" />
    <url value="https://psis-sync.national.ncrs.nhs.uk/DocumentReference?subject=https%3A%2F%2Fdemographics.spineservices.nhs.uk%2FSTU3%2FPatient%2F9876543213" />
  </link>
</Bundle>
{% endhighlight %}
</div>

#### Single Pointer Matched
- HTTP 200 - Request was successfully executed
- Bundle resource of type searchset containing a total value of 1 with a DocumentReference resource that conforms to the `NRL-DocumentReference-1` profile.

<div class="github-sample-wrapper scroll-height-350">
{% highlight XML %}
<?xml version="1.0" encoding="UTF-8"?>
<Bundle xmlns="http://hl7.org/fhir">
   <id value="dd9724d1-7b61-44e2-9023-b72e6b966018-76563212455590986546" />
   <type value="searchset" />
   <total value="1" />
   <link>
      <relation value="self" />
      <url value="https://psis-sync.national.ncrs.nhs.uk/DocumentReference?subject=https%3A%2F%2Fdemographics.spineservices.nhs.uk%2FSTU3%2FPatient%2F9876543210" />
   </link>
   <entry>
      <fullUrl value="https://psis-sync.national.ncrs.nhs.uk/DocumentReference/0353e505-f7be-4c20-8f4e-337e79a32c51-76009894321256642261" />
      <resource>
         <DocumentReference>
            <id value="0353e505-f7be-4c20-8f4e-337e79a32c51-76009894321256642261" />
            <meta>
               <profile value="https://fhir.nhs.uk/STU3/StructureDefinition/NRL-DocumentReference-1" />
               <version value="1" />
               <lastUpdated value="2016-03-08T15:26:00+01:00" />
            </meta>
            <masterIdentifier>
               <system value="urn:ietf:rfc:3986" />
               <value value="urn:oid:1.3.6.1.4.1.21367.2005.3.7" />
            </masterIdentifier>
            <status value="current" />
            <type>
               <coding>
                  <system value="http://snomed.info/sct" />
                  <code value="736253002" />
                  <display value="Mental health crisis plan" />
               </coding>
            </type>
            <class>
               <coding>
                  <system value="http://snomed.info/sct" />
                  <code value="734163000" />
                  <display value="Care plan" />
               </coding>
            </class>
            <subject>
               <reference value="https://demographics.spineservices.nhs.uk/STU3/Patient/9876543210" />
            </subject>
            <indexed value="2016-03-08T15:26:01+01:00" />
            <author>
               <reference value="https://directory.spineservices.nhs.uk/STU3/Organization/RGD" />
            </author>
            <custodian>
               <reference value="https://directory.spineservices.nhs.uk/STU3/Organization/RR8" />
            </custodian>
            <relatesTo>
               <code value="replaces" />
               <target>
                  <identifier>
                     <system value="urn:ietf:rfc:3986" />
                     <value value="urn:oid:1.3.6.1.4.1.21367.2005.3.6" />
                  </identifier>
               </target>
            </relatesTo>
            <content>
               <attachment>
                  <contentType value="application/pdf" />
                  <url value="https://p1.nhs.uk/MentalhealthCrisisPlanReport.pdf" />
                  <creation value="2016-03-08T15:26:00+01:00" />
               </attachment>
               <format>
                  <system value="https://fhir.nhs.uk/STU3/CodeSystem/NRL-FormatCode-1" />
                  <code value="urn:nhs-ic:unstructured" />
                  <display value="Unstructured Document" />
               </format>
               <extension url="https://fhir.nhs.uk/STU3/StructureDefinition/Extension-NRL-ContentStability-1">
                  <valueCodeableConcept>
                     <coding>
                        <system value="https://fhir.nhs.uk/STU3/CodeSystem/NRL-ContentStability-1" />
                        <code value="static" />
                        <display value="Static" />
                     </coding>
                  </valueCodeableConcept>
               </extension>
            </content>
            <context>
               <period>
                  <start value="2016-03-07T13:34:00+01:00" />
               </period>
               <practiceSetting>
                  <coding>
                     <system value="http://snomed.info/sct" />
                     <code value="708168004" />
                     <display value="Mental health service" />
                  </coding>
               </practiceSetting>
            </context>
         </DocumentReference>
      </resource>
      <search>
         <mode value="match" />
      </search>
   </entry>
</Bundle>
{% endhighlight %}
</div>

#### Multiple Pointers Matched
- HTTP 200 - Request was successfully executed
- Bundle resource of type searchset containing a total value of 2 containing DocumentReference resources that conform to the `NRL-DocumentReference-1` profile

<div class="github-sample-wrapper scroll-height-350">
{% highlight XML %}
<?xml version="1.0" encoding="UTF-8"?>
<Bundle xmlns="http://hl7.org/fhir">
   <id value="dd9724d1-7b61-44e2-9023-b72e6b966018-54446768443265458908" />
   <type value="searchset" />
   <total value="2" />
   <link>
      <relation value="self" />
      <url value="https://psis-sync.national.ncrs.nhs.uk/DocumentReference?subject=https%3A%2F%2Fdemographics.spineservices.nhs.uk%2FSTU3%2FPatient%2F9876543211" />
   </link>
   <entry>
      <fullUrl value="https://psis-sync.national.ncrs.nhs.uk/DocumentReference/0353e505-f7be-4c20-8f4e-337e79a32c51-58778905432232112312" />
      <resource>
         <DocumentReference>
            <id value="0353e505-f7be-4c20-8f4e-337e79a32c51-58778905432232112312" />
            <meta>
               <profile value="https://fhir.nhs.uk/STU3/StructureDefinition/NRL-DocumentReference-1" />
               <version value="1" />
               <lastUpdated value="2016-03-08T15:26:00+01:00" />
            </meta>
            <masterIdentifier>
               <system value="urn:ietf:rfc:3986" />
               <value value="urn:oid:1.3.6.1.4.1.21367.2005.3.11" />
            </masterIdentifier>
            <status value="current" />
            <type>
               <coding>
                  <system value="http://snomed.info/sct" />
                  <code value="736253002" />
                  <display value="Mental health crisis plan" />
               </coding>
            </type>
            <class>
               <coding>
                  <system value="http://snomed.info/sct" />
                  <code value="734163000" />
                  <display value="Care plan" />
               </coding>
            </class>
            <subject>
               <reference value="https://demographics.spineservices.nhs.uk/STU3/Patient/9876543211" />
            </subject>
            <indexed value="2018-07-01T15:26:01+01:00" />
            <author>
               <reference value="https://directory.spineservices.nhs.uk/STU3/Organization/RGD" />
            </author>
            <custodian>
               <reference value="https://directory.spineservices.nhs.uk/STU3/Organization/RR8" />
            </custodian>
            <content>
               <attachment>
                  <contentType value="application/pdf" />
                  <url value="https://p1.nhs.uk/MentalhealthCrisisTeam" />
                  <creation value="2018-07-01T15:26:00+01:00" />
               </attachment>
               <format>
                  <system value="https://fhir.nhs.uk/STU3/CodeSystem/NRL-FormatCode-1" />
                  <code value="urn:nhs-ic:record-contact" />
                  <display value="Contact details (HTTP Unsecured)" />
               </format>
               <extension url="https://fhir.nhs.uk/STU3/StructureDefinition/Extension-NRL-ContentStability-1">
                  <valueCodeableConcept>
                     <coding>
                        <system value="https://fhir.nhs.uk/STU3/CodeSystem/NRL-ContentStability-1" />
                        <code value="static" />
                        <display value="Static" />
                     </coding>
                  </valueCodeableConcept>
               </extension>
            </content>
            <context>
               <period>
                  <start value="2017-05-12T13:34:00+01:00" />
               </period>
               <practiceSetting>
                  <coding>
                     <system value="http://snomed.info/sct" />
                     <code value="708168004" />
                     <display value="Mental health service" />
                  </coding>
               </practiceSetting>
            </context>
         </DocumentReference>
      </resource>
      <search>
         <mode value="match" />
      </search>
   </entry>
   <entry>
      <fullUrl value="https://psis-sync.national.ncrs.nhs.uk/DocumentReference/5303c09e-de28-4622-9416-62ab0b22e15a-54598780989231213433" />
      <resource>
         <DocumentReference>
            <id value="5303c09e-de28-4622-9416-62ab0b22e15a-54598780989231213433" />
            <meta>
               <profile value="https://fhir.nhs.uk/STU3/StructureDefinition/NRL-DocumentReference-1" />
               <version value="1" />
               <lastUpdated value="2016-03-08T15:26:00+01:00" />
            </meta>
            <masterIdentifier>
               <system value="urn:ietf:rfc:3986" />
               <value value="urn:oid:1.3.6.1.4.1.21367.2005.3.10" />
            </masterIdentifier>
            <status value="current" />
            <type>
               <coding>
                  <system value="http://snomed.info/sct" />
                  <code value="736253002" />
                  <display value="Mental health crisis plan" />
               </coding>
            </type>
            <class>
               <coding>
                  <system value="http://snomed.info/sct" />
                  <code value="734163000" />
                  <display value="Care plan" />
               </coding>
            </class>
            <subject>
               <reference value="https://demographics.spineservices.nhs.uk/STU3/Patient/9876543211" />
            </subject>
            <indexed value="2018-07-02T11:25:01+01:00" />
            <author>
               <reference value="https://directory.spineservices.nhs.uk/STU3/Organization/RGD" />
            </author>
            <custodian>
               <reference value="https://directory.spineservices.nhs.uk/STU3/Organization/RR8" />
            </custodian>
            <relatesTo>
               <code value="replaces" />
               <target>
                  <identifier>
                     <system value="urn:ietf:rfc:3986" />
                     <value value="urn:oid:1.3.6.1.4.1.21367.2005.3.9" />
                  </identifier>
               </target>
            </relatesTo>
            <content>
               <attachment>
                  <contentType value="application/pdf" />
                  <url value="https://p1.nhs.uk/MentalhealthCrisisPlanReport.pdf" />
                  <creation value="2016-03-08T15:26:00+01:00" />
               </attachment>
               <format>
                  <system value="https://fhir.nhs.uk/STU3/CodeSystem/NRL-FormatCode-1" />
                  <code value="urn:nhs-ic:unstructured" />
                  <display value="Unstructured Document" />
               </format>
               <extension url="https://fhir.nhs.uk/STU3/StructureDefinition/Extension-NRL-ContentStability-1">
                  <valueCodeableConcept>
                     <coding>
                        <system value="https://fhir.nhs.uk/STU3/CodeSystem/NRL-ContentStability-1" />
                        <code value="static" />
                        <display value="Static" />
                     </coding>
                  </valueCodeableConcept>
               </extension>
            </content>
            <context>
               <period>
                  <start value="2016-03-07T13:34:00+01:00" />
                  <end value="2017-02-13T14:11:00+01:00" />
               </period>
               <practiceSetting>
                  <coding>
                     <system value="http://snomed.info/sct" />
                     <code value="708168004" />
                     <display value="Mental health service" />
                  </coding>
               </practiceSetting>
            </context>
         </DocumentReference>
      </resource>
      <search>
         <mode value="match" />
      </search>
   </entry>
</Bundle>
{% endhighlight %}
</div>

#### Error Response (OperationOutcome)
- HTTP 400 - Bad Request: Invalid Parameter
- OperationOutcome resource that conforms to the [Spine-OperationOutcome-1](https://fhir.nhs.uk/STU3/StructureDefinition/Spine-OperationOutcome-1) profile

<div class="github-sample-wrapper scroll-height-350">
{% highlight XML %}
<?xml version="1.0" encoding="UTF-8"?>
<OperationOutcome xmlns="http://hl7.org/fhir">
   <id value="5a4171ac-486e-11e8-9ffc-6c3be5a609f5-34326129638901216693" />
   <meta>
      <profile value="https://fhir.nhs.uk/STU3/StructureDefinition/Spine-OperationOutcome-1" />
   </meta>
   <issue>
      <severity value="error" />
      <code value="invalid" />
      <details>
         <coding>
            <system value="https://fhir.nhs.uk/STU3/CodeSystem/Spine-ErrorOrWarningCode-1" />
            <code value="INVALID_PARAMETER" />
            <display value="Invalid parameter" />
         </coding>
      </details>
      <diagnostics value="The given resource URL does not conform to the expected format - https://demographics.spineservices.nhs.uk/STU3/Patient/[NHS Number]" />
   </issue>
</OperationOutcome>
{% endhighlight %}
</div>

#### Partial Success Response (searchset Bundle with OperationOutcome)
- HTTP 200 - Request was successfully executed
- Bundle resource of type searchset with a total value of 1 containing a DocumentReference resource that conforms to the `NRL-DocumentReference-1` profile
- Additional entry containing a `Spine-OperationOutcome-1` with details of the issue that occurred

<div class="github-sample-wrapper scroll-height-350">
{% highlight XML %}
<?xml version="1.0" encoding="UTF-8"?>
<Bundle xmlns="http://hl7.org/fhir">
   <id value="714aa0a4-8fd4-4fc1-9df9-ad9f1312c982" />
   <meta>
      <lastUpdated value="2020-05-22T09:03:40.172+00:00" />
   </meta>
   <type value="searchset" />
   <total value="1" />
   <link>
      <relation value="self" />
      <url value="https://us-central1-adh-nrls.cloudfunctions.net/DocumentReference?subject=https%3A%2F%2Fdemographics.spineservices.nhs.uk%2FSTU3%2FPatient%2F9690869035" />
   </link>
   <entry>
      <fullUrl value="http://35.246.116.71:5001/nrls-ri/DocumentReference/5ea1aae82f5bfb00013ddae7" />
      <resource>
         <DocumentReference>
            <id value="5ea1aae82f5bfb00013ddae7" />
            <meta>
               <versionId value="1" />
               <lastUpdated value="2020-04-23T14:49:12.192+00:00" />
               <profile value="https://fhir.nhs.uk/STU3/StructureDefinition/NRL-DocumentReference-1" />
            </meta>
            <status value="current" />
            <type>
               <coding>
                  <system value="http://snomed.info/sct" />
                  <code value="736253002" />
                  <display value="Mental health crisis plan" />
               </coding>
            </type>
            <subject>
               <reference value="https://demographics.spineservices.nhs.uk/STU3/Patient/9690869035" />
            </subject>
            <author>
               <reference value="https://directory.spineservices.nhs.uk/STU3/Organization/MHT01" />
            </author>
            <custodian>
               <reference value="https://directory.spineservices.nhs.uk/STU3/Organization/MHT01" />
            </custodian>
            <content>
               <extension url="https://fhir.nhs.uk/STU3/StructureDefinition/Extension-NRL-ContentStability-1">
                  <valueCodeableConcept>
                     <coding>
                        <system value="https://fhir.nhs.uk/STU3/CodeSystem/NRL-ContentStability-1" />
                        <code value="static" />
                        <display value="Static" />
                     </coding>
                  </valueCodeableConcept>
               </extension>
               <attachment>
                  <contentType value="application/pdf" />
                  <url value="https://provider-C.example.com/fhir/STU3/careconnect/binary/mhcp/94f0c43f-0a67-4de2-9291-1a3191c5c241" />
                  <creation value="2020-04-23T14:49:02.930Z" />
               </attachment>
               <format>
                  <system value="https://fhir.nhs.uk/STU3/CodeSystem/NRL-FormatCode-1" />
                  <code value="urn:nhs-ic:unstructured" />
                  <display value="Unstructured Document" />
               </format>
            </content>
            <context>
               <period>
                  <start value="2020-04-23T14:49:02.930Z" />
               </period>
               <practiceSetting>
                  <coding>
                     <system value="http://snomed.info/sct" />
                     <code value="708168004" />
                     <display value="Mental health service" />
                  </coding>
               </practiceSetting>
            </context>
         </DocumentReference>
      </resource>
      <search>
         <mode value="match" />
      </search>
   </entry>
   <entry>
      <resource>
         <OperationOutcome>
            <meta>
               <profile value="https://fhir.nhs.uk/STU3/StructureDefinition/Spine-OperationOutcome-1" />
            </meta>
            <issue>
               <severity value="warning" />
               <code value="exception" />
               <details>
                  <coding>
                     <system value="https://fhir.nhs.uk/STU3/CodeSystem/Spine-ErrorOrWarningCode-1" />
                     <code value="INVALID_REQUEST_STATE" />
                     <display value="The request exists but is not in an appropriate state for the call to succeed" />
                  </coding>
               </details>
               <diagnostics value="Unable to complete search request https://delay-proxy.glitch.me/status/400/DocumentReference?subject=https%3A%2F%2Fdemographics.spineservices.nhs.uk%2FSTU3%2FPatient%2F9690869035. Please contact the support team in order to help you trace the issue that led to this OperationOutcome." />
            </issue>
         </OperationOutcome>
      </resource>
      <search>
         <mode value="outcome" />
      </search>
   </entry>
</Bundle>
{% endhighlight %}
</div>