---
layout: default
subtitle_before: Custom Modules
title: Webform
subtitle_after: 'Webform: Read Only'
image: inline-assets/documentation/custom-modules/webform/readonly.svg
menu_title: 'Webform: Read Only'
menu_order: 5
---

## Summary
This submodule provides an additional security layer on top of the
off-the-shelf access controls afforded by the Webform module. The purpose of
this module is to completely prevent editing or deleting **_anything_** related
to certain webforms of higher importance to the business. This module forms the
basis of the enforced governance contract for the information request forms.

All changes to forms enrolled in the "readonly" governance contract **_can
only_** be updated via configuration management by IT.

## Third party settings
This module adds a single third-party setting to the webform configuration
entity type: `enforce_read_only`.

### Access controls
If readonly mode is selected, all mutable routes including all edit and delete
forms for webforms and their submissions are restricted such that not even
administrators can access them. The only way that changes can be made are
either through configuration deployments or update hooks.

### Configuration ignore strategy
The strategy for managing config ignored webforms and webform options on this
application is also based around this module. There is a
`config_ignore_settings_alter` hook implementation that adds the appropriate
webform and webform options configuration to the config ignore list based on
the `enforce_read_only` third-party setting. All forms that are not marked as
read-only are automatically added to config ignore.

### Additional safeguards
The third-party setting for enrolling a form into read-only mode is locked on
managed hosting environments because this can lead to data loss. Enrolling new
forms into the read-only governance model must be performed on local
environments.

## Requirements
This module requires the custom Webform module.

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
[issue queue](https://github.com/psu-online-education/psu_webform_readonly/issues).
