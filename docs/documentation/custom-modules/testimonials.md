---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Testimonials
image: inline-assets/documentation/custom-modules/testimonials.svg
menu_title: Testimonials
menu_order: 31
---
## Summary
The Testimonials module customizes all things related to the Story content
type. It features a third party setting on the story content type, a custom
block type, a custom field formatter, and a variety of hooks.

## Third party setting: Implicit menu parent
This feature is no longer required and should be removed.

## Student stories aggregation block
This block simply displays a list of stories rendered as teasers. Nowadays,
there isn't a compelling reason not to use a View here, as story aggregations
are no longer rendered in the context of programs and partners.

## Legacy image field formatter
This field formatter exists to work around a problem that was encountered by
the creative team when originally setting up success stories. The problem is
that the original images for various success stories were either lost or were
never obtained in the first place. This created an unfortunate situation where
the only available images were too small to be displayed in the design without
upscaling (which causes quality to degrade significantly).

To work around this, the legacy image field formatter was created that
intelligently inspects the dimensions of the source image and conditionally
applies different image styles to it. For example, if the source image is less
than 400px in width, it is displayed differently than say, an image that is
1200px in width.

## Node preprocess hook
This hook exists to dynamically build a "view all" link that is displayed on
full story nodes. Previously, stories could render in their canonical URL, or
could be viewed in the context of programs and partners. This level of dynamic
path resolution is no longer required and should be replaced with one path.

## Story form alter hook
This hook adds description text to the title field and adjusts the max length
validation criteria on the call to action text field. The custom validation
criteria were added to guarantee that calls to action meet design expectations.

## Entity display build alter hook
The design calls for conditional rendering. If a story has a video set, then
the video should be displayed in the full view mode, otherwise the story image
should be displayed instead.

## Page title alter hook
This hook overrides the node title with a hard-coded string, "Student
Spotlight". This was a requirement because the University wanted to prevent
the student name from being indexed by search engines or showing up in
analytics in any way, shape, or form. This was deemed a risk if the student no
longer wished to be referenced on the World Campus marketing platform.

## Technical debt
The third party setting is no longer used and should be removed.

Nowadays, the success stories aggregation block should be a simple view. There
are no longer any requirements that would preclude the use of no-code views to
accomplish the task.

The legacy image formatter was to be a stop-gap solution, but has become a
permanent fixture. Success stories should be audited and stories without proper
media assets should be archived and the legacy image formatter should be
removed.

The preprocess node hook should be removed and the title description on the
edit form should be set using a base field override instead.
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
[issue queue](https://github.com/psu-online-education/psu_testimonials/issues).
