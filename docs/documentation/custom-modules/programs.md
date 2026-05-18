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

| Format                                                                                                           | Context                                                                    |
|------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| `@action @action_deadline @result @result_date`                                                                  | Used in places where punctuation afterwards is not needed                  |
| `@action @action_deadline @result @result_date.`                                                                 | Used in places where punctution afterwards is needed                       |
| `<strong>@action @action_deadline</strong> @result @result_date`                                                 | Used in places where emphasis is intended on the action versus the result  |
| `@action <span class="visually-hidden">to the [node:title] program</span> @action_deadline @result @result_date` | Used in places where the program the deadline is for must be disambiguated |

## Next steps block
![The next steps block displays up to three pieces of information: a heading, the next deadline, and a call to action]({{ "/assets/documentation/custom-modules/programs/next-steps-block.png" | relative_url }})

The next steps block is a contextually aware custom block type that
conditionally displays an additional call to action with the intent of
improving application conversion.

### Properties
This block type accepts a customizable heading markup element and heading
level. The call to action is hard-coded with the following properties:

| Property             | Value         |
|----------------------|---------------|
| Label                | How to Apply  |
| Url                  | #how-to-apply |
| Color                | Alternate     |
| Color on dark        | Reversed      |
| Expand to fit        | true          |
| Icon after           | Chevron right |
| Tracking description | How to Apply  |
| Tracking Placement   | Next Steps    |

### Display logic
This block will only appear if the current program is accepting applications
_(as determined by the Salesforce reference data_). If this condition is not
met, this block will remove itself from the layout.

If there are no deadlines in the future, the deadline will be omitted and only
the heading and call to action will remain.

### Other Considerations
This block type is conditionally visible based on external conditions! It must
only be placed in layout configurations that still meet design expectations if
the current block is abruptly removed without warning.

This block type is **not** enrolled in the "auto-hide-on-missing-id"
functionality! If the program lacks a "#how-to-apply" section, the block will
still render with a broken CTA button!

## Application deadline block
![The application deadline block displays the next application deadline with special stylization]({{ "/assets/documentation/custom-modules/programs/application-deadline-block.png" | relative_url }})

The application deadline block is a contextually aware custom block type that
conditionally displays the next application deadline in a stylized fashion.

### Display logic
This block will only appear if the current program is accepting applications
and has an upcoming deadline _(as determined by the Salesforce reference
data_). If either condition is not met, this block will remove itself from the
layout.

### Other Considerations
Due to special design considerations (conditional breakpoints based on sibling
content blocks), this block can only be placed within **At a glance** layouts.
This rule is enforced through a **Layout Builder Restriction**.

## Application deadlines list block
![The application deadlines list block diplays the next N deadlines for the current program with stylized iconography.]({{ "/assets/documentation/custom-modules/programs/application-deadlines-list-block.png" | relative_url }})

The application deadlines list block is a contextually aware custom block type
that conditionally displays the next N application deadlines in a stylized
fashion. The design heavily utilizes iconography to reduce the cognitive effort
needed to interpret the dates and times by visually tying a deadline to an icon
that represents the term (spring, summer, or fall). This list carries
_semantic meaning_, having the terms displayed chronologically.

### Display logic
This block will only appear if the current program has at least one upcoming 
deadline _(as determined by the Salesforce reference data_). **Note - it does
not matter whether the program is accepting applications or not!**

### Other Considerations
Due to special business needs (completely removing the **Deadlines** region
from the layout if there are no upcoming deadlines), this block can only be
placed within the **Deadlines** region within **How to Apply** layout types.
This rule is enforced through a **Layout Builder Restriction**.

## Program application link block
The application deadlines list block is a contextually aware custom block type
that displays a link to the appropriate application. These links have special
tracking parameters set up.

### Graduate programs
![Graduate program links go to the Fox Graduate School.]({{ "/assets/documentation/custom-modules/programs/program-application-link-block-graduate.png" | relative_url }})

Graduate program links go to the Fox Graduate School. These are the tracking
parameters for this configuration:

| Property             | Value                                              |
|----------------------|----------------------------------------------------|
| Label                | Penn State Fox Graduate School Application         |
| Url                  | https://gradschool.psu.edu/admissions/how-to-apply |
| Color                | hollow-dotted                                      |
| Color on dark        | default                                            |
| Expand to fit        | false                                              |
| Icon after           | Arrow up-right from square                         |
| Tracking description | Application                                        |
| Tracking Placement   | How to Apply                                       |

### Undergraduate programs
![Undergraduate program links go to the MyPennState application.]({{ "/assets/documentation/custom-modules/programs/program-application-link-block-undergraduate.png" | relative_url }})

Undergraduate program links go to the MyPennState application. These are the
tracking parameters for this configuration:

| Property             | Value                                             |
|----------------------|---------------------------------------------------|
| Label                | MyPennState Undergraduate Application             |
| Url                  | https://mypennstate.psu.edu/index.cfm/login/index |
| Color                | hollow-dotted                                     |
| Color on dark        | default                                           |
| Expand to fit        | false                                             |
| Icon after           | Arrow up-right from square                        |
| Tracking description | Application                                       |
| Tracking Placement   | How to Apply                                      |

### Undergraduate certificates
![Undergraduate certifiate links go to the Undergraduate Certificate application.]({{ "/assets/documentation/custom-modules/programs/program-application-link-block-undergraduate-certificate.png" | relative_url }})

Undergraduate certificate links go to the MyPennState application. These are
the tracking parameters for this configuration:

| Property             | Value                                  |
|----------------------|----------------------------------------|
| Label                | Undergraduate Certificate Application  |
| Url                  | /Undergraduate-Certificate-Application |
| Color                | hollow-dotted                          |
| Color on dark        | default                                |
| Expand to fit        | false                                  |
| Tracking description | Application                            |
| Tracking Placement   | How to Apply                           |

## Program career information block
![The program career information block displays job titles and career outlook data.]({{ "/assets/documentation/custom-modules/programs/program-career-information-block.png" | relative_url }})

The program career information block is a contextually aware custom block type
that displays various career related data for the current program. This data is
sourced from the
[Bureau of Labor Statistics Integration Module]({{ "/documentation/custom-modules/bls" | relative_url }}).

There are two primary data points that are formatted and displayed by this
block: job titles and career outlooks.

### Job titles
Job titles are simple strings that are displayed in an unordered list.

### Career outlooks
Career outlooks are complex data that individually consist of a job title,
employment delta, and total employment projections over the next 10 years. This
information is augmented by both coloration and iconography to help with
cognitive load reduction.

### Configuration
There are a total of five user-configurable customization points: a boolean
control to display the job titles data, a customizable job titles heading, a
customizable job titles intro markup snippet, a boolean control to display the
employment outlook data, and a customizable heading for the employment outlook
data.

### Display logic
If the job titles control is disabled _or_ there are no job titles for the
current program, then the job titles section is omitted from display.

If the employment outlook control is disabled _or_ there are no employment
outlooks for the current program, then the employment outlook section is
omitted from display.

**Note - If neither the job titles nor employment outlook sections render, the
block is still displayed, but only an empty div is emitted.**

## Program credits and costs block
![The program credits and costs block displays credits and cost information in a stylized format.]({{ "/assets/documentation/custom-modules/programs/program-credits-and-costs-block.png" | relative_url }})

The program career information block is a contextually aware custom block type
that displays a stylized variant of the total credits and tuition costs for the
current program. The design strategically emphasizes the number of credits and
makes the cost a secondary data point. This information is sourced from two
structured data fields on the program. These are required fields, so this block
is unconditionally displayed.

### Display logic
If there is a range of credits to display, a slightly smaller font is used.

### Other Considerations
Due to special design considerations (conditional breakpoints based on sibling
content blocks), this block can only be placed within **At a glance** layouts.
This rule is enforced through a **Layout Builder Restriction**.

## Request information block
![The request information block displays a heading, intro paragraph, and a form.]({{ "/assets/documentation/custom-modules/programs/request-information-block.png" | relative_url }})

The request information block is the feature that drives one of the most
valuable KPIs, information requests. It features a customizable heading, one or
more images to randomly display, an introduction paragraph, and a webform.

### Personalized content delivery
This block utilizes client-side personalization to enhance the visitor
experience. If the visitor has previously requested information for the
current program, the heading, intro, and form are all replaced by a link
to download the program brochure and take the next steps. This is a very
intentional strategic nudge to get the visitor to take the next step and apply
to the program.

**Note - this personalization mechanism also causes the Request Information
call-to-action buttons to be hidden from the page.**

### Special fragment from CRM follow-up campaigns
The previously mentioned personalized content delivery strategy also takes
effect when a visitor arrives with the `#info-requested` fragment in the URL.
The CRM system includes this special fragment in follow-up campaigns sent to
users that have previously requested information for the program.

## Upcoming program events block
todo (next)!

## Technical debt
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
