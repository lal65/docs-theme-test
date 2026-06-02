---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: WYSIWYG
image: inline-assets/documentation/custom-modules/wysiwyg.svg
menu_title: WYSIWYG
menu_order: 37
---
## Summary
This module provides customizations related to the CKEditor 5 WYSIWYG editor
and downstream filter processing.

## Styles dropdown UX enhancement
There is a tiny style library injected for authenticated users that extends the
height of all combobox panels. There are a great number of WYSIWYG styles
supported by this application and having to scroll up and down to view them
proved to be cumbersome.

**Note: this style may no longer be needed in recent versions of CKEditor 5.**

## Accessible tables filter
There is a filter plugin, `a11y_tables`, that transforms semantic table markup
into a format that is renderable by the World Campus Design System. The
following operations are performed:

- Wraps the `<table>` element in a `<div class="a11y-table">` element
- Adds the `a11y-table__table` class to all `<table>` elements
- Adds the `a11y-table__caption` class to all `<caption>` elements in tables
- Adds the `a11y-table__thead` class to all `<thead>` elements in tables
- Adds the `a11y-table__tbody` class to all `<tbody>` elements in tables
- Adds the `a11y-table__tr` class to all `<tr>` elements in tables
- Adds the `a11y-table__th` class to all `<th>` elements in tables
- Adds the `a11y-table__td` class to all `<td>` elements in tables

It also ensures that the `table` library is attached to the page. This approach
was taken so that table styles would not interfere with those outside of the
WYSIWYG context.

## Read more filter
There is a filter plugin, `read_more_filter`, that transforms certain `<a>`
elements into bona-fide
[Read More](https://psu-online-education.github.io/?p=viewall-molecules-read-more)
components. This happens through server-side rendering into a virtual DOM. The
DOM markup is then extracted and replaces the `<a>` that is being transformed.

This markup is non-trivial and includes advanced SVG techniques.

## Wrapper filter
There is a simple filter plugin, `wrapper_filter`, that simply wraps the entire
input with `<div class="wysiwyg">`. This special wrapper is the target of
container queries and ensures that WYSIWYG-only styles do not leak out of the
WYSIWYG governed content.

## Legacy entity embed compatibility workarounds
The contributed entity embed module does not always attach the correct library
dependencies within the CKEditor 5 editor. This module ensures that the
`oembed_lazyload/common` and `oembed_lazyload_youtube/youtube` libraries are
attached appropriately.

**Note: This workaround will not be required when migrated to core media.**

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
[issue queue](https://github.com/psu-online-education/psu_wysiwyg/issues).
