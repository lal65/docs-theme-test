---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Announcements
image: inline-assets/documentation/custom-modules/announcements.svg
menu_title: Announcements
menu_order: 0
---
## Summary
The Announcements module provides a specialized block type that can be used to
add global information to the top of all pages. There are a number of
specialized features that makes the announcement more than meets the eye.

### Customizable severity levels
There are three severity levels that content managers may choose from.
1. Notice: for general information (neutral colors)
2. Warning: for specific, high-profile / critical announcements (yellow)
3. Emergency: reserved for immediate emergencies that involve an imminent threat to physical safety (red)

Governance for which severity level is assigned to an announcement falls to the
business.

### Client-side state management
By default, announcements are expanded, however the user can opt to collapse
them. The user preference to have a particular announcement in an expanded or
collapsed state should be persisted across page-views. Therefore, a hash value
of the announcement content is rendered into the HTML and is used by JavaScript
to maintain announcement-specific state management automatically. When an
unrecognized hash is encountered, the default is for an announcement to be open
by default.

### Design system delegation
The front-end business logic is delegated to the design system
[Announcement](https://psu-online-education.github.io/?p=viewall-organisms-announcement)
component.

#### Interesting Concern
The enclosed thin-wrapper template,
`templates/block--psu-announcements.html.twig`, makes a call to
`include @oe/announcement/announcement.twig`. This could result in a fatal
errors at runtime if this module is used without a World Campus theme.

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
