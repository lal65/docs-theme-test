---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Workflow
image: inline-assets/documentation/custom-modules/workflow.svg
menu_title: Workflow
menu_order: 36
---
## Summary
This module provides customizations related to editorial workflow and automatic
revision pruning.

## Design system registration for admin theme
The "editorial dashboard" as it is called is a series of Views that are
presented in a Tabs layout on the default user profile page. These views
provide tables containing content at various workflow stages and are sorted
by role. For example, authors will have their table of drafts, and editors will
see content that was submitted for approval.

To display these views in the standard Tabs component, the design system
binding must be added to the admin theme. This is accomplished via a
`page_attachments` hook implementation.

## Custom tokens
A series of custom tokens exist that normalize content entity revision data.
See https://www.drupal.org/project/drupal/issues/2920310 for the core issue
currently tracking this feature.

The more interesting custom token is
`[content_moderation_notifications:revision_url]`, which builds out a special
URL that automatically logs anonymous users in and redirects them back to the
original location. This URL also works for users that are already authenticated
by simply redirecting them to the final destination. This allows for a much
more intuitive and user-friendly authoring experience.

## Automatic revision pruning
At peak, this application grew to over 500,000 content revisions. The business
has agreed that revisions meeting the following criteria may be pruned:
1. Only the `node` content type is eligible for revision pruning.
2. The revision must not be the latest revision.
3. The revision must not be, or have ever been a default revision.
4. The revision must not be a forward revision.
5. The revision must be older than 6 months.

These criteria are designed to only target old revisions that the public has
never seen and are for all intents and purposes no longer useful.

**Note: regular database backups should provide an overlapping data recovery
mechanism in the event that a critical piece of data must be resurrected.**

## Custom workflow notification recipients
The unique editorial workflow requirements prescribed in the
[Workflow Functional Requirements](https://pennstateoffice365.sharepoint.com/:w:/r/sites/OOE_Projects/_layouts/15/Doc.aspx?sourcedoc=%7B2331FE3D-A6AE-429A-A95B-55677E43A039%7D&file=workflow-functional-requirements-drupal.docx&action=default&mobileredirect=true&DefaultItemOpen=1&wdOrigin=OFFICECOM-WEB.REDIRECT%2CAPPHOME-WEB.SHELL.SEARCH&wdPreviousSession=374ca89f-22ae-418e-a219-65ab1a87dccb&wdPreviousSessionSrc=AppHomeWeb&ct=1780085629244)
required custom code in order to notify the correct person(s) for certain
transitions.

### Functional requirement 3.1 (c): Return to draft
When a workflow change triggers the Return to draft transition, an email
notification must be sent to **_every user that contributed to the forward
revision chain up to that point_**. This is accomplished through a
`content_moderation_notification_mail_data_alter` hook implementation and
strategic revision iteration and checking.

### Functional requirement 3.1 (e): New revision published
When a workflow change triggers the New revision published transition, an email
notification must be sent to **_the first user that created the new draft_**.
This is accomplished through a
`content_moderation_notification_mail_data_alter` hook implementation and
strategic revision iteration and checking.

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
[issue queue](https://github.com/psu-online-education/psu_workflow/issues).
