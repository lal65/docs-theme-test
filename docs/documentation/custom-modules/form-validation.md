---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Form validation
image: inline-assets/documentation/custom-modules/form-validation.svg
menu_title: Form validation
menu_order: 9
---
## Summary
The Form validation module overrides the `FormErrorHandler` service provided by
the core Inline Form Errors module. It meets a few specialized design and
user experience requirements.

### Customizations only applied to non-admin pages
The following customizations should **_only_** apply to the front-end theme.

### Errors without a corresponding form element must appear above all others
Some errors, such as a form token expiring, do not have a direct jump-link to
a form element. The core Inline Form Errors module can intermix these errors
in with others that do link to places in the form which can lead to an
inconsistent user experience.

The customized service ensures that all errors that do not have a corresponding
jump-link will be placed **_above_** all other error messages when rendered.

### Iconography must be used
Different icons must be present in the cases of unlinked errors with and
without jump-links. For unlinked errors, an exclamation point icon must be used
and for linked errors an arrow-down icon must be used.

![Two different icons are used in error messages depending on whether the error is linked]({{ "/assets/documentation/custom-modules/form-validation/form-errors-iconography.png" | relative_url }})

### Custom verbiage must be used in the header
The core module verbiage for the messages includes the word "error", which was
not the user experience that was desired.  The business preferred use of the
word "issue" instead, with a customized pair of plural and non-plural forms.

#### Singular errors
> Please resolve this issue before proceeding:


#### Multiple errors
>Please resolve these @count issues before proceeding:

### Custom messages type must be used
The accessible label of the messages region must use language that aligns with
the "issue" verbiage.  The core module uses "Errors" here, but the label must
be

> Form input issues

### Sticky / floating elements must never cover up form labels
A special `data-scroll-offset` attribute is added to jump-links which has
influence over the dynamically maintained `scroll-padding-top` CSS property.
This mechanism acts to ensure that floating sticky elements (such as banners)
never obscure the form input label when a user is scrolled to it.

![The form element and its label are both unobscured when the jump-link is activated]({{ "/assets/documentation/custom-modules/form-validation/form-element-scroll-offsets.png" | relative_url }})

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
