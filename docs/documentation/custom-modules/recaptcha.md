---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: ReCAPTCHA
image: inline-assets/documentation/custom-modules/recaptcha.svg
menu_title: ReCAPTCHA
menu_order: 27
---
## Summary
The reCAPTCHA module provides a performance-centric integration with the
reCAPTCHA v3 Google product. Featuring secure key storage and an extremely
performant promise-based loader, this module forms the base for the invisible
reCAPTCHA integration. It also provides a very simple SDK, long before Google
offered an official one. The last thing that this module provides is a very
easy to use testing mock which can be used to make scoring deterministic for
automated behavioral testing.

## Promise based loader
The promise-based loader allows for deterministic, delayed loading of the large
reCAPTCHA package from Google.  The `Drupal.behaviors.recaptchaLoader.load`
function returns a promise that handles all internal details.

```js
Drupal.behaviors.recaptchaLoader.load().then(() => {
  // Use the reCAPTCHA library here.
});
```

This allows integrators to defer loading huge JavaScript payloads until
required. This optimization proved to be one of the more influential changes
on the journey to meet the Core Web Vitals evaluation.

## Secure key management
This module has a simple configuration schema that encourages the use of the
Key module to pull the sensitive secret key from a secure storage location.

## Verification service SDK
Three SDK methods are exposed, one of which is no longer used.

1. `RecaptchaVerificationInterface::verify` - this is currently unused, but
   previously returned a raw site verify call response from Google.
2. `RecaptchaVerificationInterface::getScore` - this performs an invisible
   challenge verification.
3. `RecaptchaVerificationInterface::verifyV2` - this performs an "I am not a
   robot" challenge verification.

## Mock Service
The mock service is provided through a submodule that performs ServiceProvider
based service swaps. This mock service is installed on all non-production
environments in order to:
1. Optimize cost savings - we're charged by the API call, so ensuring that only
   production environments are capable of talking to Google is a no-brainer.
2. Make behavioral testing easy - through special query parameters, the calls
   can be made to behave deterministically.

### Deterministic invisible scoring
Through the query parameter `score`, one can effectively set the result from
the `RecaptchaVerificationInterface::getScore` method. The score is expected to
be a float from 0 to 1.

### Deterministic "I am not a robot" challenge result
Through the query parameter `recaptcha_v2`, one can effectively set the result
from the `RecaptchaVerificationInterface::verifyV2` method. The query parameter
is expected to be a boolean (1 or 0).

## Technical debt
Nowadays, Google offers their own PHP SDK for reCAPTCHA which should likely be
used instead. There is an IT Governance project open to investigate this.

## Requirements
This module requires no modules outside drupal core.

## Installation
Install as you would normally install a contributed Drupal module. For further
information, see
[Installing Drupal Modules](https://www.drupal.org/extending-drupal/installing-drupal-modules).

## Configuration
This module exposes a simple configuration that takes two strings:
- Site key
- Secret key

It is recommended that the contributed Key module is used to secure the secret
key.

## Maintainers
- Matthew David Webb <mdw15@psu.edu>, Applications Developer Manager
- Brianne Williams <bnh10@psu.edu>, Applications Developer
- Kyle Leber <kjl16@psu.edu>, Applications Developer
- Luke Leber <lal65@psu.edu>, Applications Developer
- Zachary Ishler <zri5004@psu.edu>, Applications Developer

## Support
Submit bug reports and feature suggestions, or track changes in the
[issue queue](https://github.com/psu-online-education/psu_recaptcha/issues).
