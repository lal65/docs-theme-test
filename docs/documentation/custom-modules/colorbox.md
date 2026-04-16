---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Colorbox
image: inline-assets/documentation/custom-modules/colorbox.svg
menu_title: Colorbox
menu_order: 7
---

## Summary
The Colorbox module provides customizations for the
[colorbox](https://www.drupal.org/project/colorbox) media gallery.

### Customized formatter template
The colorbox formatter template has been customized to work with the design.

### Colorbox settings alter
The colorbox settings alter hook adds three settings, the rendered output of
the chevron-right, chevron-left, and close sprites.

### Technical debt
The formatter template override really belongs in the theme.

### Interesting concern
The `psu_colorbox_colorbox_settings_alter` hook could produce fatal exceptions
if the module is installed without using the worldcampus theme.

## Requirements
This module requires the contributed
[Colorbox](https://www.drupal.org/project/colorbox)
module.

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
