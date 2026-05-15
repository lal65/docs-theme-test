---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Programs
image: inline-assets/documentation/custom-modules/programs.svg
menu_title: Programs
menu_order: 26
---

## Summary
This module customizes the Program content type, which powers the World Campus
academic program portfolio. Key features include a core set of structured data
points, a highly flexible content builder, custom information layouts,
information request intake forms, events integration, application deadlines
integration, career outlooks integration, personalization services, and
analytics-friendly external link tracking.

## Organization
Several aspects of this module are organized into specialized submodules. Each
submodule is documented independently under this page.

## Program bundle class
The program bundle class encapsulates the business logic surrounding the
program content type. This allows for much easier mocking within unit tests.
There are also a number of convenience methods that are used to bridge the gaps
between the underlying field storage and the business language commonly
encountered with stakeholders. For example, the `::isIncludedInLists`
method is a succinct way of describing the multiple checks needed to pass for
a program to show up in various lists across the application.

Any custom code that references program nodes should use the
`\Drupal\psu_programs\ProgramInterface` typehint.

## Default content pools
There is presently one global configuration for fallback content that is used
within information request forms when no customized media is selected. This
configuration is an unlimited cardinality media image select. If a particular
program opts not to customize the information request form media, then a
random image is used instead. This random image utilizes the
[Random Item](https://github.com/psu-online-education/components/tree/main/packages/random-item)
design system component.

## Computed Fields
Application deadlines data is managed through a separate content entity which
is synchronized with the Salesforce CRM system. In order to more gracefully
integrate this data with the Drupal CMS, a computed field is used to add this
interop. This `upcoming_deadlines` field comes with its own custom field type
and a default formatter.

### Deadlines field type
The deadlines field type currently calls through to the
`crm_data_manager.helper.program_deadlines` service. Eventually it is desired
to move the business logic of this service into the deadlines field type itself
and deprecate / remove the legacy `crm_data_manager` module entirely.

### Deadlines markup formatter
The deadlines markup formatter is a flexible plugin that allows application
deadlines to be displayed with arbitrary markup interspersed throughout. It
takes a `format` setting which is a string. The format accepts a number of
replacement tokens:

| Token            | Description                                     |
|------------------|-------------------------------------------------|
| @action          | The action the user can take                    |
| @action_deadline | The deadline the user has to take the action by |
| @result          | The result if the user takes the action         |
| @result_date     | The date the result will happen                 |

The replacement patterns for each of the tokens will vary depending on the
current date and time, the deadline date and time, and the semester start date
and time. These are the factors that play into determining the replacement
patterns:

| Condition                     | Description                                                                                                         |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------|
| Semester start date           | This is the date that the semester starts.                                                                          |
| Application deadline          | This is the deadline for applying.                                                                                  |
| Early                         | This is an early deadline. Applicants that apply before this date are given priority consideration.                 |
| Rolling                       | This is a rolling deadline. Applicants are reviewed on a first-come, first-served basis until all slots are filled. |
| Far future                    | This flag is set if the application deadline occurs 8 months or more from the current date.                         |
| Deadline after semester start | This is an unusual flag that is set if the application deadline is somehow after the semester start date.           |

This yields 16 permutations of how an application deadline may be displayed.
There is baked in logic that guarantees that all scenarios end up displaying
an optimized and editorially well-formed expression.

#### @action token replacement
The `@action` token is always replaced with the string "Apply".

#### @action_deadline token replacement
For rolling deadlines, the `@action_deadline` token is replaced with the string
"Now".

For non-rolling deadlines, the token is replaced with the string "by
`{deadline}`"; where `{deadline}` is the application deadline date displayed in
the format `F j` if not `farfuture`, or `F j, Y` if `farfuture`. If the deadline is a far-future deadline and is either an early deadline or 
the semester start date occurs after the deadline, then an additional `,`
character is appended to the date format. This was an editorial requirement
because string patterns like
`Apply by January 1, 2028, to start February 2, 2028` or
`Apply by January 1, 2028, for priority consideration to start February 2, 2028`
must have a trailing comma separating the `@action_deadline` year from the
`@result`.

#### @result token replacement
For early deadlines where the application deadline is after the semester start
date, the `@result` token is replaced with the string "for priority
consideration".

For early deadlines where the application deadline is before the semester start
date, the `@result` token is replaced with the string "for priority
consideration to start".

For non-early deadlines where the application deadline is before the semester
start date, the `@result` token is replaced with the string "to start".

For non-early deadlines where the application deadline is after the semester
start date, the `@result` token is replaced with an empty string.

#### @result_date token replacement
For far-future deadlines where the application deadline is before the semester
start date, the `@result_date` token is replaced with the semester start
date formatted as `F j, Y`.

For non-far-future deadlines where the application deadline is before the
semester start date, the `@result_date` token is replaced with the semester
start date formatted as `F j`.

For deadlines where the application deadline is after the semester start
date, the `@result_date` token is replaced with an empty string.

#### Use Cases
So far, there are a number of use cases across the application including:

| Format                                                           | Context                                                                   |
|------------------------------------------------------------------|---------------------------------------------------------------------------|
| `@action @action_deadline @result @result_date`                  | Used in places where punctuation afterwards is not needed                 |
| `@action @action_deadline @result @result_date.`                 | Used in places where punctution afterwards is needed                      |
| `<strong>@action @action_deadline</strong> @result @result_date` | Used in places where emphasis is intended on the action versus the result |


### Technical debt
todo!

## Requirements
This module requires the custom `crm_data_manager` and `bls` modules.

## Installation
Install as you would normally install a contributed Drupal module. For further
information, see 
[Installing Drupal Modules](https://www.drupal.org/extending-drupal/installing-drupal-modules).

## Configuration
This module has one configuration: a global pool of media images to display
randomly within Request Information blocks as a fallback.

## Maintainers
- Matthew David Webb <mdw15@psu.edu>, Applications Developer Manager
- Brianne Williams <bnh10@psu.edu>, Applications Developer
- Kyle Leber <kjl16@psu.edu>, Applications Developer
- Luke Leber <lal65@psu.edu>, Applications Developer
- Zachary Ishler <zri5004@psu.edu>, Applications Developer

## Support
Submit bug reports and feature suggestions, or track changes in the
[issue queue](https://github.com/psu-online-education/psu_programs/issues).
