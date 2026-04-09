---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Customer Relationship Management Integration
image: inline-assets/documentation/custom-modules/crm.svg
menu_title: Customer Relationship Management Integration
menu_order: 2
---
[comment]: <> (Acronyms for the various actor groups)
*[CRM]: Customer Relationship Management
*[ECRM]: Enterprise Customer Relationship Management
*[IWA]: Infrastructure and Workload Automation

## Summary

The CRM Integration module provides a highly secure, defensive solution for
interfacing with the Penn State ECRM Group. It is a very strange solution for
a very unfortunate relationship with the central groups and their vendor.

## Solution Architecture

> [!WARNING]
> The Applications Developers did **_not_** have any meaningful influence over
> how the central group or vendor set anything up. This is **_not_** an
> architecture that would have been chosen from the Applications Developers side.

&nbsp;

### Organizations Involved

==TODO: Fill in details about ECRM, IWA, Informatica, Salesforce...==

### Integration Actors
There are three primary actors in the end-to-end integration: Salesforce,
Informatica, and this application.

#### Salesforce
Salesforce, otherwise known as "Education Cloud" is a proprietary platform that
was licensed by the University to act as the enterprise-wide CRM system.

#### Informatica
Informatica is a cloud based data management platform. It acts as an
intermediary between Salesforce and this application.

#### The prospect site
Th

### REST Endpoint
The REST endpoint located at `/api/v2/crm_import` accepts POST with the
`application/json` content type. Authorized requests to this endpoint result
in the transactional replacement of a number of data sets that represent data
structures within the Salesforce instance. This is **_not_** a direct
integration with Salesforce! There is no off-the-shelf solution that works with
Informatica!
