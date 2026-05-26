---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Taxonomy
image: inline-assets/documentation/custom-modules/taxonomy.svg
menu_title: Taxonomy
menu_order: 30
---
## Summary
The Taxonomy module customizes all things related to taxonomy. It features
several hooks and context-aware custom block plugin types.

## Call to action hooks
This hook is specific to the Secondary Topics vocabulary. These pages feature
an embedded information request form, so the request info call to action is
altered to link to the embedded form instead of the general information
request form.

## Form alter hooks
There is a specialized form alter hook that targets Program Collection blocks
on Secondary Topic pages. There is a business rule that mandates that when
program collection blocks are used on secondary topic pages, the program
collection must filter the program list by the current term. This adds a
pseudo-context-awareness to the otherwise contextless block content type.
Likewise, filtering by program level is also disabled.

The goal here is to allow for 100% hands-free block configuration on secondary
topic pages.

There is also an older procedural hook that adds `#description` markup to the
name and description fields. This hook should be replaced with base field
overrides.

## Gin hooks
As of Drupal 10.6, the Gin admin theme does not render taxonomy term edit forms
using the usual Gin content edit form. This simple hook allows term edit pages
to look just like regular content pages from a content manager's perspective.

## Page title alter hooks
There is a page title alter hook implementation that adds the appropriate
image, title, and title suffix for primary topics, secondary topics, and
program levels.

### Primary topics
The primary topics title is sourced from the default title field, the title
suffix is always "Online Degree and Certificate Programs", and the optional
image is sourced from the `field_image` field.

### Secondary topics
The secondary topics title is sourced from the `field_program_navigation_title`
field. The title suffix is always "Online Degree and Certificate Programs", and
the optional image is sourced from the `field_image` field.

### Program levels
The program levels title is sourced from the default title field, there is
never a title suffix, and the optional image is sourced from the `field_image`
field.

## Page attachment hooks
There is a page attachment hook implementation that adds the Calendar Filters
vocabulary to the `drupalSettings` object. This information is used by the
Webinar content type for front-end based filtering.

## Related topics block
![The related topics block has a heading, intro paragraph, and grid of related topics.]({{ "/assets/documentation/custom-modules/taxonomy/related-programs-block.png" | relative_url }})

The related topics block is a context-aware block type that enables a
"web-like" amongst both primary and secondary topics. It operates by reading
from a structured taxonomy reference field on both the primary and secondary 
topics vocabularies. These related topics are displayed in a grid, enabling
smooth navigation across the web of topics. It also has customizable heading
and intro markup fields.

## Secondary topics request information block
![The secondary topics request information block has a heading, intro paragraph, and a webform.]({{ "/assets/documentation/custom-modules/taxonomy/secondary-topics-request-information-block.png" | relative_url }})

The secondary topics request information block is a context-aware block type
that features a webform whose program list is automatically filtered to only
display programs that are tagged with the current secondary topic. It also has
customizable heading, image, and intro-copy fields. The webform itself is
pulled from a structured data field.

## Technical debt
The functionality provided by this module should likely be ported to both the
Programs and Webform modules.

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
[issue queue](https://github.com/psu-online-education/psu_taxonomy/issues).
