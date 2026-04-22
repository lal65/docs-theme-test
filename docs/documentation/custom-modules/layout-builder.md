---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Layout Builder
image: inline-assets/documentation/custom-modules/layout-builder.svg
menu_title: Layout Builder
menu_order: 19
---
## Summary
The Layout Builder module provides the basis for much of the less-structured
modern content management. It extends the core Layout Builder module and
provides a custom plugin stack, more fine-grained role based access control,
and a first class integration into many design system features.

This module is one of the most foundational parts of the newest iterations of
designs and the local coupled content model relies almost exclusively on it.

## General Layout Behaviors
In general, all layouts are designed to be **_extremely_** forgiving,
especially when dealing with highly dynamic content. All layouts accommodate
components of all width, shapes, and sizes - relying on flex-based containers
and opportunistic rendering to omit empty elements. More specifically, these
rules generally apply:

1. If a block has nothing to render, it outputs no markup
2. If a layout region has no blocks to render, it outputs no markup
3. If a layout has no regions to render, it outputs no markup

## Interspersing reusable and inline content
It is generally expected that both inline and reusable content can be freely
interspersed in all layouts. This means that template overrides should apply
to both flavors of blocks.

## Custom Layout Plugins
There are 11 custom layout plugins that provide various functionality, ranging
from multi-column, interactive controls, to extremely specialized one-off use
cases.

### Common Base Functionality
There are several configurations that all layout types have.

1. Vertical space - this configuration controls the vertical space below the
   layout. Options are "None", "Small", and "Large"
2. Site section - this configuration controls a label that is used by the
   section view tracking component to empower extremely granular reporting as
   tightly scoped as the current page. The site section preset options are
   customizable on a per-entity-bundle-type basis.
3. Custom site section link - this configuration allows for a custom anchor
   link to the layout
4. Custom site section title - this configuration allows for a custom site
   section for the layout
5. Include in ToC - this configuration allows for the layout to be added to
   a table of contents, if such a feature exists on the current page
6. ToC label - this configuration allows for selecting a preset table of
   contents option. The ToC label options are "Use site section label" and
   "Enter custom value..."
7. Custom ToC label - this configuration allows for customizing the table of
   contents link title

#### Site Sections
This tracking strategy hails from a time when massive content re-working was
happening, but the desire to maintain analytics continuity burned strong. By
abstracting the GTM tracking through a DataLayer based approach, major content
re-work could happen on a rolling basis without causing any disruptions to
Google Analytics.

For example, navigation to a one column layout whose site section is "Demo
Section" results in the following DataLayer event:

```json
{
  "event": "section_view",
  "section_view_title": "Demo Section",
  "tags": {
    "activation_type": "USER_ACTIVATE",
    "section_type": "onecol"
  }
}
```

Navigation or activation of interactive controls within other sections will
result in more `section_view` events which are able to produce very specific
reports and also be aggregated together for holistic content type reporting. 

## Types of Layouts
There are presently 11 types of layouts on this application:
- One column
- Two column max
- Three column max
- Four column max
- Full width
- Full width (randomized)
- Accordion Groups
- Tabs
- Progressive Disclosure
- At a Glance
- How to Apply

It is understood that these layouts may be used in any future designs within
the scope of their functionality to achieve new no-code page building
experiences.

### Multi-column layouts
The multi-column layouts are available in one, two, three, and four column
varieties. These layouts are labeled to content managers as "N columns max"
meaning that the content will display in at most N columns, while automatically
collapsing to fewer columns as appropriate. This collapsing can happen in
multiple ways:
1. If the content runs out of horizontal space due to minimum width
   requirements
2. If one or more columns has no meaningful content to display
3. At certain breakpoints if collapsing has not already happened automatically

### Full width layouts
There are presently two full-width layout types: full width and full width
(randomized). Both layout types span the entire viewport and have no padding.
Only very specific designs are intended to be used in full width layout types.

#### Randomized full width layout
This specialization allows for performant client-side randomization of content
in that only a single block that is placed in this layout will be displayed
randomly on each page load. This randomization happens via the
[Random Item](https://psu-online-education.github.io/?p=viewall-organisms-random-item)
design system utility library.

On the layout builder editorial interface, all blocks are rendered in preview
mode sequentially.

### Interactive layouts
Some layouts are focused around interactive design patterns such as accordions,
tabs, or other expandable widgets. These layouts often have customization
points specific to the design patterns themselves. A common recurring pattern
with interactive layouts is an `intro` region of sorts, which is always visible
regardless of the activation state of the underlying widgets.

#### Accordions Groups
Accordion Groups have special configurations for the accordion item semantic
and style. This allows accordions to act as semantic heading elements while
still retaining design freedom that is decoupled from the semantic.

### Dynamic-region layouts
Some layouts contain a number of regions that are largely unknown ahead of
time. Three flavors of such layouts include _Accordion Groups_, _Tabs_, and the
very specific _How to Apply_ layout types. These layout types are unique in
that the number of regions are defined through layout configuration. Each
region comes with its own set of configurations which allows for very granular
control.

- Anchor link - the options here include three different types: "Derive
  automatically from title", "Enter custom value...", or allows the content
  creator to select a preset value configured on the content type.
- Custom anchor link - this is a free-form text field that allows for any valid
  anchor link to be entered
- Custom site section title - this is a free-form text field that allows for
  any site section title to be entered
- Include in ToC - this is a checkbox that allows an individual region to be
  added to the table of contents
- ToC label - this select has two options: "Use site section label" or "Enter
  custom value..."
- Custom ToC label - this is a free-form text field that allows for any table
  of contents label to be entered


## Requirements
This module requires no modules outside drupal core.

## Installation
Install as you would normally install a contributed Drupal module. For further
information, see
[Installing Drupal Modules](https://www.drupal.org/extending-drupal/installing-drupal-modules).

## Configuration
This module exposes a single configuration: a toggle for globally disabling
geolocation calls.

## Maintainers
- Matthew David Webb <mdw15@psu.edu>, Applications Developer Manager
- Brianne Williams <bnh10@psu.edu>, Applications Developer
- Kyle Leber <kjl16@psu.edu>, Applications Developer
- Luke Leber <lal65@psu.edu>, Applications Developer
- Zachary Ishler <zri5004@psu.edu>, Applications Developer

## Support
Submit bug reports and feature suggestions, or track changes in the
[issue queue](https://github.com/psu-online-education/psu_geolocation/issues).
