---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Partners
image: inline-assets/documentation/custom-modules/partners.svg
menu_title: Partners
menu_order: 24
---

## Summary
This module customizes the Partner content type, which powers the World Campus
B2B marketing portfolio. Key features include a highly flexible content
builder, information request intake forms, events integration, and tuition
reduction application workflows.

The partners module is a great example of modern architecture featuring bundle
classes, object-oriented hooks, and ample unit test coverage.

## Partner bundle class
The partner bundle class encapsulates the business logic surrounding the
partner content type. This allows for much easier mocking within unit tests.

Any custom code that references partner nodes should use the
`\Drupal\psu_partners\PartnerInterface` typehint.

## Partner attribution cookies
Each time a prospect visits a partner page, a first party cookie is set with
the following parameters:

| Parameter | Value                |
|-----------|----------------------|
| name      | partner_id           |
| value     | <field_partner_id>   |
| expire    | +3 month             |
| path      | /                    |
| domain    | .worldcampus.psu.edu |
| secure    | true                 |
| httponly  | false                |

### Apple Intelligent Tracking Prevention
Apple ITP prevents JavaScript cookies from being set for longer than 7-14 days,
therefore the expiration date cannot be guaranteed. This can be alleviated
through the use of a custom response header and subsequent CDN transformation
rule that creates a proper `Set-Cookie` header as part of the response
transformation pipeline (which occurs after the vendor's Varnish layer strips
out all `Set-Cookie` headers).

## Path alias restrictions
There is a business requirement that states that all partner pages must start
with an approved prefix.  The approved prefixes at this time are:
1. /b2b/
2. /member/
3. /gov/

Any attempts to save a partner without one of these prefixes will result in a
validation error. Instructions are also added to the path alias widget through
a form alter hook implementation.

## Dynamic tuition reduction links
Access to the tuition reduction form requires authentication through the Penn
State single sign on service. As such, the form is actually hosted on
https://student.worldcampus.psu.edu, which is already set up to accommodate
this requirement and has access to the Penn State CPR to pre-fill many form
inputs for the end-user. The partners module will dynamically replace parts of
the `psu_partners.tuition-reduction-link` route based on the current partner
that is being rendered. It will add in query parameters to help pre-populate
the application form after the end-user signs in.

This strategy results in most of the form being automatically pre-filled for
the end user.

## Events system integration
There are two parts to the events system integration: an "Upcoming events"
block as well as a special route that will render a full event within the
context of a partner.

### Upcoming events block
This block is exposed to Layout Builder and may be placed anywhere on the page
at the strategists' discretion. It will display the upcoming events that are
related to the current partner. Activating one of the event links will take the
end-user to a special event page within the context of the current partner.

### Special full event route
This route renders a special minimal header and footer, which keeps the
end-user focused within the context of the current partner. Each of the partner
pages acts as its own "microsite" within the greater application. All features
available for events are equally supported in this context including embedded
event registration forms with seamless On24 integration.

## Information request form block
The request information form block is exposed to Layout Builder and may be
placed anywhere on the page at the strategists' discretion. This is a very
simple block and renders the structured `field_webform` field on the partner
node.

## Allowed values callbacks
There are two
[Allowed Values Callbacks](https://api.drupal.org/api/drupal/core%21modules%21options%21options.api.php/function/callback_allowed_values_function/11.x)
in this module. These callbacks provide an integration with the
[CRM Partners]({{ "/documentation/custom-modules/crm#partners" | relative_url }})
and
[CRM Topics]({{ "/documentation/custom-modules/crm#topics" | relative_url }})
entities. These are artifacts that stem from the era where the Campus Nexus CRM
system was in use at World Campus and this data was not stored in proper
entities.

### Technical debt
Given recent developments that have moved the storage of the CRM Partners and
CRM Topics into first class entities, these allowed value callbacks should be
retired and replaced with simple, off the shelf entity reference fields. This
effort would require a field migration.

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
[issue queue](https://github.com/psu-online-education/psu_partners/issues).
