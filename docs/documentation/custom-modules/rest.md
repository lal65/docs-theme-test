---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: REST
image: inline-assets/documentation/custom-modules/rest.svg
menu_title: REST
menu_order: 28
---
## Summary
The REST module is an old module that still retains two features related to
RESTful web services.

## Course Data Sync
The course data sync endpoint exposes a subset of the layout builder powered
data structure of the program content type. This information is fetched and
displayed on https://student.worldcampus.psu.edu by a specialized custom
block type that exists on that application.

The endpoint operates by rendering the program in a virtual DOM and using an
xpath selector to drill down to the **_courses_** section, selecting all blocks
that are not flagged as `sync-hidden`. The extracted HTML is concatenated and
returned from the API.

### Strange access control rules
The `wcprospect8` and `wcstudent` applications serve different demographics.
When a program becomes unpublished on `wcprospect8` (the Marketing application
used to drive new leads), there are usually students enrolled in the program.
This means that the course data must still exist and be accessible to the users
of the `wcstudent` application. To accomplish this, all programs served by this
endpoint are temporarily published (and not persisted). This could be viewed as
an access bypass, but it is intentional and controlled.

### Special CORS policy
The `wcstudent` application front-end accesses this API anonymously. This
requires a special CORS policy and special `Vary` header on `Origin`. There
exists a Response Subscriber that ensures that responses from this endpoint
vary on the `Origin` value.

## ECRM Webform Submission Export
This endpoint serves webform submissions based on a loose shape matching
algorithm. It is designed to serve as a single point of access for all CRM
integrations.

| Property       | Value                      |
|----------------|----------------------------|
| Endpoint       | /api/v2/crm_export/webform |
| Method         | GET                        |
| Authentication | basic                      |

### Query parameters
The following query parameters may be used to select results very granularly.

#### last_id
This integer is intended to be the last submission id that the CRM system has
successfully ingested.

#### sub_limit
This integer controls the maximum number of submissions returned by a single
call.

#### form_fields
This string is used to determine which forms should have their submissions
included in the response. This forms the basis of the shape matching. For
example, if the business defines an "information request" form as a form that
has a first name, last name, phone, email, and program of interest field, then
passing those fields into the `form_fields` parameter will result in **all**
form variants that have the requested fields being included in the response.

This mechanism allows for a **_very decoupled approach_** between the web
side and the CRM side. It allows for new form variants to be added and removed
without the need of lengthy and complicated interdepartmental hurdles.

#### A healthy dose of paranoia
For "information request" forms, there has always been an additional governance
safeguard in place that requires that a form field of `webform_type_rfi` only
apply if the form is enrolled into configuration management. This safeguard
actively prevents content managers from creating forms that can unexpectedly
add data to requests for this specific echelon of forms.

#### Unfortunate ETL responsibilities
When the University hired the Accenture vendor to build out the Informatica
integrations, there were severe communication problems. As a result the
wcprospect8 application was forced to become an ETL layer because the
proprietary ETL tooling apparently couldn't meet requirements.

##### String trimming
All user-submitted data is trimmed of white-space as an ETL step before being
included in the response.

##### WCGEN to NO_SELECT to WCGEN
This is one of the strangest requirements that grew over time. End-users may
sometimes select `WCGEN`, or "General Penn State World Campus Information" for
their program of interest.  **_Only for program of interest 1_**, there is a
requirement to transmute the `WCGEN` value to `NO_SELECT` before saving to the
Drupal database. Following the implementation of Salesforce, there was another
requirement added (unsure by whom), to transform the saved `NO_SELECT` value
back to `WCGEN`.

This chain of transformations highlights a fundamental misalignment between
the CRM and Analytics systems.  The analytics systems are looking for
`NO_SELECT` whereas the Salesforce system is looking for `WCGEN`. This could
have easily been avoided, but alas communications never happened and the
Applications Developers were never invited to have a seat at the solutioning
table.

## Technical debt
The functionality provided by this module should likely be ported to both the
Programs and Webform modules.

## Requirements
This module requires no modules outside drupal core.

## Installation
Install as you would normally install a contributed Drupal module. For further
information, see
[Installing Drupal Modules](https://www.drupal.org/extending-drupal/installing-drupal-modules).

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
[issue queue](https://github.com/psu-online-education/psu_rest/issues).
