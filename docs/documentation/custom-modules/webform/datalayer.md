---
layout: default
subtitle_before: Custom Modules
title: Webform
subtitle_after: 'Webform: Data Layer'
image: inline-assets/documentation/custom-modules/webform/datalayer.svg
menu_title: 'Webform: Data Layer'
menu_order: 1
---

## Summary
This submodule adds advanced Data Layer support for webform submission events.
It provides a Webform Handler that accepts one or more key-value pairs that are
added to the Data Layer when the form is successfully submitted. It also
provides an interaction tracking mechanism that provides visibility into how
users progress through multistep AJAX forms.

## Data layer handler and supporting custom ajax command
The data layer handler accepts a newline delimited list of pipe-separated
key-value pairs. For example, the following configuration:
```text
test_key|test value
test_key_2|test value 2
```

would be added to the data layer as:

```text
{
  "test_key": "test value",
  "test_key_2": "test value 2"
}
```

The handler also accepts an optional summary of logic for administrative use,
although recent upstream releases of Webform have added their own
administrative notes feature. The custom field should be migrated to use the
official one now that it exists.

When a form is submitted, the handlers will fire their `::postSave` methods.
Within this method, the appropriate data layer values are written to the
Private Temp Store service. In reality, this service is a wrapper around an
anonymous PHP session. Depending on how the form is configured, this session
data is either read immediately (in the same request), or is read subsequently
in the next request if the form redirects the user to its confirmation page.

### Forms that cause a page reload on submit
If a webform is configured to use AJAX and its confirmation type is set to
one of:
- Inline
- Message
- Modal
- None

then an implementation of `hook_ajax_render_alter` is used to read the session
values and append a `DatalayerPushCommand` to the AJAX render array. After
copying the data from the session to the push command, the session is closed to
re-enable full-page-caching support.

### Forms that do not cause a page reload on submit
For all other forms, a `hook_datalayer_alter` implementation performs a similar
function by adding the session values to the pageview event. After
copying the data from the session to the push command, the session is closed to
re-enable full-page-caching support.

## Interaction tracking
While the data layer handler feature works very well for tracking complete form
submissions, it doesn't handle instances where multistep forms are abandoned
partway through. Interaction tracking provides this missing link.

Interaction tracking is configured through a third-party setting on individual
forms. The settings are as follows:

| Setting                     | Description                                                                                               |
|-----------------------------|-----------------------------------------------------------------------------------------------------------|
| enable_interaction_tracking | The toggle to turn interaction tracking on for the form                                                   |
| interaction_form_type       | The form type, which is a general form category (one of `GENERAL`, `PROGRAM`, `CAMPAIGN PAGES`, or `B2B`) |

As soon as the user interacts with a form for the first time (that is, when
focus enters the form), an event is pushed into the data layer, such as:

```json
{
  "event": "formInteraction",
  "formStep": 1,
  "formID": "<the webform machine name>",
  "formCategory": "<the webform category>",
  "formType": "<the form type>"
}
```

This interaction is stored in global state and is remembered for the remainder
of the page view. When the user proceeds to step 2, an additional event is
pushed, this time with `"formStep": 2`. This process continues until the user
either abandons or completes the form. Going backwards in the form will never
cause an interaction event to be pushed, nor will going forward in the form
after going back. In other words, interaction tracking captures the _furthest
point in the form that the user progresses_.

Through using both submission and interaction tracking, Marketing is able to
have a holistic view into how each step of each form of each campaign is
performing.

## Advanced use cases
As with all webform handlers, the data layer handlers can be conditionally
enabled based on arbitrary criteria. For example, there may be one data layer
item that should be pushed only if a user enters a certain value into a certain
field. One real-world use case is that for program of interest 1, if the user
fails to select anything, the analytics team wanted to see `NO_SELECT` as the
value in the data layer.

## Technical debt
The custom summary of logic field should be removed after migrating all data to
the new upstream configuration.

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
[issue queue](https://github.com/psu-online-education/psu_webform_datalayer/issues).
