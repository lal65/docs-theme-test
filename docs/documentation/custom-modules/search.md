---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Search
image: inline-assets/documentation/custom-modules/search.svg
menu_title: Search
menu_order: 29
---
## Summary
The Search module customizes all things related to search functionality.

## Field config form alters
It is unclear why these alters exist, but they are not useful.

## Search view preprocessing
The search view preprocessing adds customized header and footer markup.

### Header markup
The views header markup will contain a string like "3 results for business" or
"1 result for education".

### Footer markup
The views footer markup will contain a reference to the Student Center
application in the form of "Current students may find more relevant results
for @term by using the Student Center".

## Technical debt
The field configuration form alters are not useful and should be removed.

The header and footer markup _may_ be able to be powered by configuration-based
replacement patterns directly in the view itself.

It is very likely this whole module is no longer required.

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
[issue queue](https://github.com/psu-online-education/psu_search/issues).
