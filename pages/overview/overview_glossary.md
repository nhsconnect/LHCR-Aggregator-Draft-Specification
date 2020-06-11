---
title: Glossary
keywords: abbreviations definitions glossaries terms
tags: [overview]
sidebar: overview_sidebar
permalink: overview_glossary.html
summary: "Glossary of terms used this implementation guide"
toc: false
---

Glossary of common terms and abbreviations used throughout this documentation site.

## .NET
.NET is both a business strategy from Microsoft and its collection of programming support for Web services. Microsoft’s goal is to provide individual and business users with a seamlessly interoperable and Web-enabled interface for applications and computing devices and to make computing activities increasingly Web browser-oriented. The .NET platform includes servers; building-block services, such as Web-based data storage; and device software. It also includes Passport, Microsoft’s fill-in-the-form-only-once identity verification service.

## API
The term Application Programming Interface is quite a confused term in the context of FHIR. Strictly speaking, an API is a set or services offered by a programming library that can be made use of by another application. Historically these would have been made available from a single machine although now they are available across networks.

In the service/message/document categorisation, an API is a service interface, though this categorisation is contentious. As technology became increasingly capable of supporting distributed operations, the differences between SOA (Service Oriented Architecture) and APIs became blurred. In the context of FHIR, we define APIs as RESTful interfaces.

## ASID
Accredited System ID (obtained by completing SCAL)

## DDC
Digital Delivery Centre

## dm+d
The NHS Dictionary of Medicines and Devices is a dictionary of descriptions and codes which represent medicines and devices in use across the NHS. It is delivered through a partnership between NHS Digital and the NHS Business Services Authority and provides the recognised NHS Standard for uniquely identifying medicines and medical devices used in patient care.

## DMS
Domain Message Specification Description of messages definitions and interactions relating to a specific business area (such as Ambulance, Child Health or Discharge etc.)

## DSTU
Draft Standard for Trial Use

## EUOD
End User Organisation Declaration

## FHIR
FHIR is the latest standard to be developed under the HL7 organisation. Pronounced ‘Fire’ and stands for Fast Healthcare Interoperability Resources.

## FHIRPath
FHIRPath is a path-based navigation and extraction language, somewhat like XPath. Operations are expressed in terms of the logical content of hierarchical data models, supporting traversal, selection, and filtering of data.

## FQDN
A Fully Qualified Domain Name consists of a host name (including all subnames) and a domain name, including the top-level domain (TLD).

## ITK
The Interoperability Toolkit, in the context of the NHS this is a set of common specifications, frameworks, and implementation guides to support interoperability within local organisations and across local health and social care communities.

## JSON
Javascript Object Notation is a lightweight data-interchange format. It is easy for humans to read and write and easy for machines to parse and generate. It is based on a subset of the JavaScript Programming Language.

## JWT
JSON web token, a standard for access tokens containing information about authorisation claims.

## LHCRE
Local Health and Care Record Exemplar

## MPV
Manufacturer Product Version

## N3
N3 is a Wide Area IP Network (WAN), connecting many different sites across the NHS within England & Scotland. It also connects to other networks via Gateways, notably to the Internet via the Internet Gateway.

## NEMS
National Events Management Service, a sister project to the NRL that tracks events.

## NRL
The National Record Locator is a national index to identify available records for patients and locate them across local and national care record solutions (such as SCR).

## ODS
The Organisational Data Service is responsible for publishing organisation and practitioner codes, along with related national policies and standards. They’re also responsible for the ongoing maintenance of the organisation and person nodes of the Spine Directory Service, the central data repository used within various NHS systems and services.

## PAS
The Patient Administration System records the patient’s demographics (e.g. name, home address, date of birth) and details all patient contact with the hospital, both outpatient and inpatient.

Some PAS systems do support additional clinical behaviours.

## PDS
The Personal Demographics Service is a national electronic database of NHS patients containing details such as name, address, date of birth and NHS Number (known as demographic information). (Implemented with HL7 v3)

## RBAC
Role Based Access Control defines roles and allowed business activities associated with a user (via their smartcard). (Implemented using SAML)

## Record Group
A grouping that contains one or more Record Types.

## Record Type
The clinical type of a record as defined by SNOMED-CT code. The clinical types permitted on the NRL are defined by a controlled list.

## REST
Representational State Transfer is a protocol based upon a network of Web resources (a virtual state-machine) where the user progresses through the application by selecting links, such as /user/tom, and operations such as GET or DELETE (state transitions), resulting in the next resource (representing the next state of the application) being transferred to the user for their use. Typically preferred due to it being “stateless” so no information about the client session needs to be maintained on the server.

## RESTful
Interfaces are often referred to as being “RESTful” when they follow principles of REST to avoid debate about whether they fully conform to REST.

## SCAL
Supplier Conformance Assessment List

## SCR
Summary Care Record

## SCRa
Summary Care Record Application

## SDS
The NHS Spine Directory Service is a component of the NHS Spine service comprising of the Spine User Directory and Spine Accredited Systems and Services. The SDS ensures that transactions/messages are only processed from authorised users and systems. (Implemented with LDAP)

## SNOMED
Systematized Nomenclature of Medicine

## SNOMED-CT
The most comprehensive and precise clinical health terminology product in the world, owned and distributed by SNOMED International.

## SOA
Method for systems integration and development that allows different applications to exchange data in a loosely coupled way.

## Spine
A collection of national applications, services, and directories which support the health and social care sector in the exchange of information in national and local IT systems

## SSP
Spine Secure Proxy

## STU
Standard for Trial Use The original DSTU2 FHIR standard has been superseded by the new standard STU3. To reflect that important parts of the spec are well past the draft stage

## TKW
Toolkit Workbench

## TLS
Transport Layer Security

## TLSMA
TLS Mutual Authentication

## UML
The Unified Modelling Language is a family of graphical notations baked by a single meta-model, that help in designing and describing software systems. Especially useful when working with OOP paradigms..

## Use Case
A list of actions or event steps, typically defining the interactions between a role (known in the Unified Modeling Language as an actor) and a system, to achieve a goal. The actor can be a human or other external system.

## Use Case Diagram
A visual presentation of a Use Case, standardised within UML.

## X509 certificate
A format of public key certificate originally defined in RFC 5280. Often colloquially referred to as an “SSL certificate”.

## XML
eXtensible Markup Language is a protocol designed to store and transport data. XML was designed to be both human and machine readable.

## xPath
XPath is a major element in the XSLT standard. XPath can be used to navigate through elements and attributes in an XML document. XPath is a syntax for defining parts of an XML document. XPath uses path expressions to navigate in XML documents. XPath contains a library of standard functions.