---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Webform
image: inline-assets/documentation/custom-modules/webform.svg
menu_title: Webform
menu_order: 34
---

## Summary
The Webform module builds upon the contributed module with the same name. There
are a lot of customizations, third-party integrations, extension points, A/B
testing support, and carefully curated feature sets. This is one of the key
modules that powers the fine-tuned, enterprise grade form building platform.

Many of the features are more fully documented in submodule pages.

## Advanced governance and security
Several custom permissions and governance controls were added to harden
processes and enforce stringent access controls.

**Note:** these governance rules may no longer make sense given the new
reporting structures.

### Information request form governance
It has been the long-standing agreement that the Applications Developers are
the only parties to make changes to the various information request forms. Such
changes are made through the Configuration Management system, which allows the
forms to be committed to the version control system and deployed atomically. To
enforce this, a custom submodule was created: Webform Read Only, which actively
prevents any edits to certain forms. Not even administrators may modify the
information request forms on production through the Webform user interface.

### Information request form submission protection
The original internal access control requirements within this application were
not as intentional as they likely should have been. There are a couple
operations that were deemed too dangerous to allow to be governed by existing
access control mechanisms.

1. Deleting all submissions for a given webform: there is an additional access
   check that prevents **_anyone_** from mass deleting form submissions for
   forms that are marked as "read only". For forms that are not marked as "read
   only", only users with the **administrator** role may delete all submissions.
2. Deleting individual submissions for a given webform: there is an additional
   access check that ensures only **administrator** or **webform_editor** users
   may delete individual submissions.

## Cutting edge spam and risk mitigation
There are a number of advanced spam mitigation features added by this module.
The tactics include rate limiting, advanced data validation against third-party
services, intelligent CAPTCHA challenges, and multi-tiered governance gates.

### Rate limiting
Rate limiting is provided via the Webform Flood submodule. This feature allows
form builders to set submission rate limits on a per-form basis.

### Advanced data enrichment and diagnostics
Advanced data diagnostics are provided via the Webform Neutrino submodule. This
submodule introduces a new custom element type: Neutrino. This element type
records a plethora of additional information about phone numbers, IP addresses,
domains, and other data points which can be enhanced through third-party data
enrichment services.

For a specific example, it becomes possible to route submissions in different
ways through downstream business processes based on whether the user's IP
address was using a VPN service. There are an unlimited number of possible
business rules that can be facilitated through this system.

### Advanced address validation
Address validation is performed against both Salesforce backed data stores in
addition to advanced ZIP validation performed against a third-party API
service. The custom address element type will be covered in more detail in its
own dedicated section.

The allowed country and state data points are enforced against what is referred
to as the "CRM Reference Data", which is managed by a third-party group within
Penn State. This data is ingested by the Drupal application through a rigid
accountability process that includes manual data review steps. In short, any
sort of service disruption on the third-party side will **_never_** impact the
ultimate end-users (prospective students). A representative from the World
Campus business is always required to verify and accept all pending changes to
the reference data before it is used on any forms.

ZIP codes are also validated for users that self-elect that they are residing
in the United States. This validation is performed in real-time on the
application front-end as well as behind the scenes in a server to server
validation routine upon form submit. The real-time validation is an important
user experience feature that gives prospective students instant feedback about
the status of their form submission without having to even submit the form. The
back-end validation ensures that no invalid ZIP codes can be recorded due to
client-side browser tampering.

#### Resilience in the face of third-party outages
There is an important requirement to call out regarding the ZIP validation. If
the third-party vendor is down or unavailable for any reason, ZIP validation is
automatically disabled in real-time with no IT staff intervention. Every time
that this fail-safe is triggered, Splunk will send an email notification to
the Applications Developers group at `mkt-developers@outreach.psu.edu`. Ever
since migrating away from the USPS web service to Smarty, this has occurred
routinely.

### Intelligent CAPTCHA challenges
The Marketing group was very hesitant to add any sort of friction to forms on
the wcprospect8 application. This hesitation was mixed with a healthy dose of
restraint when it comes to blocking submissions. For these reasons, a novel
(at the time) solution was implemented: utilizing Google reCAPTCHA version 3
with a fallback to Google reCAPTCHA 2 for low scoring users. This system allows
the vast majority of users to submit forms without any sort of challenge at all
while forcing the few low scoring users (or automation) to complete an "I'm not
a robot" challenge, which **_may_** require completing an interactive
challenge.

The custom CAPTCHA solution was implemented via a custom Webform element plugin
type that can be added to forms at the discretion of the business. This form
element type, as well as possibilities for its usage, is more thoroughly
documented on the Webform Recaptcha submodule page.

## Custom form element types
There are a total of 8 custom form element types that were added to meet the
original needs of the legacy Drupal 7 application. The data collected by some
of these elements may no longer be useful to the business, but the
conversations into deprecating and removing them has consistently stalled out.

### Address composite element
This composite element exists to collect address information. Its architecture
reflects the requirements of the legacy Campus Nexus CRM system. The original
requirements also included advanced conditional logic based on whether the
end-user preferred to receive a physical mailed brochure, a process which the
business has subsequently discontinued.

The Campus Nexus CRM system had unique storage requirements for various address
parts. The following fields exist for each address element:

| Field     | Description                                                                                                     |
|-----------|-----------------------------------------------------------------------------------------------------------------|
| country   | The country code (must exist within the CRM Reference Data)                                                     |
| postal    | The postal code for non-US countries (only visible if the selected country is NOT the United States)            |
| address_1 | The first line of the address                                                                                   |
| address_2 | The second line of the address                                                                                  |
| zip       | The ZIP code (only visible if the selected country is the United States                                         |
| city      | The city                                                                                                        |
| state     | The state (must exist within the CRM reference data, only visible if the selected country is the United States) |

Each field can be conditionally visible, and conditionally required on a
form-by-form basis.

#### Special Campus Nexus requirements
As alluded to in the preceding table, there are expected to be one or more
blank values in each address element submission data set because there is
conditional visibility and required logic depending on the selected country.
The element will effectively clear out any data that the user may have entered
in the scenario that they use client-side tampering to include data for both US
and non-US addresses. In other words, if the country is US, then the `postal`
field is cleared. If the country is not the US, then the `state`, `zip`, and
`city` fields are cleared.

#### Geolocation data enrichment
If the end-user selected the country of US and the address element was
configured to not display the city and/or state fields, then the back-end
server will automatically geolocate the city and state based on either the
provided ZIP code or the user's IP address if no ZIP code is available.

### Campaign identifier element
This is a simple hidden element type that captures the value of the first-party
`CID` cookie and sets it on the hidden element. This element is how campaign
attribution is recorded for form submissions. If no such `CID` cookie exists on
the user's browser, then an empty value is recorded.

**_Important:_** the Campus Nexus CRM system could not accommodate CID values
greater than 255 characters in length, therefore the element itself acts as an
ETL step and will truncate all characters beyond 255.

### Device category element
This is a simple value element type that performs an API lookup to determine
if the user-agent matches a known mobile device or not. This data point is
likely unused by the business, but efforts to remove it have all but stalled.

If the third-party service reports that the submitted user-agent is for a
mobile device, the element value is 'mobile', otherwise it is 'desktop'.

### Google client identifier element
This is a simple hidden element type that captures the value of the Google
client identifier and sets it on the hidden element. This element helps to
match leads across multiple systems. This element requires that Google Tag
Manager not be blocked by client-side instrumentation. If GTM is unavailable,
then it is expected that this element's value be empty.

**_Important:_** the Campus Nexus CRM system could not accommodate Google
client identifier values greater than 255 characters in length, therefore
the element itself acts as an ETL step and will truncate all characters
beyond 255.

### Military select composite element
This composite element exists to collect military affiliation data. It consists
of two fields: `military_status`, and `military_branch`. Each data point must
match one that exists within the CRM Reference Data.

The CRM Reference Data for military statuses also includes a flag that
determines whether the military branch field should be shown. For military
statuses that are configured (in Salesforce) to not display the military
branch selection, this element ensures that any value that could have been
entered into the military branch field via browser tooling be cleared out.

### Partner identifier element
This is a simple hidden element type that captures the value of the first-party
`partner_id` cookie and sets it on the hidden element. This element is how
partner-based attribution is recorded for form submissions. If no such
`partner_id` cookie exists on the user's browser, then an empty value is
recorded.

**_Important:_** the Campus Nexus CRM system could not accommodate partner id
values greater than 255 characters in length, therefore the element itself acts
as an ETL step and will truncate all characters beyond 255.

### User agent element
This is a simple value element type that reads the value of the `User-Agent`
request header.

**_Important:_** the Campus Nexus CRM system could not accommodate user agent
values greater than 255 characters in length, therefore the element itself acts
as an ETL step and will truncate all characters beyond 255.

### Program select composite element
This composite element is among the most complicated. The purpose of the
element is to provide users with an intuitive way of selecting one or more
programs of interest. As the number of programs offered by World Campus has
grown over the years, so has the complexity involved for the end-user to find
their program out of a list of over 200. This element also allows for selection
filtering based on program topic. The relationship between topics and programs
is maintained within the Salesforce CRM system and is part of the CRM Reference
Data.

This composite has one configurable option on a form-by-form basis: the maximum
number of programs that the user can select. Typically, this is set to a value
from 1 to 3.

#### CRM limitations and the curious UX problem
Due to limitations in the downstream CRM marketing, only the first program of
interest is included in subsequent marketing correspondence. Despite this, the
business wants to collect more than one program of interest on all information
request forms.

There forms the user experience problem: the first program of interest is the
most important one for the user to select, but the user is not aware of the
downstream limitation! This problem has forced the evolution of the program
select composite element and has precluded the use of traditional multi-select
widgets, as such widgets do not intuitively communicate the implicit
significance of the **_first_** program of interest.

#### Advanced multi-filtering
Through custom code, multi-filtering mode can be activated so that there may
be multiple topic filters applied to the program list. This is only a feature
available on campaign pages presently. The use case here was that the program
lists on certain pre-filtered campaigns grew too large, and thus it was
requested that an additional visible topic filter be added for end-users.

## Unique A/B testing support
While the contributed Webform module forms the basis for variant based testing,
there is one custom variant type that was added to facilitate a unique type of
test. The "Webform wizard page order" variant type allows the pages multistep
forms to be re-ordered for certain segments of traffic.

For example, the A/B test idea that resulted in the creation of the new variant
plugin type wanted to test whether users were more likely to complete a form
submission if they were presented with program of interest selection first
versus entering their contact information first.

## Hooks
There are a multitude of hook implementations that are needed to support the
design and functional requirements of this application.

### Required indicator placement
Due to design requirements for multistep forms, the required indicator element
is not located in a compatible location in the DOM. Therefore, the required
indicator position must be changed. In a `webform_submission_form_alter` hook
implementation, multistep forms have the required indicator element unset, as
the design has it placed within the progress area instead. Otherwise, if the
form is not multistep, then the required element is wrapped in a div element
with the class `.form-item` to ensure there is adequate vertical spacing
beneath it.

### Forcing the use of clientside validation
The webform offers multiple methods of validating user input, but the World
Campus design only provides a specification for a single method. Therefore,
in a `form_webform_settings_form_form_alter` hook implementation, the
`form_novalidate` and `form_disable_inline_errors` settings have been forcibly
removed so that form builders cannot select them. This is a safeguard against
allowing untested functionality from being configured.

In the future, if the `form_novalidate` or `form_disable_inline_errors`
functionality is desired, a design and functional behavior specification must
be provided before unblocking these features.

### Third party settings for Smarty integration
A `webform_admin_third_party_settings_form_alter` hook implementation adds
secure configuration for the Smarty (ZIP validation) integration. Third party
settings include:

| Setting    | Description                                    | Governance |
|------------|------------------------------------------------|------------|
| enabled    | Global kill-switch for the integration service | Marketing  |
| auth_id    | The API username                               | IT         |
| timeout    | The API timeout                                | IT         |
| cache_ttl  | The API cache TTL                              | IT         |
| auth_token | The API auth token (secured by Key module)     | IT         |

### Custom tokens
There is a single custom token added: `[webform-submission:confirmation_url]`.
This token forms a stateless, bookmarkable URL that the information request
form submissions are redirected to upon completing a form. This token
incorporates the programs of interest and military status elements to form a
highly personalized confirmation page. A set of query parameters is used by
a block on the confirmation page to provide this personalized experience.

### Calls to action
On canonical webform routes, all calls to action must be removed.

### Webform element configuration form alterations
There are several features that are forcibly disabled globally across all
element types. These features are either undesirable in the design, or may
introduce accessibility problems if used improperly.

The `title_display` configuration for elements allows the element title to be
placed in a variety of locations, such as above, below, inline, or even
completely omitted. In the case of the World Campus form styles design, the
`none` and `inline` options are undesirable, and are therefore removed.

The `description_display` configuration for elements allows the element
description to be displayed in a variety of formats. In the case of the
World Campus form styles design, the `tooltip` option introduces possible
accessibility concerns, so it has been removed.

The `help_display` configuration for elements allows the element help text
to be displayed in a variety of formats. In the case of the World Campus form
styles design, the `title_before` and `element_before` options were undesirable
from an aesthetics perspective, so they have been removed.

The jQuery driven `timepicker` option for date elements has been removed, as
all supported browsers now work with native functionality.

Certain advanced date format options have been removed. The `datetime`,
`datetime-local`, `text`, and `none` options proved to be buggy while
manually testing, and they undesirable anyway, so they have been removed.

Certain advanced time format options have been removed. The `timepicker`,
`text`, and `none` options proved to be buggy while
manually testing, and they undesirable anyway, so they have been removed.

Finally, the `slideout` option for the terms of service element has been
removed due to the wildly unpredictable nature of the terms of service content.
The concern was that a slide-out experience would be difficult to test for
user experience and a modal-based experience would be much more predictable.

### Performant element library attachments
To keep a minimal front-end asset footprint, any additional scripting for
various features have been componentized into their own libraries and attached
as needed. This means that if a feature isn't needed on a page, the scripting
is not included.  There are **_many_** component libraries.

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
[issue queue](https://github.com/psu-online-education/psu_webform/issues).
