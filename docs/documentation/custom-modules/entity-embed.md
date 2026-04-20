---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Entity embed
image: inline-assets/documentation/custom-modules/entity-embed.svg
menu_title: Entity embed
menu_order: 14
---
## Summary
This module adds a field formatter that meets the very specific original
WYSIWYG editing requirements. WYSIWYG support for media in Drupal core would
not be available for 6 months after this application launched, so the
contributed [Entity Embed](http://drupal.org/project/entity_embed) module is
used for media in the WYSIWYG and not core media.

### Supporting External Linking
The off-the-shelf responsive image formatter does not support linking images to
external addresses. This was deemed a hard requirement.

### More restrictive image style options
The field formatter more tightly restricts the allowed image styles to exactly
4 choices:
- original image
- awards_2x_wysiwyg
- small_graphic_3x_wysiwyg
- small_image_wysiwyg

#### A note on the original image style
This is a **_severe_** performance risk as it could serve massive files.

## Technical debt
Now that there is core media support for CKEditor, the content should be
migrated to use that so entity embed can be dropped.

## Requirements
This module requires the contributed
[Entity Embed](http://drupal.org/project/entity_embed) module.

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
[issue queue](https://github.com/psu-online-education/psu_entity_embed/issues).
