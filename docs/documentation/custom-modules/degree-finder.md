---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Degree finder
image: inline-assets/documentation/custom-modules/degree-finder.svg
menu_title: Degree finder
menu_order: 12
---

## Summary
The degree finder module provides multiple forms and blocks that facilitate
navigating to programs and certificates. Some legacy forms and blocks remain
from earlier design iterations and can be removed.

### Degree Finder (Banner)
The degree finder banner block / form is intended to be used on the homepage
content type. It is a unique block in that it utilizes a level 1 heading
element, effectively replacing the global page title block.

![The degree finder banner form is displayed inside the degree finder banner block]({{ "/assets/documentation/custom-modules/degree-finder/degree-finder-banner.png" | relative_url }})

#### Degree Finder (Banner) Form
The form itself has two elements that are intrinsically linked together: a
select element for degree levels and a select element for topics. Special
JavaScript is applied to perform bidirectional filtering to prevent a
scenario where the user would see no results upon submitting.

For example when a user selects a level, all topics that would not yield
results are removed from the topic select. Likewise, when a user selects
a topic, all levels that would not yield results are removed from the level
select.

Upon submitting the form, the user is taken to the full degree finder form with
the topic and level pre-selected so results are visible on first paint.

#### Degree Finder (Banner) Block
The degree finder banner block has a number of customization points. It allows
for customizing the intro copy below the level 1 heading, adding a pool of
background images that are displayed randomly, and adding an optional badge.

### Degree Finder Filter Form
The degree finder filter form exists on its own canonical route. It provides
a faceted search by both degree levels and topics which allow users to
narrow down programs based on their field of interest and educational goals.
The entire filtering experience happens without AJAX, so the user experience is
optimized for responsiveness.

![The degree finder banner form allows for a faceted program search]({{ "/assets/documentation/custom-modules/degree-finder/degree-finder-filter-form.png" | relative_url }})


#### Degree Finder Filter Form Route
The route for this form is `psu_degree_finder.filter_form`, which is used by
the DataLayer alter hook implementation.

### Custom DataLayer hook
There is a `hook_datalayer_alter` implementation that will change the
`site_section` to "Degree finder" if on the `psu_degree_finder.filter_form`
route.

## Requirements
This module requires the custom Programs module.

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
[issue queue](https://github.com/psu-online-education/psu_degree_finder/issues).
