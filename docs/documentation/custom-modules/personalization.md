---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Personalization
image: inline-assets/documentation/custom-modules/personalization.svg
menu_title: Personalization
menu_order: 25
---
## Summary
This module provides the base functionality of all client-side personalization.

## Local storage
This module installs the `wc_personalization` local storage object, which is a
generic, versioned data store. It also provides exception-safe accessors to
both get and set data.

### Upgrade paths
Sequential data upgrade paths are provided by the scripting in this module. The
most common upgrade path is when a program changes its code after being
published, which has only happened a handful of times.

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
[issue queue](https://github.com/psu-online-education/psu_personalization/issues).
