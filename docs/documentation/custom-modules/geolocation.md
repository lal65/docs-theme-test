---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Geolocation
image: inline-assets/documentation/custom-modules/geolocation.svg
menu_title: Geolocation
menu_order: 18
---
## Summary
The Geolocation module provides a performant IP-Based geolocation solution that
sets a first party cookie on all World Campus properties. It provides a
controller that will set a `geolocation_country` cookie in a fashion that does
not have issues with Apple Intelligent Tracking Prevention.

### Questionable scope
This module also provides a JavaScript library that interfaces with a custom
Webform element type. This library should likely be moved to the dedicated
`psu_webform_geolocation` module given it no longer has to manage multiple
consumers.

## Requirements
This module requires the contributed
[Neutrino API](https://www.drupal.org/project/neutrino_api) module.

## Installation
Install as you would normally install a contributed Drupal module. For further
information, see
[Installing Drupal Modules](https://www.drupal.org/extending-drupal/installing-drupal-modules).

## Configuration
This module exposes a single configuration: a toggle for globally disabling
geolocation calls.

## Maintainers
- Matthew David Webb <mdw15@psu.edu>, Applications Developer Manager
- Brianne Williams <bnh10@psu.edu>, Applications Developer
- Kyle Leber <kjl16@psu.edu>, Applications Developer
- Luke Leber <lal65@psu.edu>, Applications Developer
- Zachary Ishler <zri5004@psu.edu>, Applications Developer

## Support
Submit bug reports and feature suggestions, or track changes in the
[issue queue](https://github.com/psu-online-education/psu_geolocation/issues).
