---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Diff
image: inline-assets/documentation/custom-modules/diff.svg
menu_title: Diff
menu_order: 13
---
## Summary
This module extends the contributed Diff module in a variety of ways. The
original requirement that drove the creation of this module was that more than
just nodes needed to be compatible with the tooling. In recent years, the
contributed Diff module has come a long way in supporting this through
[Issue #2452523](https://www.drupal.org/project/diff/issues/2452523), which
makes most of this module obsolete.

### Diffs for custom entity types
Presently, the only custom entity type that remains using this feature is the
Aside entity type. After Issue #2452523 lands, the Aside entity type should be
updated to be compatible with the upstream feature rather than this custom one.

### Theme-specific CSS resets
The original theme developed by Eastern Standard had major problems with the
diff module. Over time, various resets needed to be added to combat the
incompatibilities. These incompatibilities should be re-investigated, as the
original theme no longer exists in that state.

### Diff support for layout builder components
Some pages are exceedingly complex with interactive components nested inside
interactive components. The existing diff layout plugins sometimes struggle to
capture the meaningful changes, so a new plugin was developed that breaks out
pages into the individual layouts, sections, regions, and blocks that comprise
it. This sometimes allows for a much more precise window into the changes.

## Requirements
This module requires the contributed Diff module.

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
[issue queue](https://github.com/psu-online-education/psu_diff/issues).
