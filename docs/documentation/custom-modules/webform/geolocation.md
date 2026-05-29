---
layout: default
subtitle_before: Custom Modules
title: Webform
subtitle_after: 'Webform: Geolocation'
image: inline-assets/documentation/custom-modules/webform/geolocation.svg
menu_title: 'Webform: Geolocation'
menu_order: 3
---

## Summary
This submodule provides targeted client-side geolocation for the custom address
element.

## Third party settings
This module adds a single third-party setting to the Webform configuration
entity type. The `geolocation_enabled` setting controls whether geolocation
applies on a form-by-form basis. This setting can be forcibly disabled by the
global geolocation toggle (controlled by IT).

## Geolocation
The geolocation feature only operates on non-admin pages, meaning it is a
feature exclusively available to prospective students. The first time that
an address composite element that utilizes the country field is encountered
by the end-user, the country is set to the geolocated data point by the
`geolocation_country` cookie that is set by the Geolocation module. The country
is only set **_once_**, meaning that the end-user may change the country to
another option. This is sometimes required for the use case of military service
members serving on bases in foreign countries.

## Known race condition with automated tests
The automated testing platform operates so quickly that there is sometimes a
race condition where the country selection is updated before the geolocation
JavaScript applies. See
[#3](https://github.com/psu-online-education/wcprospect8/issues/3) for more
details.

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
[issue queue](https://github.com/psu-online-education/psu_webform_geolocation/issues).
