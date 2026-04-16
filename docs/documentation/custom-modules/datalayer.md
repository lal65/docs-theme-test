---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: DataLayer
image: inline-assets/documentation/custom-modules/datalayer.svg
menu_title: DataLayer
menu_order: 10
---
## Summary
This module adds a very specific set of DataLayer values (as specified by the
Analytics Team from mid-2018).

### Schema
There are three core data points that are added to every page view. These data
points allow the business to segment analytics in ways that are relevant to their stakeholders.

#### Site Section
The `site_section` DataLayer key represents logical segments of the site from
the perspective of the **_business_** - which is not always the same
perspective from the content management system. The following table defines the
rules for determining the site section.

| CMS Condition                                                  | Site Section                                                       |
|----------------------------------------------------------------|--------------------------------------------------------------------|
| Homepage                                                       | `Home`                                                             |
| Program topics taxonomy term page                              | `Topics`                                                           |
| Program levels taxonomy term page                              | `Degrees and certificates`                                         |
| Search view page                                               | `Search`                                                           |
| Event node page                                                | `Event`                                                            |
| Campaign page node                                             | `Campaign page`                                                    |
| Story page                                                     | `About us`                                                         |
| Events calendar route                                          | `Calendar`                                                         |
| RFI form confirmation page (determined by hard-coded path)     | `RFI thank you`                                                    |
| Webinar form confirmation page (determined by hard-coded path) | `Webinar Thank You`                                                |
| Program node page                                              | `Program`                                                          |
| Node page not fitting any above criteria                       | Attempt to look up a value from the main menu tree (if applicable) |
| Event node served within a program page (custom route)         | `Event`                                                            |
| Any page rendered with admin theme                             | `Admin`                                                            |
| No criteria are matched                                        | `Miscellaneous`                                                    |

The rules are evaluated in order and stop when the first condition is
satisfied. For example, if the homepage somehow renders with the admin
theme, the site section would be `Homepage`, not `Admin`. **_This logic has
the highest Change-Risk-Antipattern-Index of any custom module on the
application!_**

#### Maintenance warning
The conditions determined by hard-coded path will become very fragile as the
Optimized Service Teams are solidified. Marketing will no longer have direct
access to development resources to share responsibilities like this. An
alternative approach should be applied here.

### Program level
The `program_level` also takes a business-focused approach to determining the
value. The following table defines the rules for determining the program level.

| CMS Condition                                          | Program Level                                                                           |
|--------------------------------------------------------|-----------------------------------------------------------------------------------------|
| Program levels taxonomy term page                      | The value of the `field_datalayer_id` field on the current term                         |
| Program node page                                      | The value of the `field_datalayer_id` field on the term assigned to the current program |
| Event node served within a program page (custom route) | The value of the `field_datalayer_id` field on the term assigned to the current program |

### Program ID
The `program_id` takes a more straightforward approach.

| CMS Condition                                          | Program ID                                                         |
|--------------------------------------------------------|--------------------------------------------------------------------|
| Program node page                                      | The value of the `field_program_code` field on the current program |
| Event node served within a program page (custom route) | The value of the `field_program_code` field on the current program |

### A base field is added to menu links
A base field, `datalayer_id`, is a string field added to all menu link content
entities. The value of this field is used to look up a fallback value for the
current page based on its path. The algorithm starts at the current page and
works backwards up to the menu root looking for the first entity that has a
value set.

### Custom hooks
There is one custom hook defined by this module.

#### hook_datalayer_alter
This hook allows arbitrary key-value pairs to be added to the DataLayer.

```php
<?php // custom.module

function custom_datalayer_alter(array &$values) {
  $values['key'] = 'value';
}

```

## Requirements
This module requires no modules outside drupal core.

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
[issue queue](https://github.com/psu-online-education/psu_datalayer/issues).
