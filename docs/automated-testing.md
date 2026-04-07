---
layout: default
subtitle_before: Penn State World Campus
title: Prospect Site
subtitle_after: Automated Testing
menu_title: Automated Testing
menu_order: 1
---
> Test automation is the use of software (separate from the software being tested) for controlling the execution of tests and comparing actual outcome with predicted.

&nbsp;

## Testing Strategy
The testing strategy for the prospects application is multi-faceted.

<details>
  <summary>Unit testing</summary>
  <div markdown="1">

### Unit testing
Unit tests are the lowest level tests that exist for this application. They are
based on the [PHPUnit](https://phpunit.de/index.html) framework and are highly
Drupal-specific. These tests are run automatically on pull requests and may be
triggered on-demand locally. There are several tiers of unit tests.

#### Unit Tests
Unit tests focus on isolated PHP classes or functions and validate behavior
without relying on Drupal services, the database, or the filesystem.
Dependencies are typically mocked or stubbed so that only the logic under test
is executed. These tests are:

1. Extremely fast to run
2. Easy to write and maintain
3. Ideal for validating business logic, utility classes, and edge cases

Unit tests form the foundation of the test pyramid and provide immediate
feedback during development.
Use unit tests when:

1. Testing pure logic or calculations
2. Verifying behavior of a single class or method
3. You want maximum speed and minimal setup

#### Kernel tests
Kernel tests boot a minimal Drupal kernel, allowing interaction with real
services, configuration, and the database while still avoiding a full HTTP
request lifecycle. They provide a middle ground between unit and functional
tests by validating:

1. Entity definitions and persistence
2. Service wiring and event subscribers
3. Hooks and plugin discovery
4. Configuration and schema behavior

Kernel tests are more expensive than unit tests but are still relatively
fast and deterministic. Use kernel tests when:

1. You need real Drupal services or entities
2. Database interaction is required
3. Full page rendering is not necessary

#### Functional tests
Functional tests (sometimes called browser tests) execute the application
through a simulated HTTP request using Drupal’s internal browser. They verify
complete request/response behavior and page output without real JavaScript
execution. These tests validate:

1. Routes and controllers
2. Permissions and access control
3. Forms and submissions
4. Rendered HTML responses

Because they require a full Drupal installation, functional tests are slower
and more resource‑intensive than kernel tests. Use functional tests when:

1. Testing user-facing features without JavaScript
2. Verifying access rules and workflows
3. Ensuring responses match expected output

#### Functional Javascript tests
Functional JavaScript tests extend functional testing by using a real browser
environment (via WebDriver) to execute JavaScript. These tests simulate true
user interaction and validate client‑side behavior. They are capable of
testing:

1. AJAX-driven interfaces
2. Dynamic form updates
3. Front‑end frameworks and behaviors
4. Complex user workflows

These tests are the most expensive to run and maintain, but they provide the
highest fidelity to real user behavior. Use functional JavaScript tests when:

1. JavaScript is critical to functionality
2. Asynchronous behavior must be validated
3. End‑to‑end confidence is required

#### Running PHPUnit tests locally
To run phpunit tests locally, the `run-phpunit` helper script can be used.

```bash
# Run phpunit tests for the "bls" module
run-phpunit @bls
```

</div></details>
<details>
  <summary>Behavioral testing</summary>
  <div markdown="1">

### Behavioral testing
Behavioral tests target the overall product as the end-user perceives it. Test
cases are often centered around high value functionality. Behavioral testing is
provided by two distinct services: [Behat](https://behat.org/) and
[Ghost Inspector](https://ghostinspector.com/).

#### Behat testing
Behat is used when authenticated access to the application back-end is
required. We were unable to acquire authorization for the Ghost Inspector
platform to authenticate this application.

Due to the expensive nature of running behat tests, these tests only run on
local development environments.  To execute behat tests locally:

```bash
# Run all behat tests (could take hours!)
run-behat

# Run a single suite
run-behat @homepage

# Run multiple suites
run-behat "@one-or-more&&@tags-to-run&@@tests-for"
```

#### Ghost inspector testing
Ghost inspector behavioral tests can be run two ways: through the Ghost
Inspector dashboard and through GitHub Actions. Typically, the Ghost
Inspector dashboard is used when setting tests up. After initial set up the
tests are committed to the application repository under
`tests/ghostinspector/functional-suites` or `tests/ghostinspector/data-suites`
depending on the nature of the test case.

</div></details>
<details>
  <summary>Visual testing</summary>
  <div markdown="1">
### Visual testing
Visual tests target the actual appearance of the application front-end. Visual
testing is provided by the [Ghost Inspector](https://ghostinspector.com/)
platform.

The visual testing strategy operates on a **_stateless model_** where there are
two application environments configured side-by-side with different code and/or
configuration revisions deployed to each.  The pseudo-flow goes:

```php
$baseline_env = 'https://{{ baseline_env }}.worldcampus.psu.edu';
$test_env = 'https://{{ test_env }}.worldcampus.psu.edu';
foreach ($pages_to_test as $page) {
  $screenshot_a = get_screenshot($baseline_env . '/' . $page);
  $screenshot_b = get_screenshot($test_env . '/' . $page);
  if ($screenshot_a !== $screenshot_b) {
    // Fail.
  }
}
```

The baseline and test environments are configurable via a GitHub action.

<details>
  <summary>Compatible Environments</summary>
  <div markdown="1">
The environments that maybe selected are:
- test
- dev
- dev1
- dev2
- dev3
- dev4
- dev5
- dev6
- dev7
- dev8
- dev9

</div></details>
</div></details>