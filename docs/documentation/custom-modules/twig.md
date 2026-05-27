---
layout: default
subtitle_before: Prospect Site
title: Custom Modules
subtitle_after: Twig
image: inline-assets/documentation/custom-modules/twig.svg
menu_title: Twig
menu_order: 32
---
## Summary
The Twig module adds filters, functions, and other customizations to the Twig
templating language. It also provides a workaround for an unexpected quirk in
how the design system attaches to the CMS.

## Custom function: `autohide_on_missing_id`
The `autohide_on_missing_id` function generates a block of inline CSS that
guarantees zero content layout shifting in situations where the server-side
renderer is not certain whether a piece of content is to be displayed or not.
This CSS is parsed and applied before first paint and takes a target ID as
an argument.

For example, take a call-to-action button that may link to a piece of
personalized content on the page which is conditionally displayed based on a
local storage read. If the personalized content is not displayed, then the
call-to-action should be hidden as well (to prevent a broken link).

The mechanism hinges on the CSS `:has` selector and operates like this:

```css
body:not(:has(#target-id)) [data-autohide-on-missing-id="target-id"] {
  /* hide the dependent element, adjust margins, etc. */
}
```

This is, of course, a simplified example, but the actual CSS handles all things
from hiding the block, to collapsing and effectively hiding the enclosing
layout if the block was the only content present. This function remembers all
IDs that the CSS has been generated for and will only return the inline CSS
once (to prevent front-end bloat).

## Custom filter: `clean_unique_id`
The `clean_unique_id` filter takes an input string and runs it through a
normalizer and guarantees uniqueness. This is an accessibility enhancement.

```twig
<div id="{{ 'my-id'|clean_unique_id }}"></div>
<div id="{{ 'my-id'|clean_unique_id }}"></div>
```

would yield

```html
<div id="my-id"></div>
<div id="my-id--2"></div>
```

## Custom filter: `strip_paragraphs`
This filter was added to work around inherent limitations within the CKEditor 5
WYSIWYG editor. The editor always wraps its markup in some kind of block level
element, which proves exceedingly problematic when attempting to use such
output within certain contexts.

For example, take this use case:

```twig
<h1>{% raw %}{{ some_variable }}{% endraw %}</h1>
```

Without this filter, markup like `<h1><p><em>Hello</em> World</p></h1>` would be
emitted, which is malformed HTML.

Instead, the `strip_paragraphs` filter can be used.

```twig
<h1>{% raw %}{{ some_variable|strip_paragraphs }}{% endraw %}</h1>
```

This produces `<h1><em>Hello</em> World</h1>`, which is well-formed markup.

## Components namespaces alter hook
This module implements `hook_components_namespaces_alter` to ensure that the
`oe` twig namespace is registered. Under certain circumstances, such as when
attempting to use design system artifacts on the admin theme, fatal errors can
occur if the namespace isn't registered. This quirk is a compelling argument
for instead registering components via a module and not via a theme.

## Technical debt
The components namespaces alter hook implementation alludes to a wider
architectural design pattern problem. The design system components are
currently registered by the `worldcampus` theme. Given the general trajectory
of the core Single Directory Components feature and how Drupal Canvas has been
moving, there is mounting evidence to support registering components via module
and not via theme. This is, however, a much larger change than can be
accomplished within the scope of this module.

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
[issue queue](https://github.com/psu-online-education/psu_twig/issues).
