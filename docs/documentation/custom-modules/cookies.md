---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Cookies
image: inline-assets/documentation/custom-modules/cookies.svg
menu_title: Cookies
menu_order: 7
---

## Summary
The Cookies module provides server-side, cookie-based visitor segmentation and
supporting infrastructure adaptations that enable personalized experiences to
be delivered safely and efficiently through CDN caching layers.

### Setting cookies for anonymous visitors
There is a business requirement that all requests that come in with certain
query parameters have special cookies set, however the Acquia infrastructure
strips all `Set-Cookie` headers from responses prior to caching them for
security reasons. To work around this, the cookie values are shipped to the
client via the DOM response and are set with JavaScript.

#### Campaign tracking
When a request comes in with a `cid` (case insensitive) query parameter, a
cookie of name `CID` and value of the query parameter must be set on the
`.psu.edu` scope for 1 year.

#### Partner tracking
When a request comes in with a `partner_id` query parameter, a cookie of name
`partner_id` and value of the query parameter must be set on the
`.worldcampus.psu.edu` scope for 3 months.

#### Problems with Apple Intelligent Tracking Prevention
Apple's ITP feature has known issues with this approach that were introduced
around 3-4 years after this workaround was implemented. Cookies set via
JavaScript do not _actually_ use the TTL value that is specified, but is
forcibly reduced to 7-14 days.

For this reason, this approach should be abandoned and replaced with a more
sophisticated edge-based approach using Cloudflare transformation rules.

### Visitor affiliation segmentation
The business has opted to invest in server-side rendering for user
segmentation. This allows for the complete document to be delivered without
any JavaScript based methods altering the page content in ways that might
trigger a flash of original content.

#### Block purpose
The business intent of this block is to perform server-side interception of
World Campus students that have visited the prospect site and direct them to
the World Campus Chaiken Center instead. The block also allows visitors with a
`Student` affiliation to update that affiliation by clicking an "I'm not a
student" button.

#### Unaffiliation controller
There is a controller that has built-in cross-origin protections and handles
re-packing the affiliations cookie without the Student affiliation.

It also gracefully handles an edge case where a user might have an `acquia_a`
cookie set on the `.worldcampus.psu.edu` scope and a second one set on the
`.psu.edu` scope. Given the decentralized nature of the University and the
scarcity of segmentation cookie names available, this was deemed a risk worth
preparing for.

In the event of a user having two `acquia_a` cookies, a warning message is
shown to the end-user.

> Sorry, we are having trouble removing the banner. Please contact
> <a href="https://www.worldcampus.psu.edu/contacts-help">Penn State World
> Campus Technical Support</a> for help with this issue.

#### Technical implementation
This module provides a special block plugin type that conditionally appears based
on the visitor’s suspected affiliation with the University. This affiliation is
managed through the `acquia_a` cookie.

This cookie has a unique interaction with the Acquia infrastructure in that
responses are able to vary based on the value of a single cookie. Functionally,
this behavior is implemented via the
[Acquia Cookie Vary](https://www.drupal.org/project/acquia_cookie_vary) module.
That module implements a response subscriber which extracts the `acquia_a`
`Set-Cookie` header value into two custom response headers:
`X-Acquia-Cookie-Key` and `X-Acquia-Cookie-Value`.

These headers are then consumed by the Varnish `vcl_hash` subroutine to ensure
distinct cache entries for each affiliation permutation. Similarly, the
Cloudflare CDN uses a cache rule that varies the cache key on the `acquia_a`
cookie for the relevant page scopes.

By structuring the system this way, visitors are able to receive fully
personalized, pre-rendered pages directly from the CDN without any JavaScript-
driven layout shifting or client-side content replacement.

## Requirements
This module requires the contributed
[Acquia Cookie Vary](https://www.drupal.org/project/acquia_cookie_vary)
module.

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
[issue queue](https://github.com/psu-online-education/psu_cookies/issues).
