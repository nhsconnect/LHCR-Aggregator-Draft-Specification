---
title: Solution Data Model
keywords: engage about
# tags: [overview]
sidebar: overview_sidebar
permalink: overview_data_model.html
summary: Solution Data Model
---

## Data Model

In order to support the Consumer and Provider interactions with the NRL, a data model is provided for the Pointer. The data model is purposefully lean, each property has a clear reason to exist, and it directly supports the activities of Consumers and Providers.

You can explore an in-depth view of the lean data model and the full NRL DocumentReference profile in the [FHIR Resources and References section](explore_reference.html).

| Property | Cardinality | Description | 
|-----------|----------------|------------|
|[Identifier](pointer_identity.html)|0..1|Assigned by the NRL at creation time. Uniquely identifies this record within the NRL. Used by Providers to update or delete.|
|Profile|0..1|The URI of the FHIR profile that the resource conforms to. Indicates the version of the pointer model.|
|Pointer version |0..1|Assigned by the NRL at creation or update time. Used to track the current version of a Pointer.|
|Pointer last updated datetime|0..1|Assigned by the NRL at creation and update time. The date and time that the pointer was last updated.|
|Pointer indexed datetime|0..1|Assigned by the NRL at creation time. The date and time that the pointer was created.|
|[Master Identifier](pointer_identity.html)|0..1|An optional identifier of the document as assigned by the Provider. It is version specific — a new master identifier is required if the document is updated.|
|[Pointer Status](pointer_lifecycle.html)|1..1|The status of the pointer|
|Patient|1..1|The NHS number of the patient that the record referenced by this Pointer relates to. Supports Pointer retrieval scenarios.|
|Pointer owner|1..1|The entity that maintains the Pointer. Used to control which systems can modify the Pointer.|
|Record owner|1..1|The entity that maintains the Record. Used to provide the Consumer with context around who they will be interacting with if retrieving the Record.|
|Record category|1..1|A high-level category of the record. The category will be one of a controlled set. It will not be possible to create a pointer with a category that does not exist within this controlled set.|
|Record type|1..1|The clinical type of the record. Used to support searching to allow Consumers to make sense of large result sets of Pointers. The clinical type will be one of a controlled set. It will not be possible to create a pointer with a type that does not exist within this controlled set.|
|Record creation clinical setting|1..1|Describes the clinical setting in which the content was created.|
|Period of care|0..1|Details the period in which the documented care is relevant.|
|Pointer reference|1..*|Information about the record referenced|
|Record creation datetime|0..1|The date and time (on the Provider’s system) that the record was created, for static records.|
|Record URL|1..1|Absolute URL for the location of the record on the Provider’s system|
|Record format|1..1|Describes the technical structure and rules of the record such that the Consumer can pick an appropriate mechanism to handle the record.|
|Record MIME type|1..1|Describes the type of data such that the Consumer can pick an appropriate mechanism to handle the record.|
|Record stability|1..1|Describes whether the record content at the time of the request is dynamically generated or is static.|
|[Related Documents](pointer_maintenance.html)|0..1|Relationship to another pointer|
