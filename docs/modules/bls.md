---
layout: default
subtitle_before: Penn State World Campus
title: Custom Modules
subtitle_after: Bureau of Labor Statistics Integration
menu_title: Bureau of Labor Statistics Integration
menu_order: 2
---
## Summary
The BLS Integration module provides a turnkey Drupal solution for interfacing
with a complex business process that involves multiple external groups. BLS is
an acronym for the U.S. Bureau of Labor Statistics, the principal federal
agency for labor economics, providing data on inflation, unemployment, and
employment projections.

There is a desire to display a carefully curated subset of this information
in a web-friendly format with strict editorial guidelines, safety mechanisms,
and governance policies.

## Solution Architecture
This solution was originally planned out with no support from Penn State IT.
Applications Developers were not involved in any meetings or discussions that
took place between Marketing and IT.

See the [Workfront Issue](https://experience.adobe.com/#/@pennstateoutreach/so:pennstateoutreach-Production/workfront/issue/617bf77c0024e80be85c3281191c6e11/overview) for more history.

### Organizations Involved
Several discrete groups make up the overall solution.

#### Business Intelligence & Market Research
The **_Business Intelligence & Market Research_** team at Penn State maintains
a manually curated spreadsheet of certain data points within their own
infrastructure. The platform and format of this team are irrelevant to this
module, but their part in this involves taking the canonical BLS data sets and
modifying it in ways dictated by the business.

##### Team Contacts
- Jennie Ishler <jci2@psu.edu>, Business Intelligence Manager
- Jason Lynn Park <jlp145@psu.edu>, Market Research Specialist
- Anya Olsavicky <ako105@psu.edu>, Market Research Specialist

#### Platform Strategy & Operations
The **_Platform Strategy & Operations_** team at Penn State takes on the
primary content management role for the World Campus marketing application.
This team decides if and when updated data from the **_Business Intelligence
& Market Research_** team goes live on the web properties.

##### Team Contacts
- Denny Connolly <djc5014@psu.edu>, Director of Platform Strategy and Operations
- Melanie Swiftney <mzs6920@psu.edu>, Platform Strategy and Operations Manager
- Emily C Ansley <eca5091@psu.edu>, Platform Strategy and Operations Manager

#### Applications Developers
The **_Applications Development_** team presently maintains an ETL data pipeline
that translates the raw output from the **_Business Intelligence & Market
Research_** team into a format that the **_Platform Strategy & Operations_**
team can review and moderate.

##### Team Contacts
- Matthew David Webb <mdw15@psu.edu>, Applications Developer Manager
- Brianne Williams <bnh10@psu.edu>, Applications Developer
- Kyle Leber <kjl16@psu.edu>, Applications Developer
- Luke Leber <lal65@psu.edu>, Applications Developer
- Zachary Ishler <zri5004@psu.edu>, Applications Developer

### ETL Process
The ETL pipeline can be found at
https://github.com/psu-online-education/bls-data-conversion. At a high level,
this process takes Microsoft Excel files and converts the data therein to JSON
data files.

The JSON files are served over REST to be consumed by the Drupal application.

#### JSON Schema
The generated JSON files are expected to match the following schema definition.
```json

{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://psu-online-education.github.io/bls-data-conversion/schema/wc-bls-data-prospect.schema.json",
  "title": "World Campus BLS Prospect Data",
  "description": "Curated BLS job outlook and job title data used by the World Campus prospect Drupal integration.",
  "type": "object",
  "required": ["job_outlooks", "job_titles"],
  "additionalProperties": false,
  "properties": {
    "job_outlooks": {
      "type": "array",
      "description": "Occupational outlook records derived from BLS data.",
      "items": {
        "$ref": "#/$defs/job_outlook"
      }
    },
    "job_titles": {
      "type": "array",
      "description": "Curated job titles associated with academic programs.",
      "items": {
        "$ref": "#/$defs/job_title"
      }
    }
  },
  "$defs": {
    "job_outlook": {
      "type": "object",
      "additionalProperties": false,
      "required": [
        "uid",
        "occ_title",
        "tot_emp",
        "employment_change",
        "sort_order",
        "programs"
      ],
      "properties": {
        "uid": {
          "type": "string",
          "description": "BLS SOC occupation identifier.",
          "pattern": "^[0-9]{2}-[0-9]{4}$"
        },
        "occ_title": {
          "type": "string",
          "description": "End-user-facing occupation title."
        },
        "tot_emp": {
          "type": "integer",
          "description": "Total employment estimate.",
          "minimum": 0
        },
        "employment_change": {
          "type": "number",
          "description": "Projected employment change expressed as a decimal (e.g., 0.058 for 5.8%).",
          "minimum": -1
        },
        "sort_order": {
          "type": "integer",
          "description": "Display weight used for sorting."
        },
        "programs": {
          "type": "array",
          "description": "World Campus academic program codes.",
          "items": {
            "type": "string",
            "minLength": 1
          }
        }
      }
    },
    "job_title": {
      "type": "object",
      "additionalProperties": false,
      "required": ["job_title", "programs"],
      "properties": {
        "job_title": {
          "type": "string",
          "description": "End-user-facing job title."
        },
        "programs": {
          "type": "array",
          "description": "World Campus academic program codes.",
          "items": {
            "type": "string",
            "minLength": 1
          }
        }
      }
    }
  }
}

```

### Drupal CMS Integration
This module provides a very user-friendly way for content managers to review,
moderate, and process changes.

#### Accessing BLS Integration Data
A link under the **_Content_** tab is provided to users that have a role
assigned that has the `access bls data` permission. The main content view
serves two purposes:

1. Providing links to view the currently live data sets.
2. Displaying a real-time data synchronization status update for users that
   have a role assigned that has the `synchronize bls data` permission.

![The Manage BLS Data user interface is displayed showing a status message of "There are currently no pending changes" and two links to various data sets]({{ "/assets/modules/bls/manage-bls-data.png" | relative_url }})

##### Viewing Live Data Sets
Live data sets may be previewed through the Drupal admin UI.

###### Job outlooks
The **_Job outlooks_** data set can be found under **_Content_** →
**_Manage BLS Data_** → **_View job outlooks_**. The read-only table displays
all **_Job outlook_** entities.
- The **_UID_** column is the unique identifier of the record
- The **_OCC title_** column is the end-user facing label
- The **_Total employment_** column is the number of positions available for
  the job outlook
- The **_Employment change_** column is the percent change (either positive or
  negative) from the last published data set
- The **_Sort order_** is the weight that is used to sort job outlook records
  when multiple are displayed together
- The **_Programs_** column displays all programs that the job outlook should
  be displayed on.

![Job outlooks are displayed in tabular format]({{ "/assets/modules/bls/job-outlooks.png" | relative_url }})

###### Job titles
The **_Job titles_** data set can be found under **_Content_** →
**_Manage BLS Data_** → **_View job titles_**. The read-only table displays
all **_Job title_** entities.
- The **_Job title_** column is the end-user facing label (and is also unique).
- The **_Programs_** column displays all programs that the job title should
  be displayed on.

![Job titles are displayed in tabular format]({{ "/assets/modules/bls/job-titles.png" | relative_url }})

##### Synchronizing Data Sets
When the module detects that updated BLS data exists, content managers are
notified via the status message on the **_Manage BLS Data_** overview.

![When pending content exists, there is a message saying "There are currently pending changes. To review, press the Synchronize BLS Data button"]({{ "/assets/modules/bls/pending-changes-overview.png" | relative_url }})

Upon pressing **_Synchronize BLS Data_**, the content manager is presented with
a form that displays a red/green differential of the content that is currently
active versus the content that is pending to review. In the following example,
a typo in the "Accountants and Auditors" job outlook is corrected.

![The job with UID 13-2011 has its OCC title changed from "Accountants and Autitors typo" to "Accountants and Auditors"]({{ "/assets/modules/bls/synchronize-form.png" | relative_url }})

Upon pressing the **_Synchronize_** button, the pending data will be made
active and all caches that hold the pages associated with the newly
synchronized data will be flushed.

###### Synchronization Errors
When the configured API endpoint fails to respond matching the expected schema
the end-user is presented with an error diagnostic in an "Error message".

On the **_Manage BLS Data_** page

![A client error occurred because of a 404 not found response on the Manage BLS Data page]({{ "/assets/modules/bls/synchronization-error.png" | relative_url }})

On the **_Manual approval is required._** page, note the **_Synchronize_**
button is removed.

![A client error occurred because of a 404 not found response on the Manual approval is required page]({{ "/assets/modules/bls/synchronization-error-manual-approval.png" | relative_url }})

A similar error message will be presented if there is any sort of
synchronization error that happens during the process itself. Data integrity
is guaranteed through the use of transactions in the database layer.

#### BLS Data Entities
The entity types defined by this module are extremely light-weight.

1. These entity types are **_NOT_** revisionable.  The **_Business Intelligence
   & Market Research_** team is the canonical source this data and assumes all
   accountability for maintaining historical revisions as needed.
2. These entity types **_CANNOT_** be viewed individually on their own pages.
3. These entity types are **_READ ONLY_**.  There are no edit forms for these
   entities.  The only way to modify them is via the synchronization
   functionality.

These constraints may seem unusual, but the **_Business Intelligence & Market
Research_** team was adamant that **_Microsoft Office 365_** was the only
platform that would be used to maintain this data.

This module **_only_** concerns itself with the storage, access control, and
synchronization of said entities. It guarantees that there is always a
highly available persistent data source available for downstream consumers
regardless of organizational upheavals.

## Requirements
This module requires no modules outside of Drupal core.

### Required Configuration
There is a soft-dependency on certain configurations. For complete support a
node bundle of **_program_** must exist with a **_field_program_code_** text
field.

## Installation
Install as you would normally install a contributed Drupal module. For further information, see [Installing Drupal Modules](https://www.drupal.org/extending-drupal/installing-drupal-modules).

## Configuration
This module requires one configuration: `bls.settings:bls_api_endpoint` which
is a URL to a compatible data source. There is no GUI for setting this. Use
**_drush_** and configuration management instead.

```bash
drush config:set bls.settings bls_api_endpoint https://example.com/data.json
drush config:export
```

## BLS API Endpoint Requirements
The configured endpoint **_MUST_**:
1. Be accessible from the public internet without any authentication
2. Serve a resource of type `application/json`
3. Meet the schema defined in the solution architecture documentation

Note - the data is designed to be served to the public. There is no value in
attempting to protect it via authentication mechanisms.

## Maintainers
- Matthew David Webb <mdw15@psu.edu>, Applications Developer Manager
- Brianne Williams <bnh10@psu.edu>, Applications Developer
- Kyle Leber <kjl16@psu.edu>, Applications Developer
- Luke Leber <lal65@psu.edu>, Applications Developer
- Zachary Ishler <zri5004@psu.edu>, Applications Developer

## Support
Submit bug reports and feature suggestions, or track changes in the
[issue queue](https://github.com/psu-online-education/bls/issues).
