---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Asides
image: inline-assets/documentation/custom-modules/asides.svg
menu_title: Asides
menu_order: 1
---
## Summary
The Aside module provides a custom entity type that is used to store reusable
information that should never be viewed on its own. Note - this module was
created long before off-the-shelf solutions like the
[Storage](https://www.drupal.org/project/storage) module existed and before the
[Rabbit Hole](https://www.drupal.org/project/rabbit_hole) module had a proper
release for Drupal 8.


### Asides are revisionable
Asides are in all circumstances content that is expected to be shared amongst
a large number of pages. Therefore, they must be revisionable to enable quick
and easy reverting and to leave an audit trail.

### Technical Debt
There are several areas of technical debt to clean up from previous redesigns.

### Custom forms and handlers are still used
Drupal 10 added various reusable entity forms and handlers, including access
control handlers. This module should be updated to leverage those and not
custom ones.

#### Three (probably) unused block plugins
These were originally added to work around shortcomings in the original
theme (by Eastern Standard) that prevented content from spanning 100% of the
viewport and were placed in global theme regions outside the page content.

### Two (probably) unused templates
The `aside-content-add-list` and `featured-item-block` templates and their
associated hooks are likely unused.

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
