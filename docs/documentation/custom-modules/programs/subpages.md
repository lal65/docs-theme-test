---
layout: default
subtitle_before: Custom Modules
title: Programs
subtitle_after: 'Programs: Subpages'
image: inline-assets/documentation/custom-modules/deprecated.svg
menu_title: 'Programs: Subpages'
menu_order: 4
---

## Summary
This submodule contains legacy artifacts that should either be refactored to
other technologies or moved to other modules.

## Admissions tool
The "Admissions tool" is a custom form and block plugin pair that provides
prospective students with direct jump-links to the "How to Apply" section of
the selected program.

## Personalization JavaScript
This simple JavaScript library updates the local storage personalization data
to update the `visited_programs` mapping. The current program either has its
last visited timestamp updated, or has a new entry added if this is the first
time the user has visited it.

This data is subsequently used by the "Program History" custom block type.

## Technical debt
This custom block and form should be replaced with a no-code webform.

In the previous design, this JavaScript needed to apply to all subpages within
a program. Nowadays, since programs have been redesigned as SPA's, this library
should be moved directly to the Programs Personalization module instead.

## Requirements
This module requires the custom Programs module.

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
[psu_programs_subpages issue queue](https://github.com/psu-online-education/psu_programs_subpages/issues).
