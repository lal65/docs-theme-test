---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Login
image: inline-assets/documentation/custom-modules/login.svg
menu_title: Login
menu_order: 20
---
## Summary
This module provides customizations related to the login process.

## Security Hardening
The following features are conditionally disabled on environments that require
single sign on:
- User registration
- Password reset form
- Password reset controller
- Native authentication form
- CLI login link controller

These are disabled to lessen the surface area of the authentication mechanisms
in play and only allow single sign on to be used.

## Discoverability Tweaks
There was a request from the SEO/SEM team that the login page be marked as
`noindex` and `nofollow` to prevent these pages from appearing for non-staff in
search results.


## User Dashboard Theme Negotiator
The canonical user entity view display must be rendered with the admin theme.
This is because the main function of these pages is displaying common editorial
functions and actions that the user is the expected to perform.

### Technical Debt
This module attempts to modify front-end libraries that no longer exist. The
libraries were removed during a previous upgrade and the hook that adjusts the
libraries still exists. This hook should simply be deleted.

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
[issue queue](https://github.com/psu-online-education/psu_login/issues).
