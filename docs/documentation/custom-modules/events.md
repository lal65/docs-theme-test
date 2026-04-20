---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Events
image: inline-assets/documentation/custom-modules/events.svg
menu_title: Events
menu_order: 16
---
## Summary
The Events module was originally written to handle everything related to event
management. This included a canonical calendar, displaying event nodes in
varying contexts based on how the user accessed the event, and providing the
integration pieces with third party platforms.

### Events calendar
The events calendar is an AJAX powered form that offers a simple faceted search
based on taxonomy. The special `psu_events.calendar` route allows for
programmatic access to the route without relying on paths.

### Event nodes in multiple contexts
In previous design iterations, displaying events was much more complex than it
is today. There are special caching precautions that are still in place that
are likely no longer needed.

### Twig language extensions
This module adds a filter and a function to the twig language which is used to
perform "smart" date and time manipulation.

#### cast_to_datetime
The `cast_to_datetime` filter converts a string to a datetime object. It takes
one additional argument, the timezone to convert the datetime into.

{% raw %}
```twig
{% set datetime = '2026-04-21T15:00:00'|cast_to_datetime('America/New_York') %}
```
{% endraw %}

#### get_date_range_metadata
The `get_date_range_metadata` function gets an array of meta-information for
the provided date range. This information is typically used for conditional
date format processing.

{% raw %}
```twig
{% set start = '2026-04-21T15:00:00'|cast_to_datetime('America/New_York') %}
{% set end = '2026-04-21T17:00:00'|cast_to_datetime('America/New_York') %}
{% set metadata = get_date_range_metadata(start, end) %}

{# metadata:
  [
    'data-same-year', 
    'data-same-month', 
    'data-same-meridiem', 
    'data-same-day',
    'data-end-on-the-hour',
    'data-start-on-the-hour',
    'data-same-timezone',
#}
```
{% endraw %}

This allows for very granular formatting. For example, a same-day event might
be denoted as `March 22, 2022 9:00–10:00 a.m. (EDT)` where the start time
meridiem is omitted. On the other extreme, an event that might span multiple
days is expanded to
`March 22, 2022 9:00 a.m. – March 23, 2022 10:00 a.m. (EDT)` to prevent
ambiguity.

### Third party platform integrations
Third party platform integrations are handled through dedicated submodules.

## Technical debt
There is no longer sufficient complexity to justify the event manager service.
This should be replaced with either a simple entity query or view. The AJAX
driven calendar should be replaced with a simple client-side-only javascript
version. The `EventDateRange` field formatter is also deprecated and all usages
of it should be updated to use the design system smart date and smart time
components instead.

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
[issue queue](https://github.com/psu-online-education/psu_events/issues).
