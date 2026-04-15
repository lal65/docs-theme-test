---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Datetime range timezone
image: inline-assets/documentation/custom-modules/datetime-range-timezone.svg
menu_title: Datetime range timezone
menu_order: 8
---
## Summary
The Datetime range timezone module is a fork of the contributed module and
provides a field type that stores a date range plus a configurable timezone.
This functionality was deemed a requirement for the events management system.

### Salesforce <span class="text--contrasting"> Does Not</span> Support This Functionality!
Previously, the Campus Nexus CRM system supported customizing timezones on a
per-event basis, however it does not appear that the Salesforce CRM system
provides feature parity in this regard. For this reason, it may no longer make
sense to maintain the complexity that customizable timezones introduces.

## Field Type: DateRangeTimezone

### Schema
The field schema is as follows:

| Column    | Type         |
|-----------|--------------|
| value     | varchar(20)  |
| end_value | varchar(20)  |
| timezone  | varchar(255) |

## Field Widget: DateRangeTimezone
The field widget extends the core `datetime_range` widget an simply adds a
select form element for the `timezone`.

## Field Formatters
There are two field formatters for the datetime range timezone field type.

### DateRangeTimezoneSingleDate
This formatter has three configurable options:
1. Whether to display the start date or end date
2. The format to display the selected date with
3. Whether to display the timezone or not

### DateRangeTimezone
This formatter has three configurable options:
1. The separator character that is placed between the start and end dates
2. The format to display the dates with
3. Whether to display the timezone or not

### <strong>These formatters do not meet requirements</strong>
There are very complex requirements surrounding displaying dates, therefore
these formatters are not used.

## Requirements
This module requires no modules outside drupal core.

## Installation
Install as you would normally install a contributed Drupal module. For further information, see [Installing Drupal Modules](https://www.drupal.org/extending-drupal/installing-drupal-modules).

## Configuration
This module has no configuration.

## Maintainers
- Matthew David Webb <mdw15@psu.edu>, Applications Developer Manager
- Brianne Williams <bnh10@psu.edu>, Applications Developer
- Kyle Leber <kjl16@psu.edu>, Applications Developer
- Luke Leber <lal65@psu.edu>, Applications Developer
- Zachary Ishler <zri5004@psu.edu>, Applications Developer

## Support
Submit bug reports and feature suggestions, or track changes in the
[issue queue](https://github.com/psu-online-education/datetime_range_timezone/issues).
