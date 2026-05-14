---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Pages
image: inline-assets/documentation/custom-modules/pages.svg
menu_title: Pages
menu_order: 23
---
## Summary
This module customizes behaviors related to general pages, although some degree
of cross-leak into various other content types still remains.

## Military brochure block & analytics
There is a special block type that provides a download link for the military
brochure. Attached to this block are custom analytics such that download clicks
will register a datalayer event as follows:

```json
{
  "event": "pdfClick",
  "pdfClickType": "Military Brochure Download"
}
```

## Page tile navigation block
This block type is primarily used on level 1 navigation pages. It displays
child pages in a grid of tiles. Two types of tiles display in their own
distinct groups:
1. Regular tiles - these tiles display the page title and summary
2. Audience tiles - these tiles display the page title and an image

The origins of the business need for this distinction has been lost over time.

### Audience tiles
To achieve the "audience tile" design, this module adds a pair of base fields
to the core `menu_link_content` entity type. The `audience_navigation` field is
a boolean which denotes a menu link as an "audience navigation" item. The
`audience_navigation_image` field is an entity reference to an image media
item. This image is displayed within the tile view of the menu link.

## Automatic path aliasing enhancements
There was originally a concern that changes to menu links would not
automatically update the page URLs to follow suit. As a result, every time a
main menu link content entity is updated, the associated node's path (if such a
node exists), is updated based on the new position of the menu link in the
hierarchy. This is achieved through entity create, update, and delete hooks.

## More intelligent path alias defaults
There was a desire to have _dynamic_ default values for path aliases based on
whether a node is in the main menu or not. For nodes not in the main menu, a
default pattern of `/[node:title]` should be used. For nodes that are in the
main menu, a default pattern of
`[node:menu-link:parents:join-path]/[node:title]` should be used.

## Legacy event selection management
The original requirements for selecting events to display were incompatible
with what views could accomplish. Therefore, custom logic was implemented that
met all requirements. This logic used to power many different content types and
was designed to be pluggable. This is the only remaining implementation of that
system, and internally it simply powers an entity query.

### Architectural problems
Over time, an architectural smell has formed such that the Symfony event added
by the pages module now powers the news and program modules' event selection.
The news and program implementations should be refactored to use entity queries
directly instead.

## Technical debt
The military brochure block is currently surfaced on 3 unique paths, none of
which are in service. It appears that the content management team has
refactored the military site section content and neglected to follow up about
this feature.

This block should either be re-placed based on new requirements from the
content management team or deprecated / removed entirely.

## Requirements
This module requires the contributed `pathauto` module and the custom
`psu_events` module.

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
[issue queue](https://github.com/psu-online-education/psu_pages/issues).
