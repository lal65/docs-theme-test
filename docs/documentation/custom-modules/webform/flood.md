---
layout: default
subtitle_before: Custom Modules
title: Webform
subtitle_after: 'Webform: Flood Control'
image: inline-assets/documentation/custom-modules/webform/flood.svg
menu_title: 'Webform: Flood Control'
menu_order: 2
---

## Summary
This submodule provides a simple webform handler that leverages the core flood
service to enable submission rate limiting.

## Flood handler
The flood handler has two configurable options:

| Option    | Description                                                            |
|-----------|------------------------------------------------------------------------|
| threshold | The number of submissions allowed to pass within the configured window |
| window    | The number of seconds to remember submissions for                      |

For example, a threshold of `5` and a window of `60` will allow a maximum of
5 submissions to pass within a minute per IP address.

It is recommended to put this handler in the first position in the list so that
it is the first to execute. When the flood control activates, the request is
immediately terminated with an HTTP/429 response.

## Requirements
This module requires the custom Webform module.

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
[issue queue](https://github.com/psu-online-education/psu_webform_flood/issues).
