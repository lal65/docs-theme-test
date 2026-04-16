---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Brochures
image: inline-assets/documentation/custom-modules/brochures.svg
menu_title: Brochures
menu_order: 4
---
*[WCGEN]: General World Campus Information

## Summary
The Brochures module provides several customizations to brochure media
management.

### Special download controller
One of the brochure requirements were that end-users should be able to download
a copy of the brochure without having to visit the asset directly on disk.
Nowadays, the
[Media Alias Display](https://www.drupal.org/project/media_alias_display)
module does the exact same thing, but didn't exist 10 years ago.

### Third party settings
Two third party settings are added to the `brochure` media bundle. These fields
designate two special brochures that are used in very specific contexts.

#### Default brochure
The "default brochure", more commonly known as the "WCGEN" brochure, is used
as a generic fallback brochure to offer a user whenever a more specific
brochure is not available.

#### Military brochure
The "military brochure" is displayed in one specific context and is accompanied
by a text blurb that is configured within this module.

### Intro text configuration
A simple configuration containing one text blurb is configured within this
module.

### Technical debt
The military brochure and intro text should be replaced by modern page building
tooling through a custom block plugin, most likely.

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
[issue queue](https://github.com/psu-online-education/datetime_range_timezone/issues).
