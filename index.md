---
title: Introduction to RL Service
keywords: homepage
tags: [overview]
sidebar: overview_sidebar
permalink: index.html
toc: false
summary: A brief introduction to getting started with the RL API.
---

# Introduction
Welcome to the Record Locator Technical Specification landing page. This site will continue to be updated as we receive feedback, which we welcome from all within the clinical and non-clinical community. To discuss any questions or queries feel free to email us at <nrls@nhs.net>.

# Background
Prerequisites - Knowledge of the National Record Locator (NRL) and Local Healthcare Records (LHCR) is assumed.

LHCRs may have their own Local Record Locators (LRL) that perform a similar role to the NRL however they facilitate the sharing of a richer set of locally relevant data which may also include data that is of national interest.

LHCRs are free to push national-level Pointers up to NRL to ensure that this data is shared at that national level. However they are also free to create so-called Patient Pointers instead. A Patient Pointer indicates that the LRL contains a set of more granular Pointers but it doesn’t say which types of Pointers. 

Consider a Consumer client who wishes to find Pointers of a given type (xyz) for a given Patient (123). They issue a query to NRL that is able to return all Pointers of type xyz for that Patient. However some information is potentially missing as some LHCRs may not have pushed up xyz Pointers but have instead pushed up Patient pointers.

In order to determine whether or not a LHCR contains a Pointer of type xyz for the patient they would need to query the LHCR via its LRL interface.

The presence of the Patient pointer gives the consumer an efficiency in that it should only query a LHCR if there is a Patient pointer for the given patient.

Assuming there is a Patient pointer the consumer will ask the LRL for any Pointers of type xyz for the patient. In this way the consumer performs a scatter gather operation in order to retrieve the full set of Pointers of type xyz for the patient.

This process of retrieving Patient pointers and calling associated LHCRs to gain the full set of Pointers is known as aggregation. In order to simplify the process of aggregation each LRL should expose the same query interface. This specification defines that interface. 

{% include note.html content="LRL's and the NRL can both be viewed as RL's and should therefore implement the same query specification. Currently the NRL implements a slight variation of the specification detailed in this guide however it is still compatible with the functionality detailed within these pages such that it could act as a RL in an aggregation workflow."%}
