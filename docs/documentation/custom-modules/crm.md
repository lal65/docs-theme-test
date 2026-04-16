---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Customer Relationship Management Integration
image: inline-assets/documentation/custom-modules/crm.svg
menu_title: Customer Relationship Management Integration
menu_order: 8
---
[comment]: <> (Acronyms for the various actor groups)
*[CRM]: Customer Relationship Management
*[ECRM]: Enterprise Customer Relationship Management
*[IWA]: Infrastructure and Workload Automation

## Summary

The CRM Integration module provides a highly secure, defensive solution for
interfacing with the Penn State ECRM Group. It is a very strange solution for
a strange relationship with the central groups and their vendor.

Extraordinary precautions were taken to ensure that full control of the data
that appears on the website firmly rests with the World Campus business. There
is nothing that any upstream systems can do to break this application without a
World Campus business representative explicitly agreeing to it through an
automated enforced governance pipeline.

## Solution Architecture

> The Applications Developers did **_not_** have any meaningful influence over
> how the central group or vendor set anything up. This is **_not_** an
> architecture that would have been chosen from the Applications Developers side.


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

#### Architectural Security Risks
This architecture is problematic from a security perspective.

1. An extraordinarily weak authentication mechanism is all that Informatica
   presently supports
2. It is inherently more risky to accept POST requests than it is to simply
   make GET requests to retrieve data
3. The data being transmitted from Salesforce to this application is public 
   domain knowledge

The only question is why. It would be a much more secure architecture to simply
host the public domain data in a location that this application can reach out
and read it versus having a proprietary third party platform POST it.

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

![The Manage CRM Data user interface is displayed showing a status message of "There are currently no pending changes" and two links to various data sets]({{ "/assets/documentation/custom-modules/crm/manage-crm-data.png" | relative_url }})

##### Viewing Live Data Sets
Live data sets may be previewed through the Drupal admin UI.

###### Countries
The **_Countries_** data set can be found under **_Content_** →
**_Manage CRM Data_** → **_View Countries_**. The read-only table displays
all **_Country_** entities.
- The **_Code_** column is the unique identifier of the entity, which _should_
  correspond to a valid ISO-3166-1 alpha3 code
- The **_Label_** column is the end-user facing label
- The **_Two character code_** column _should_ correspond to a valid ISO-3166-1
  alpha2 code

![Countries are displayed in tabular format]({{ "/assets/documentation/custom-modules/crm/countries.png" | relative_url }})

###### Military branches
The **_Military branches_** data set can be found under **_Content_** →
**_Manage CRM Data_** → **_View Military Branches_**. The read-only table
displays all **_Military branch_** entities.
- The **_Code_** column is the unique identifier of the military branch
- The **_Label_** column is the end-user facing label
- The **_Sort order_** column determines the order that multiple military
  branches appear in select lists

![Military branches are displayed in tabular format]({{ "/assets/documentation/custom-modules/crm/military-branches.png" | relative_url }})

###### Military statuses
The **_Military statuses_** data set can be found under **_Content_** →
**_Manage CRM Data_** → **_View Military Statuses_**. The read-only table
displays all **_Military status_** entities.
- The **_Code_** column is the unique identifier of the military status
- The **_Label_** column is the end-user facing label
- The **_Sort order_** column determines the order that multiple military
  statuses appear in select lists
- The **_Display branch_** column controls the conditional field visibility of
  a military branch select element

For example, the **_None_** option for a military status will result in the
military branch select not appearing for the end-user _(if the user is not in
the military, they will obviously not belong to a branch, so why ask them?)_.

![Military statuses are displayed in tabular format]({{ "/assets/documentation/custom-modules/crm/military-statuses.png" | relative_url }})

###### Partners
The **_Partners_** data set can be found under **_Content_** →
**_Manage CRM Data_** → **_View Partners_**. The read-only table
displays all **_Partner_** entities.
- The **_Code_** column is the unique identifier of the partner
- The **_Label_** column is the end-user facing label

![Partners are displayed in tabular format]({{ "/assets/documentation/custom-modules/crm/partners.png" | relative_url }})

###### Programs
The **_Programs_** data set can be found under **_Content_** →
**_Manage CRM Data_** → **_View Programs_**. The read-only table
displays all **_Programs_** entities.
- The **_Code_** column is the unique identifier of the partner
- The **_Label_** column is the end-user facing label
- The **_Send to Inside Track_** column determines whether information requests
  to this program are eligible for follow-up contact by Inside Track
- The **_Spring Start date_** column is the optional ISO-8601 formatted start
  date of the spring term
- The **_Spring Deadline_** column is the optional ISO-8601 formatted
  application deadline date for the spring term
- The **_Spring Early deadline_** column is the optional ISO-8601 formatted
  early application deadline date for the spring term
- The **_Spring Deadline Extended_** column is a flag for whether the
  application deadline for the spring term has been extended
- The **_Summer Start date_** column is the optional ISO-8601 formatted start
  date of the summer term
- The **_Summer Deadline_** column is the optional ISO-8601 formatted
  application deadline date for the summer term
- The **_Summer Early deadline_** column is the optional ISO-8601 formatted
  early application deadline date for the summer term
- The **_Summer Deadline Extended_** column is a flag for whether the
  application deadline for the summer term has been extended
- The **_Fall Start date_** column is the optional ISO-8601 formatted start
  date of the fall term
- The **_Fall Deadline_** column is the optional ISO-8601 formatted
  application deadline date for the fall term
- The **_Fall Early deadline_** column is the optional ISO-8601 formatted
  early application deadline date for the fall term
- The **_Fall Deadline Extended_** column is a flag for whether the
  application deadline for the fall term has been extended
- The **_Visible Semester Deadlines_** column is a value from 0 to 3 that
  determines the number of upcoming deadlines to display
- The **_Hide Deadline_** column is a flag that determines whether the next
  application deadline should be displayed

- ![Programs are displayed in tabular format]({{ "/assets/documentation/custom-modules/crm/programs.png" | relative_url }})

###### States
The **_States_** data set can be found under **_Content_** →
**_Manage CRM Data_** → **_View States_**. The read-only table
displays all **_State_** entities.
- The **_Code_** column is the unique identifier of the state
- The **_Label_** column is the end-user facing label

![States are displayed in tabular format]({{ "/assets/documentation/custom-modules/crm/states.png" | relative_url }})

###### Topics
The **_Topics_** data set can be found under **_Content_** →
**_Manage CRM Data_** → **_View Topics_**. The read-only table
displays all **_Topic_** entities.
- The **_Code_** column is the unique identifier of the topic
- The **_Label_** column is the end-user facing label
- The **_Default topic_** column is a flag that determines if the topic is one
  of the default topics
- The **_Programs_** column is a list of programs that should appear under the
  topic

![Topics are displayed in tabular format]({{ "/assets/documentation/custom-modules/crm/topics.png" | relative_url }})

##### Synchronizing Data Sets
When the module detects that updated CRM data exists, content managers are
notified via the status message on the **_Manage CRM Data_** overview.

![When pending content exists, there is a message saying "There is new CRM data for you to review. View Changes"]({{ "/assets/documentation/custom-modules/crm/pending-changes-overview.png" | relative_url }})

Upon clicking **_View Changes_**, the content manager is presented with
a form that displays a red/green differential of the content that is currently
active versus the content that is pending to review. In the following example,
a new country, "New California Republic" is added and a topic is modified to
have one program removed and another program added.

![The New California Republic country is added and the finance campaign topic has program "Finance (Bachelor of Science)" removed and the "Applied Statistics (Master's Degree)" added"]({{ "/assets/documentation/custom-modules/crm/synchronize-form.png" | relative_url }})

Upon pressing the **_Synchronize_** button, the pending data will be made
active and all caches that hold the pages associated with the newly
synchronized data will be flushed.

###### Synchronization Errors
If the pending data is updated while a content manager is reviewing a
changeset, the content manager will be unable to complete synchronization due
to their review having been made stale.

Upon clicking the "Synchronize" button, they will be presented with an error
message stating:

> The reference data has been updated while you were reviewing the pending
> changes. Please refresh this page and re-review the changes to proceed.


![A stale review error prevents synchronization]({{ "/assets/documentation/custom-modules/crm/stale-review-error.png" | relative_url }})

#### BLS Data Entities
The entity types defined by this module are extremely light-weight.

1. These entity types are **_not_** revisionable.  The **_ECRM_** team is the
   canonical source this data and assumes all accountability for maintaining 
   historical revisions as needed for legal compliance.
2. These entity types **_cannot_** be viewed individually on their own pages.
3. These entity types are **_read only_**.  There are no edit forms for these
   entities.  The only way to modify them is via the synchronization
   functionality.

These constraints may seem unusual, but the enterprise architecture design was
decided without consulting the Applications Developers.

This module **_only_** concerns itself with the storage, access control, and
synchronization of said entities. It guarantees that there is always a
highly available persistent data source available for downstream consumers
regardless of organizational upheavals.

## Requirements
This module requires no modules outside of Drupal core.

## Installation
Install as you would normally install a contributed Drupal module. For further information, see [Installing Drupal Modules](https://www.drupal.org/extending-drupal/installing-drupal-modules).

## Configuration
This module does not expose any configuration.

## Maintainers
- Matthew David Webb <mdw15@psu.edu>, Applications Developer Manager
- Brianne Williams <bnh10@psu.edu>, Applications Developer
- Kyle Leber <kjl16@psu.edu>, Applications Developer
- Luke Leber <lal65@psu.edu>, Applications Developer
- Zachary Ishler <zri5004@psu.edu>, Applications Developer

## Support
Submit bug reports and feature suggestions, or track changes in the
[issue queue](https://github.com/psu-online-education/crm/issues).
