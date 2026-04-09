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

Extraordinary precautions were taken to ensure that full control of the data
that appears on the website firmly rests with the World Campus business. There
is nothing that any upstream systems can do to break this application without a
World Campus business representative explicitly agreeing to it through an
automated enforced governance pipeline.

## Solution Architecture

> The Applications Developers did **_not_** have any meaningful influence over
> how the central group or vendor set anything up. This is **_not_** an
> architecture that would have been chosen from the Applications Developers side.

&nbsp;

### Organizations Involved
TODO: Fill in details about ECRM, IWA, Informatica, Salesforce...who is in
charge, how to contact, etc...

### Integration Actors
There are three primary actors in the end-to-end integration: Salesforce,
Informatica, and this application.

#### Salesforce
Salesforce, otherwise known as "Education Cloud" is a proprietary platform that
was licensed by the University to act as the enterprise-wide CRM system.
Salesforce exposes data that can be read by Informatica.

#### Informatica
Informatica is a cloud based data management platform. It acts as an
intermediary between Salesforce and this application. Informatica reads data
from Salesforce and performs an authenticated HTTP POST to the prospect site.

#### The prospect site
This application accepts authenticated HTTP POST requests from Informatica. It
stores the pending information in a temporary holding area until a business
representative can manually review and approve the data.

### REST Endpoint
The REST endpoint located at `/api/v2/crm_import` accepts POST with the
`application/json` content type. Authorized requests to this endpoint result
in the transactional replacement of a number of data sets that represent data
structures within the Salesforce instance. This is **_not_** a direct
integration with Salesforce! There is no off-the-shelf solution that works with
Informatica!

The request format is the result of a trial end error process by the
Informatica operators, who were primarily looking for **_any format that would
work_**. A highly custom, and somewhat strange looking API was created from the
request format that resulted from that experimentation. In other words, this
solution was custom-built around the capabilities of the third party system.

As a result the Applications Developers at World Campus do **_not_** have an
OpenAPI compatible specification for this endpoint. The only documentation of
that kind is riddled with proprietary Informatica extensions and **_not_**
portable.

### Pending Data Storage
Successful POST requests data is written to a special location in the database
via the
[State API](https://www.drupal.org/docs/develop/drupal-apis/state-api/state-api-overview).

> The State API is a simple system for the storage of information about the
> system's state. The information is stored in the database and will be lost
> when the database is dropped or the site is re-installed from configuration.

&nbsp;

The following State keys are written:
- crm_b2b_partners
- crm_countries
- crm_military_branches
- crm_military_statuses
- crm_programs
- crm_programs_parents_children
- crm_states
- crm_topics

Such writes are transactional. All data stored for any of the keys are wiped
out and replaced with the new incoming data.

### Drupal CMS Integration
This module provides a very user-friendly way for content managers to review,
moderate, and process changes to Salesforce data.

#### Accessing Salesforce Data
A link under the **_Content_** tab is provided to users that have a role
assigned that has the `access crm data` permission. The main content view
serves two purposes:

1. Providing links to view the currently live data sets.
2. Displaying a real-time data synchronization status update for users that
   have a role assigned that has the `synchronize crm data` permission.
