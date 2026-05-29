---
layout: default
subtitle_before: Custom Modules
title: Webform
subtitle_after: 'Webform: Recaptcha'
image: inline-assets/documentation/custom-modules/webform/recaptcha.svg
menu_title: 'Webform: Recaptcha'
menu_order: 6
---

## Summary
This submodule provides a composite webform element type that implements an
intelligent combination of reCAPTCHA products in order to provide security
without adding friction.

## Implementation strategy
The element type utilizes Google reCAPTCHA version 3 (invisible recaptcha) to
perform heuristic based scoring on form submissions. Each form is able to
specify the score threshold at which additional challenges must be passed in
order to be accepted. If a user falls below the configured threshold, then they
may be forced to complete a Google reCAPTCHA version 2 (checkbox) challenge.

The operating principle is that the vast majority of users score high enough to
never encounter any sort of captcha at all, while automated traffic is blunted
by the checkbox challenge. Google Cloud Console data reinforces the assertion
with over 95% of assessments falling above the default challenge threshold
globally.

**Note:** form owners are not required to enable the "low score fallback"
mechanism. This has been used very conservatively in the past to gather
baseline score metrics to better inform how a particular form should configure
its threshold. If the low score fallback is not enabled, then all submissions
will be allowed to pass while having their scores silently recorded in the
background.

## Recaptcha composite element type
The element has two main composite fields: `token` and `score`. The `token`
field is a hidden field that holds the reCAPTCHA token that is generated on
the client-side by the reCAPTCHA client library. This token is sent to the
server back-end on form submit where the application queries the Google Cloud
API to retrieve the assessment result for that particular token. The score is
then saved in the `score` value field. This `score` field cannot be modified by
the end-user and may be used in downstream business processes.

If an invalid or missing token is encountered, the `score` field is set to an
empty string to differentiate it from a score of `0`.

## Downstream processing
Given the score is saved as a persistent data point, it may be used in
conditional handlers. For example, if a form wishes to silently discard any
submissions with a score less than 0.4, that is a decision that can be made
by the form owner.

In practice, the scores are pushed to the data layer on the submission event.

## Technical debt
In late 2025, Google announced a new product enhancement called
[Policy Based Challenges](https://security.googlecloudcommunity.com/community-blog-42/stop-guessing-start-challenging-introducing-recaptcha-s-policy-based-challenges-5995)
which effectively does the same fallback behavior as the custom back-end code
in this module does. There is currently a project in IT Governance to adopt
the new product and sunset parts of this custom module.

## Requirements
This module requires the custom Webform and Recaptcha modules.

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
[issue queue](https://github.com/psu-online-education/psu_webform_recaptcha/issues).
