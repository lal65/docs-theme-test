---
layout: default
subtitle_before: Prospect Site
title: Custom Themes
subtitle_after: World Campus
image: inline-assets/documentation/custom-themes.svg
menu_title: World Campus
menu_order: 1
---
## Summary
The World Campus theme provides the vast majority of the front-end
components, scripts, and styles. It provides an integration with the World
Campus Design System. It also removes, replaces, and / or extends many
third-party libraries to meet the unique needs of World Campus.

This theme is **_old_** and has seen as many as 4 major redesigns.

## Key strategies
This theme is designed to be extremely light-weight and highly performant. It
is optimized for Core Web Vitals and is one of the top leaders of the pack in
competitive performance benchmarking against other major Universities and
internal peer units.

### Conditional library inclusion
Bundlers have typically been used to create aggregates that are shared
throughout entire applications. This can yield extremely large asset files that
contain a lot of unused symbols.

The World Campus theme operates differently: it always provides optimized
aggregates that only contain the components that are used on any given page.
This has resulted in having an average CSS payload of less than 25kB. The
long-term goal has been to completely remove styles from the critical rendering
path to achieve a single round-trip First Contentful Paint. Inlining the
critical CSS has **_not yet been implemented_** due to other organizational
problems that became hard blockers in late 2024 (forced use of Adobe Webfonts).

### Conditional rendering
This application has complex business rules that rely on multiple third-party
systems that Marketing is not in control of. The application has to
automatically and gracefully react to data changes in these external systems
which may occur at random times. One key strategy is the use of conditional
rendering tactics that prevents extraneous DOM elements from being added if
there is no meaningful content to render inside. For example, if a block is
added that displays Career Information sourced from the Bureau of Labor
Statistics data source, but there is no data to display, two things should
happen:
1. The career information block should not render
2. If there are no other blocks in the layout region other than the career
   information block, then the layout region should not render.
3. If there are no other blocks in _any_ layout regions other than the career
   information block, then the entire layout should not render.

This same logic tracks all the way up to the global theme region. In short,
there should **_never_** be empty DOM elements output, but instead the theme
should automatically reformat itself based on predefined rules to gracefully
adapt to the optimal layout strategy in all circumstances -- without any human
intervention.

## Random images
There are a number of raster and vector images committed to the theme
repository. It's unclear exactly how widely used these images are.

These should likely be audited through the CDN access reporting and pruned
appropriately.

## Design system integration
The design system integration is presently provided through a
`hook_library_info_alter` implementation. This implementation iterates through
the `upstream-components` directory, which is created through a NPM postinstall
script.

The long-term goal is to simply use the `online_education` base theme to manage
the design system integration, given so much of this application is now almost
identical to the Chaiken Center for Student Success theme. Utilizing this base
theme would allow for a massive reduction in complexity and maintenance costs.

## Application-specific componentry
There are a total of **_160_** custom front-end component libraries specific to
the World Campus theme! Roughly two thirds of these libraries consist of
technical debt that should be reaped in addition to functionality provided
through the `online_education` base theme.

### Template overrides
There are a great number of template overrides, many of which have a
corresponding style library. A number of these templates can likely be removed
with minimal effort. Many exist as stop-gaps during periods of redesigns where
both the old and new versions of certain content needed to be supported
simultaneously.

### Single directory components
While the custom front-end components may partially resemble
[Single Directory Components](https://www.drupal.org/docs/develop/theming-drupal/using-single-directory-components/quickstart)
they actually predate the technology by about 5 years.

**These components should be converted to single directory components!**

## Breakpoint strategy
There are two general breakpoint strategies in use: typical responsive design
breakpoints and a special set of non-overlapping breakpoints that are used for
asset preloading.

### Responsive design breakpoints
These breakpoints correspond to the
[World Campus Design System breakpoints](https://psu-online-education.github.io/?p=viewall-visual-styles-breakpoints).

| Name    | Media query         |
|---------|---------------------|
| default |                     |
| xxs     | (min-width:320px)   |
| xs      | (min-width:550px)   |
| s       | (min-width:800px)   |
| m       | (min-width:950px)   |
| l       | (min-width:1024px)  |
| xl      | (min-width: 1280px) |

These breakpoints are used to optimize raster image asset delivery by keeping
the derivative asset mapped as closely to the viewport dimension as possible.

### Preload breakpoints
These breakpoints closely follow the responsive design breakpoints, but do not
overlap. Preloads operate differently than image source sets and such in that
**all** matching media queries are triggered -- not the most appropriate one.

| Name        | Media query                               |
|-------------|-------------------------------------------|
| xxs_preload | (max-width:549px)                         |
| xs_preload  | (min-width:550px) and (max-width:799px)   |
| s_preload   | (min-width:800px) and (max-width:949px)   |
| m_preload   | (min-width:950px) and (max-width:1023px)  |
| l_preload   | (min-width:1024px) and (max-width:1279px) |
| xl_preload  | (min-width:1280px)                        |

These breakpoints are used in conjunction with the
[Responsive Image Preload](https://www.drupal.org/project/responsive_image_preload)
module in order to take the most aggressive path forward in asset queueing
prioritization.

**Note: Since 2024, all major browsers now support the native
[fetchpriority](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/fetchpriority)
attribute! This should be used instead!**

## Assorted Hooks
There are a number of hooks that power the theme, many of which are the result
of quick-fixes during previous major projects. Many of these hook
implementations can likely be replaced with no-code equivalents.

### `hook_library_info_alter`
This implementation powers the design system integration. See the design system
integration section for details.

### `hook_preprocess_block`
This implementation is more or less a big spaghetti-code structure that targets
various block types and adjusts their variables before rendering. This hook
should be refactored into more focused implementations, likely in dedicated
helper modules with unit testing.

### `hook_preprocess_page`
This implementation does a number of things.

#### Flash message on the "latest version" routes
It is not always obvious if the current page being previewed is currently
published or is a forward revision (awaiting publishing). This hook adds a
helpful message to the latest revision pages.

![An informational message displays "You are currently viewing the latest content version, which is not yet published."]({{ "/assets/documentation/custom-themes/worldcampus/latest-revision-flash-message.png" | relative_url }})

#### Auto-attaches per-bundle node libraries
There are several style libraries that only apply to the canonical
representations of certain content types. This hook automatically attaches a
library of `worldcampus/node--$bundle` to the current page if the route match
has a `node` parameter that is actually a node. This covers the canonical
route, latest revision page, layout builder editing page, etc.

#### Auto-attaches certain taxonomy libraries
The program topics and secondary topics vocabularies have special style
libraries that must apply. Much like the per-node bundle feature, this hook
attaches the appropriate library based on the presence of a term in the route
match of the proper bundle.

**Note: this is impacted by
[Issue #35](https://github.com/psu-online-education/wcprospect8/issues/35).**

#### Admin toolbar displacement
This application generally has a sticky banner that floats along the top of the
viewport. The presence of the admin toolbar must push the sticky banner down by
the height of the toolbar. This hook registers a simple library that adds a
callback to the `Drupal.worldcampus.scrollOffsetCallbacks` array that will
calculate the height of the toolbar and add it to the displacement total.

**Note: this handles the primary and secondary Gin admin theme toolbars.**

#### Single sign on flag to the theming layer
This hook also adds a flag to the page template, `sso_enabled`, which reads
from the `simplesamlphp_auth.settings:activate` configuration. This is used
on the user login page to customize the user experience for both single sign on
and native authentication experiences.

#### Call to action blocks that aren't placed through theme regions
There is a strange design pattern with the sticky banner in that there are
multiple designs for it based on the type of page. This precludes the use
of "global theme regions" for placing various content therein. One of the
things that appear in the **General Sticky Banner** are call-to-action buttons.
The business rule for displaying these call-to-action buttons is that they
should only be rendered if the
`worldcampus_header_cta_large_viewports_how_to_apply` and
`worldcampus_header_cta_large_viewports_request_info` blocks are to be
displayed, respectively. To accomplish this, the settings for these blocks
are copied into variables within the preprocess page hook. This data is then
used to conditionally render the correct call-to-action blocks within the
**General Sticky Banner** (overriding the `placement` tracking option to
`Top Sticky`).

#### `hook_preprocess_views_view`
This hook adds two progressive enhancements to various views.

##### Conditional topic filter on degree level pages
On degree level pages, there exist grids of programs displayed alphabetically.
Some degree levels display _many_ programs, so many in fact that the user
experience recommendation is to provide an additional filtering option for
prospective students on these pages. The logical criteria to provide additional
sub-sorting on is by **Program Topic**. Therefore, if there are programs from
multiple topics displayed in the list, an additional exposed filter by topic is
applied to the view. If the programs displayed are all of a single topic, then
the expectation is that no additional filter is displayed.

![54 Master's Degrees can be sub-sorted by topic.]({{ "/assets/documentation/custom-themes/worldcampus/programs-by-level-view-additional-topic-filter.png" | relative_url }})

##### A legacy design hack on a single page
The main degree and certificate index page has a legacy design artifact that
requires a unique layout section background. This artifact was from an era
when this page (/degrees-and-certificates) was a hard-coded controller route
and not managed via content. When this page was converted to be more content
management friendly, the one feature that could not be provided off-the-shelf
was placing arbitrary images as layout backgrounds. This is troublesome for
accessibility and will **_never_** be a generally supported feature.

To work around this, a CSS override is attached to the view that renders
within the section being targeted. Using a `:has` selector, this image is
applied to the layout background. The image is controlled by IT (as in
committed to version control), and color contrast safety is guaranteed.

### `hook_preprocess_views_view_field`
This hook alters the output of the `topics_with_program_count`,
`undergraduate_levels_with_program_count`, and
`graduate_levels_with_program_count` views. These views display program topics
and levels, respectively. The special business requirement here states that the
program count tagged with each term must be appended to the view field.

Entity queries run to calculate the total programs for each row and render
the result via a stylized
[Parallel Navigation Item](https://psu-online-education.github.io/?p=viewall-molecules-parallel-navigation-item).

### `hook_theme_suggestions_block_alter`
This hook was useful prior to Drupal 10.3. A theme suggestion of
`block__block_content__$bundle` is generated for all block bundles. Drupal 10.3
added a similar theme suggestion hook that adds the
`block__block_content__type__$bundle` theme suggestion.

The recommendation here is that all templates be renamed to use the core
suggestion and then remove the entire hook.

### `hook_page_attachments_alter`
This hook performs three primary tasks.

#### Attaches authenticated component library
The authenticated component library adds a number of enhancements for
authenticated users.

- Greatly enhances the layout builder user experience for the editor persona
- Integrates the design system
[Auto Expansion mechanism](https://psu-online-education.github.io/?p=viewall-utility-auto-expansion)
with [editoria11y](https://www.drupal.org/project/editoria11y).
- Works around admin toolbar font sizing issues due to custom theme root font
scaling

#### Adds client-side markup for the **_Camera_** sprite
This is used by Colorbox JavaScript library overrides to display iconography
within image galleries.

**Note: this should likely be moved to `hook_js_settings_alter` to
conditionally include it on pages based on whether there are photo galleries
in use or not!**

### `hook_views_pre_render`
This hook attaches various component libraries to different views. Each of the
views in question have template overrides that do not have an alternative
mechanism to declare dependencies.

### `hook_js_settings_alter`
This hook conditionally provides server-side rendered markup for the
[Spinner](https://psu-online-education.github.io/?p=viewall-molecules-spinner)
component. The output is made available for the application front-end
JavaScript to use in client-side rendering processes. Presently this is used
in a core library override for the throbber component and also within certain
custom element libraries like email and zip-code inputs.

### `hook_form_views_exposed_form_alter`
This hook implementation specifically targets the SearchStax exposed form in
order to remove the visible title and ensure that an icon with an appropriate
accessible label is added in its place. The design calls for a magnifying glass
icon to be used within the submit button instead of visible text.

### `hook_preprocess_views_view_unformatted`
This hook implementation specifically targets the on demand webinars view. It
conditionally adds an exposed filter by webinar topic if the following criteria
are met:

1. There must be more than a single webinar in the view results
2. There must be more than a single topic shared between all results

The intention is that the filter is only exposed when it is useful and hidden
when it is not.

## Theme suggestions
The World Campus theme adds several strategic theme suggestion hook
implementations that power various functions in a portable way that scales.

**Note: the following hooks exist in the `online_education` base theme and can
be removed from this theme if and when that theme is adopted as a base theme.**

### `hook_theme_suggestions_form_element_alter`
This implementation fills in missing theme suggestions for every form element
type. It also adds more specialized suggestions for the specialized flavors of
Webform file upload elements (audio, document, image, and video).

### `hook_theme_suggestions_input_alter`
This implementation adds theme suggestions for the specific types of inputs.
For example, autocomplete inputs may render additional markup to better
position the AJAX progress throbber UI component.

### `hook_theme_suggestions_menu_alter`
This implementation adds two suggestions based on the theme region that the
menu exists within. This allows the same menu to be dropped into different
regions and render completely differently.

### `hook_theme_suggestions_page_alter`
This implementation adds a theme suggestion for the page based on the current
node type (as determined from the route match service). This allows different
content types to customize the "global theme regions" as necessary.

### `hook_theme_suggestions_taxonomy_term_alter`
This implementation adds view mode specific suggestions for taxonomy terms. It
allows taxonomy terms to be used very similarly to nodes in how they're
represented visually on the front-end.

## Preprocessing
There are a great number of preprocess hook implementations that were ported
to the `online_education` base theme, which is yet to be adopted by this
application. These hooks specify component library dependencies, normalize
various templates for common use, and work around known upstream issues.

### `hook_preprocess_checkboxes`
This implementation adds the missing `form-type-checkboxes` class, which is not
added by Drupal core, and attaches the `form-type-checkboxes` component
library.

### `hook_preprocess_details`
This implementation normalizes the placement of the "required asterisk", which
is a visual feature that denotes the fact that at least one child element
within the details element is required. It also attaches the
`form-type-details` component library.

### `hook_preprocess_fieldset`
This implementation normalizes the placement of the "required asterisk" for
fieldsets in the presence of a "webform help" UI feature by moving the asterisk
before the help tooltip activator. It also attaches the `fieldset` and `legend`
component libraries.

### `hook_preprocess_form`
This implementation attaches the `form` and `form-actions` component libraries.
It also continues to provide a legacy behavior of adding per-form classing and
libraries based on the form ID. This behavior is very seldomly used and should
be removed when the last of the legacy forms are retired.

**Note: the legacy behavior has NOT been ported to the `online_education` base
theme.**

### `hook_preprocess_form_element`
This implementation attaches the `form-item` component library. It also reads
the form element type and adds the `form-type-$type` library (which may not
exist for all form element types).

### `hook_preprocess_datetime_form`
This implementation forces the `#title_display` property for all child elements
to `invisible`. This is both a user-experience and accessibility enhancement
that was identified in the last form styles redesign project.

### `hook_preprocess_form_element_label`
This implementation normalizes the "required asterisk" element placement for
form element labels. It also attaches the `label` component library.

### `hook_preprocess_html`
This implementation exists purely for legacy reasons. It adds a class to the
`<body>` element based on the path alias of the current page. This hook should
be removed for both performance and maintainability reasons.

**Note: this hook was not ported to the `online_education` base theme.**

### hook_preprocess_input
This implementation resolves an upstream bug related to file input element
types. It copies any errors that may be attached to the element to a more
accessible location that can be referenced in the input template. It also
attaches the `input-$type` library, where `$type` can be any valid HTML
type attribute string. `$type` defaults to "text" if unspecified.

### `hook_preprocess_managed_file`
This implementation works around multiple upstream bugs related to managed file
elements. It normalizes element classing where this element does not follow the
precedent of most other element types. It adds support for the `required`
attribute, and provides disambiguity between single and multi-cardinality file
upload elements.

### `hook_preprocess_menu`
This implementation works around a core issue with setting the active trail
properly on the front-page links within menus.

It also provides a **very questionable** workaround for ensuring that success
story nodes have an active trail that leads up through the "about us" and
"success stories" menu items. Given the success stories are **_not_** part of
the menu structure itself, this workaround had to be put in place.

### `hook_preprocess_node`
This implementation attaches a library based on the bundle of the node being
preprocessed. The library attached is `node--$bundle`, which may not exist.

### `hook_preprocess_radios`
This implementation normalizes radio element classing and attaches the
`form-type-radios` library.

### `hook_preprocess_select`
This implementation conditionally attaches one of two component libraries based
on whether the Select2 configuration is set for the element. The `select`
library is attached if the element is a native select and the `select2` library
is attached if the element is a select2 element.

**Note: The select2 upstream library has known accessibility problems and
should no longer be used.**

### `hook_preprocess_textarea`
This implementation attaches the `textarea` component library.

### `hook_preprocess_webform`
This implementation conditionally attaches the `webform-ajax` component library
based on the AJAX setting of the form.

### `hook_preprocess_webform_action`
This implementation normalizes the classing on webform action buttons to match
the design system componentry. It also ensures that automatic color contrasting
support is fully applied to webform actions elements. It also ensures that
intuitive iconography is added to the various button types:

| Button Type      | Icon          |
|------------------|---------------|
| Delete           | Trash         |
| Submit           | Paper plane   |
| Preview next     | Chevron right |
| Wizard next      | Chevron right |
| Preview previous | Chevron left  |
| Wizard previous  | Chevron left  |

### `hook_preprocess_webform_progress`
This implementation conditionally adds the "required asterisk" instructions to
the progress template. This was in response to a design change that required
the indicator to be within the progress area, not underneath it.

For forms that are multistep wizard forms, the progress element title is also
overridden with that of the current step in the process.

### `hook_preprocess_webform_required`
This implementation attaches the `webform-required` component library.

### `hook_preprocess_webform_section`
This implementation normalizes the "required asterisk" element placement for
the webform section element type.
