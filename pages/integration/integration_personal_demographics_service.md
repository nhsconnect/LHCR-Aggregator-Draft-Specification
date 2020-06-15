---
title: Personal Demographics Service
keywords: spine pds integration patient, demographics
# tags: [integration]
sidebar: overview_sidebar
permalink: integration_personal_demographics_service.html
summary: "Overview of the role of the Personal Demographics Services (PDS) and the Spine Mini Services PDS (SMSP) within NRL."
---

## PDS Tracing

NRL systems MUST be capable of performing [Personal Demographics Service](https://digital.nhs.uk/services/demographics){:target="_blank"} (PDS) tracing of patients to obtain their NHS Number.

Systems MUST perform this tracing using one of the following three options:

### 1. Full PDS Spine Compliant System (PDS)

NRL systems MAY follow the guiding principles of [how systems should integrate with PDS](http://webarchive.nationalarchives.gov.uk/20160921135209/http://systems.digital.nhs.uk/demographics/spineconnect). This describes in particular the principles governing how systems should synchronise with PDS as the master repository of demographics data to ensure local system data does not become stale.

There is a very limited risk that the consumer-traced NHS number pre-dates a patient record status change to sensitive Flag (S-Flag) immediately prior to use.

### 2. Spine Mini Service PDS (SMSP)

NRL systems MAY utilise the [Spine Mini Service Provider for PDS](https://developer.nhs.uk/library/systems/nhs-digital-smsp-pds/){:target="_blank"} (SMSP) as a lighter weight alternative to integrating with the full Spine Personal Demographics Service (PDS).

{% include important.html content="As the SMSP service does not return multiple possible matches for the patient it is typically only suitable to be used where there is enough information to achieve a single matched trace." %}

{% include note.html content="SMSP responses do not include trace sequence number information. The presence of a trace sequence number in a consumer request to NRL could enable providers to programmatically determine the need to perform a PDS trace themselves, for example if their local patient record's trace was not current." %}

### 3. Demographics Batch Service (DBS)

NRL Provider systems MAY utilise the [Demographics Batch Service (DBS)](http://developer.nhs.uk/library/systems/demographic-batch-service-dbs/){:target="_blank"} to perform batch PDS Tracing.

The suitability of the use of DBS to perform batch PDS tracing would be assessed as part of the Consumer Accreditation process.

This option is for Provider systems only. Consumers performing a search operation MUST use either option 1 or 2 as described above.
