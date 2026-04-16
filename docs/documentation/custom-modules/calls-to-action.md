---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Calls to action
image: inline-assets/documentation/custom-modules/calls-to-action.svg
menu_title: Calls to action
menu_order: 5
---

## Summary
The Calls to action module provides central management for mission-critical
calls to action, such as requesting information and applying. This module
was originally built in a time when calls to action requirements were
**_much_** more complex than they are today. Whether this module still
makes sense to maintain versus a more simplified solution should be evaluated.

### Two custom block plugins
There are two custom block plugins provided by this module, one for
requesting information and one for applying.

#### Request Info call-to-action
The purpose of this block is to generate a link that directs the user to the
most relevant information request form based on their current location. By
default, information request calls to action direct users to the General RFI
form, located at https://www.worldcampus.psu.edu/request-information. If the
user is on certain areas of the site, such as program pages or partner pages,
then they will be instead directed to an embedded form on the same page.

#### Apply Now call-to-action
The purpose of this block is to generate a link that directs the user to the
most relevant application resource based on their current location. By default,
apply now calls to action direct users to the How to Apply tool, located at
https://www.worldcampus.psu.edu/admissions/how-to-apply. If the user is on
certain areas of the site, such as program pages or partner pages, then they
will be instead directed to an embedded section on the same page.

### Custom Hooks
There are three custom hooks dispatched by this module which can be used to
dynamically alter the calls to action.

#### hook_cta_apply_alter
This can be used to alter the apply now link.

```php
<?php // custom.module

use Drupal\Core\Link;
use Drupal\Core\StringTranslation\TranslatableMarkup;

/**
 * Implements hook_cta_apply_alter().
 */
function custom_cta_apply_alter(Link &$link, array &build): void {
  $link->setText(new TranslatableMarkup('Apply Today'));
}
```

#### hook_cta_request_alter
This can be used to alter the request information link.

```php
<?php // custom.module

use Drupal\Core\Link;
use Drupal\Core\StringTranslation\TranslatableMarkup;

/**
 * Implements hook_cta_request_alter().
 */
function custom_cta_request_alter(Link &$link, array &build): void {
  $link->setText(new TranslatableMarkup('Request Info Today'));
}
```

#### hook_cta_build_alter (Deprecated)
This can be used to alter the build array. This hook is no longer used and
should be removed.

### Custom Tokens
There are three custom tokens defined by this module that manage the various
application forms. At present, there are three different application forms that
prospective students may need to fill out depending on the program they wish to
enroll in.

The token values consist of carefully created links with tracking
mechanisms built in. The mechanism utilizes the 
[CTA Tracking](https://psu-online-education.github.io/?p=viewall-utility-cta-tracking)
utility.

#### Graduate programs
The graduate program application link is provided through the `[grad_app_link]`
token. Presently, the link has the following characteristics:

Link
: https://gradschool.psu.edu/admissions/how-to-apply

Text
: Begin the graduate school application

Tracking mode
: Synchronous

Tracking description
: Begin Application

Tracking placement
: Content

#### Undergraduate programs
The undergraduate program application link is provided through the `[undergrad_app_link]`
token. Presently, the link has the following characteristics:

Link
: https://gradschool.psu.edu/admissions/how-to-apply

Text
: Begin the undergraduate application

Tracking mode
: Synchronous

Tracking description
: Begin Application

Tracking placement
: Content

#### Undergraduate certificates
The undergraduate certificate application link is provided through the `[undergrad_cert_app_link]`
token. Presently, the link has the following characteristics:

Link
: https://www.worldcampus.psu.edu/Undergraduate-Certificate-Application

Text
: Begin the undergraduate certificate application

Tracking mode
: Synchronous

Tracking description
: Begin Application

Tracking placement
: Content

## Requirements
This module requires no modules outside drupal core.

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
[issue queue](https://github.com/psu-online-education/psu_cta/issues).
