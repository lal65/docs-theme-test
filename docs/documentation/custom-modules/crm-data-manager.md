---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Legacy CRM Integration
image: inline-assets/documentation/custom-modules/deprecated.svg
menu_title: <svg style="color:var(--invent-orange)" class="sprite sprite--fa-exclamation-circle" aria-label="Deprecated" viewBox="0 0 512 512"><use xlink:href="#fa-exclamation-circle"></use></svg> Legacy CRM Integration
menu_order: 999
---
## Summary

The Legacy CRM Integration module provided support for the Campus Nexus CRM
system. There are still a couple artifacts from this module that are still in
use, but the goal is to port these features to other modules or services.

### Twig Helper Function
A twig helper function, `next_deadline` still exists that will print out a
textual representation of the next available application deadline for a
program.

```twig
{% raw %}<p>The next deadline for ASTAT is: {{ next_deadline('ASTAT') }}</p>{% endraw %}
```

This twig function is presently used in the following templates:

- templates/block/block--block-content--program-collection--accordions.html.twig
- templates/block/block--block-content--program-collection.html.twig
- templates/paragraphs/paragraph--portfolio-collection.html.twig

The program collection block parts should be refactored to use a preprocess
hook instead.

The `paragraph--portfolio-collection.html.twig` template is **_unused_** and is
slated to be deleted as soon as the legacy campaign content management parts
are cleaned up.

### Deadlines Helper Service
The application deadlines resolution logic is among the most complex that is
supported. This helper service is still used widely, but ultimately the goal
is to refactor its inner workings into the more streamlined entity bundle
class itself.

## Requirements
This module requires the `crm` module.

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
[issue queue](https://github.com/psu-online-education/crm_data_manager/issues).
