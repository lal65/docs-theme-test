---
layout: default
subtitle_before: Custom Modules
title: Programs
subtitle_after: 'Programs: Personalization'
image: inline-assets/documentation/custom-modules/programs/personalization.svg
menu_title: 'Programs: Personalization'
menu_order: 3
---

## Summary
This submodule provides personalization services specific to the Program
content type.

## Program history block
The Program history block is a custom block plugin type that will display the
last N programs that a visitor has either viewed or requested information for.
It offers a number of configurable options such as:
- An optional no-results message which is displayed when a user has neither
  viewed nor requested information for any programs. If left empty, the entire
  block will be removed from the DOM in such an event.
- An optional number of programs to display. If left empty, an infinite number
  of programs may be displayed (unpaginated).
- An optional "read more" label, which is generally used to link the user to
  a page which displays more holistic program browsing history.
- An optional "read more" link, which accompanies the read more label.

This block has limited server-side rendering, mainly relying on client-side
JavaScript to read program history from local storage and generate the
appropriate markup.

A limited "teaser" configuration of this block is typically placed on the
**_homepage_** and a more holistic version on the **_My programs_** page,
although this convention is completely managed by the content team.

### Client-side render helper libraries
Two helper libraries are provided which add client-side rendering support for
certain design system artifacts. This is a bit of an oddity given that the vast
majority of lifting is typically performed through server-side rendering.

## Technical debt
The client-side JavaScript utilizes jQuery, which is no longer required.
Vanilla JavaScript is more desirable nowadays.

There is also questionable scoping for the global configuration that is exposed
through this module given the block that uses the configuration is now provided
by the base Programs module itself. This was an architectural shift that
happened during the milestone 4 program page redesign project.

## Requirements
This module requires the custom Programs module.

## Installation
Install as you would normally install a contributed Drupal module. For further
information, see
[Installing Drupal Modules](https://www.drupal.org/extending-drupal/installing-drupal-modules).

## Configuration
This module has global configuration: a series of formatted text settings that
provides markup to display in place of the program request information block
for users that have already requested information for the current program.

This configuration should likely be moved to the base programs module given the
block that it impacts now lives there and the legacy block that used to
reference the configuration in this module no longer exists.

## Maintainers
- Matthew David Webb <mdw15@psu.edu>, Applications Developer Manager
- Brianne Williams <bnh10@psu.edu>, Applications Developer
- Kyle Leber <kjl16@psu.edu>, Applications Developer
- Luke Leber <lal65@psu.edu>, Applications Developer
- Zachary Ishler <zri5004@psu.edu>, Applications Developer

## Support
Submit bug reports and feature suggestions, or track changes in the
[psu_programs_personalization issue queue](https://github.com/psu-online-education/psu_programs_personalization/issues).
