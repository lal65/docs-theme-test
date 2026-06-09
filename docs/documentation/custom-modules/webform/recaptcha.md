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
![The ReCAPTCHA v3 element is nestled under the PSU Elements category]({{ "/assets/documentation/custom-modules/webform/recaptcha/add-recaptcha-element.png" | relative_url }})

The element has two main composite fields: `token` and `score`. The `token`
field is a hidden field that holds the reCAPTCHA token that is generated on
the client-side by the reCAPTCHA client library. This token is sent to the
server back-end on form submit where the application queries the Google Cloud
API to retrieve the assessment result for that particular token. The score is
then saved in the `score` value field. This `score` field cannot be modified by
the end-user and may be used in downstream business processes.

If an invalid or missing token is encountered, the `score` field is set to an
empty string to differentiate it from a score of `0`.

### Configuration
![The ReCAPTCHA v3 element has unique options in its configuration interface]({{ "/assets/documentation/custom-modules/webform/recaptcha/configure-recaptcha-element.png" | relative_url }})

In addition to the standard composite element options, the reCAPTCHA v3 element
has a total of 6 customizable properties. These options can be customized on a
form-by-form basis.

#### Enable reCAPTCHA v2 fallback
If selected, end-users may encounter checkbox challenges if they score lowly
enough and an additional 5 options are able to be modified.

#### Threshold
This is a decimal between 0.1 and 1.0 that determines how low a score has to be
in order to trigger a checkbox challenge. The higher the threshold is set, the
more users will be challenged.

#### Low score message
![A checkbox challenge is presented with the text "Before proceeding, we'd like to confirm you're a human."]({{ "/assets/documentation/custom-modules/webform/recaptcha/low-score-message.png" | relative_url }})

This is the message that the end-user will be presented with should they score
below the threshold.

#### Recaptcha v3 message (no token)
![A checkbox challenge is presented with the text "Before proceeding, we'd like to confirm you're a human. You may need to permit Google to run scripts on this page."]({{ "/assets/documentation/custom-modules/webform/recaptcha/no-token-message.png" | relative_url }})

This is the message that the end-user will be presented with should their
browser fail to include a token with its form submission data. This can happen
if certain browser privacy extensions are blocking Google reCAPTCHA.

#### Recaptcha v2 message
![A checkbox challenge is presented with the text "Please check the 'I'm not a robot' box"]({{ "/assets/documentation/custom-modules/webform/recaptcha/recaptcha-v2-message.png" | relative_url }})

This is the message that the end-user will see if they attempt to re-submit the
form without solving the checkbox challenge if they are presented with one.

## Downstream processing
![The reCAPTCHA v3 score is saved along with the submission for downstream processing]({{ "/assets/documentation/custom-modules/webform/recaptcha/admin-view.png" | relative_url }})

Given the score is saved as a persistent data point, it may be used in
conditional handlers. For example, if a form wishes to silently discard any
submissions with a score less than 0.4, that is a decision that can be made
by the form owner.

In practice, the scores are pushed to the data layer on the submission event.

## Custom data layer events
Out of the box, there is little client-side observability with interactive
challenges. Because of this, custom data layer events have been added to give
Marketing visibility into how many reCAPTCHA 2 challenges are:

- Rendered
- Solved
- Expired
- Get an error

### Rendered event
This event is pushed whenever a reCAPTCHA 2 challenge is shown to the user.
Multiple events may be pushed if the user fails to complete the challenge and
attempts to resubmit the form anyway.

```json
{
  "event": "recaptcha",
  "eventAction": "rendered",
  "formID": "<the form id>"
}
```

### Solved event
This event is pushed whenever Google's client-side scripting indicates that a
user has successfully completed the reCAPTCHA 2 challenge.

**Note: Just because a user has solved the challenge does not necessarily mean
that their submission will be accepted!** Challenge solves do time out and can
still fail server-side validation when the result is assessed via API.

```json
{
  "event": "recaptcha",
  "eventAction": "solved",
  "formID": "<the form id>"
}
```

### Expired event
This event is pushed whenever Google's client-side scripting indicates that a
user has successfully completed the reCAPTCHA 2 challenge, but has waited too
long to submit the form. When this fires, a new reCAPTCHA 2 challenge will be
rendered, **_but will not automatically re-fire the rendered event_**.

```json
{
  "event": "recaptcha",
  "eventAction": "expired",
  "formID": "<the form id>"
}
```

### Error event
This event is pushed whenever Google's client-side scripting detects an error
condition. For example, loss of network connectivity between the user's browser
and the reCAPTCHA service will cause an error event to be pushed.

```json
{
  "event": "recaptcha",
  "eventAction": "error",
  "formID": "<the form id>"
}
```

### Looker Studio reporting
All data layer events are fed into the
[Suspicious Leads Dashboard](https://datastudio.google.com/reporting/4932f1a6-3869-42f0-97fb-69f417e63301/page/p_vynoyaet0d).

![The looker studio report gives a very detailed view into reCAPTCHA events and is filterable by campaign, browser, and other criteria]({{ "/assets/documentation/custom-modules/webform/recaptcha/looker-studio-example-report.png" | relative_url }})

This report allows filtering by session source, medium, campaign, date range,
country, state, form ID, page path, and other criteria. It gives a rolled-up
at-a-glance view of period-over-period trends and visual graphs that highlight
trends in affiliate platforms. This dashboard is intended to function as an
early warning system for newly emerging suspicious traffic patterns.

The report is maintained by Wendel Hullihen <wmh5034@psu.edu> and Nora Price
<nqp5361@psu.edu> and is operationally used by Marketing and the
[WC Suspicious Leads Task Force](https://teams.microsoft.com/l/chat/19:3cf260fefa6c49fd98908e7c05c42904@thread.v2/conversations?context=%7B%22contextType%22%3A%22chat%22%7D).

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
