---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: News
image: inline-assets/documentation/custom-modules/news.svg
menu_title: News
menu_order: 22
---
## Summary
This module customizes the News content type. This content type exists because
the World Campus business desires more control over the content that is usually
syndicated from the official PSU News application.

The news content type is one of the few still remaining using the legacy
Paragraphs based content management mechanism as well as an older front-end
design.

### Media Contacts Block
There is a special block type, the Media Contacts Block, that is placed in the
sidebar region of the main news and features index. This block displays a
customizable intro copy HTML snippet as well as one or more Media Contacts
taxonomy term references.

The purpose of this block is to provide points of contact to the Penn State
World Campus media relations team so that visitors may ask questions related
to facts, press issues, or other inquiries.

### Data Layer Customizations
This module guarantees that the site section data layer value for news articles
is "News and Features". It also adds the custom `news_and_features_topic` data
point based on which category the news article is tagged with. This allows the
analytics team to segment engagement based on news article category.

### Extra Fields
There is an extra field defined through code that displays the next upcoming
event related to the news article. This extra field is rendered at the end of
the content region following the news article.

### Colorbox module enhancements
One of the requirements for the news article content type was the ability to
place an arbitrary number of photo galleries on one page. This introduced the
potential for multiple copies of the same image to be included in multiple
galleries. In order to disambiguate the individual media instances, a custom
token was added and used in the formatter configuration that incorporates the
parent entity ID (in this case, a Paragraph) into the resulting ID attributes.

This operates as both a functional and accessibility safety net in the event
that a content manager inadvertently displays the same media item in more than
one gallery on the page.

### Content type edit form customizations
Shortly after launching the minimal viable product of the news content type
feature, it was requested that the content type be expanded to be able to
handle both internal and external news. This was accomplished by adding a radio
button that allows content managers to denote whether the news piece is an
internal or external article.

To keep the editorial experience simple and streamline, different sets of
fields are visible and/or required depending on the internal/external flag.
This is accomplished through a simple form alter hook and use of `#states`.

### Technical debt

#### Lightly used or unused reporting
There is a permission, route, and view that was originally designed to give a
customized overview of all news articles, their publication dates, and
promotion options. Through discussions with the content management team, it was
discovered that this feature is no longer utilized. This feature should likely
be removed.

#### Questionable Template Placement
The template overrides defined within this module should be moved to the theme,
otherwise there exists the risk of fatal errors should the theme be switched
out in the future.

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
[issue queue](https://github.com/psu-online-education/psu_news/issues).
