---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Campaign pages
image: inline-assets/documentation/custom-modules/campaign-pages.svg
menu_title: Campaign pages
menu_order: 5
---

## Summary
The Campaign pages module provides customizations for the campaign page content
type.

### Campaign request information block
The campaign request information block is a custom block plugin type that stems
from a design era when the request information form was not located in the page
content, but was instead part of another global theme region. To preserve
backwards compatibility, this block still exists, but is added to modern
campaign landing pages via the Layout Builder tooling.

### Custom datalayer page view information
The pageview datalayer event will have the configured campaign type added to
it. If there is no configured campaign type, `Other` will be added as the
`campaign_page_type` value.

### Technical debt
There is a lot of cleanup to perform on both fields and custom code for this
module.

1. Delete the `preprocess_page`, `page_attachments`, `node_form_alter`,
`theme_suggestions_alter` hooks
2. Simplify `preprocess_page_title` hook by removing options to display the
   information request form and call to action buttons within the page title
3. Simplify the `page--campaign-page.html.twig` template by removing all paths
   except for the modern content management mode
4. Remove the `field_content_management_mode`, `field_display_rfi`,
   `field_link`, `field_brand_boilerplate`, `field_featured_item_aside`,
   `field_rfi_floating_cta`, `field_image` `field_block_visibility`, 
   `field_intro_copy`, and `field_content` fields
5. Delete the floating rfi block from configuration
6. Delete the floating request information block plugin
7. Delete the unused libraries

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
