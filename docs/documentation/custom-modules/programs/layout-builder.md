---
layout: default
subtitle_before: Custom Modules
title: Programs
subtitle_after: 'Programs: Layout builder'
image: inline-assets/documentation/custom-modules/programs/layout-builder.svg
menu_title: 'Programs: Layout builder'
menu_order: 2
---

## Summary
This submodule provides layout builder customizations that are only applicable
to the Program content type.

## Content Syndication Controls
There exists a Layout Builder Style that adds a toggle to blocks which prevents
content syndication in certain contexts. There is a long-standing legal
requirement that mandates that all course material be displayed on the student
portal for World Campus at https://student.worldcampus.psu.edu. Rather than
duplicating content across multiple content management systems, there is a
simple REST API that syndicates **_partial content_** from the prospect site
application.

By default, Layout Builder Styles are global options. This module acts to
**_hide_** the layout builder style control on all blocks **_unless_** they are
on the Program content type.

## Heading style default label verbiage
It was requested that the empty option label for the Faculty Collection block
type be updated from "None" to "Default". This is accomplished via a simple
form alter hook.

## Custom vertical space default value for course collection block types
There exists a Layout Builder Style that adds a "space below this block"
configuration. It was requested that the course collection block type,
specifically, have a customized default value for the vertical space. The
desired default value is "Large" for only this block type.

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
[psu_programs_layout_builder issue queue](https://github.com/psu-online-education/psu_programs_layout_builder/issues).
