---
layout: default
subtitle_before: Penn State World Campus
title: Prospect Site
subtitle_after: Deployment Guide
menu_title: Deployment Guide
menu_order: 2
---
## Deploying Code
For the most part, this application is host-agnostic. Deploying enterprise
grade Drupal applications involves a handful of highly structured, repeatable
steps:
1. Enter maintenance mode
2. Flush drupal caches
3. Deploy updated code
4. Run database updates
5. Run configuration import
6. Exit maintenance mode
7. Flush drupal caches
8. Flush external caches

Certain managed hosting providers can make life easier, but at the core of it
all, these steps are all the same. Everywhere. Every time.

### Manual Deployment
Long before CI/CD pipelines roamed the earth, our ancestors bravely deployed
code by hand. Armed with SSH access, a checklist, and a healthy amount of
adrenaline, they would:

1. Carefully pull the latest code onto production
2. Hope no one else was logged in
3. Remember (most of) the steps
4. Pray nothing was cached somewhere unexpected

While manual deployments do work, they come with some well-known risks:

- ☠️ Human error (skipped steps, wrong branch, wrong environment)
- ☠️ Inconsistent release processes between developers
- ☠️ Longer maintenance windows
- ☠️ Hard‑to‑audit changes

That said, if you **_must_** deploy manually, follow the canonical sequence
above exactly, and ensure:

1. Deployments are run by an experienced developer
2. No configuration changes are made directly in production
3. A rollback plan exists before starting

### Acquia Hosting
For the most part, deployments on Acquia hosted environments are **_completely
automated_**.  The **_wcprospect8_** application utilizes a "shared
responsibilities" deployment model that attempts to bring the best of all
worlds to the business.

#### How to deploy applications on Acquia?
Applications are **_always_** backed by a version control system. A deployment
in Acquia is always tied to an individual commit hash. This can either be a tag
or a branch. For non-production environments, deploying either flavor is
permissible, but for production environments, deploying a **_tag_** is highly
recommended because tags are immutable.  Read https://docs.acquia.com/acquia-cloud-platform/working-environments#section-managing-environments
for more in-depth information on **_how_** to perform the various tasks that
follow.

Upon deploying a branch or tag, the
series of 8 steps listed above are performed automatically with no developer
intervention required.

#### Continuous Delivery: Acquia Style
See https://github.com/psu-online-education/acquia-devops-hooks for more
detailed information on exactly how the dev-ops side works.

#### Safety Precautions
There are several best practice safety precautions that should be followed
religiously for production deployments.

1. **_ALWAYS ALWAYS ALWAYS_** take a manual database backup prior to deploying
2. Write down the previously deployed tag
3. Completely understand the rollback and communication plans
4. Perform a mock-deployment to the **_test_** environment immediately
   beforehand - this tests the Acquia orchestration layer (there are semi
   frequently platform level issues that do not appear on the status page!).
5. This is the important one. If something goes wrong, be transparent about it.
   Everyone makes mistakes. There is a level of trust built up with the
   customer here. Every time a mistake happens, it's an opportunity to refine
   processes to safeguard against it happening again.

##### Test Deployment
To perform a test deployment, follow these steps:
1. Deploy the tag that is currently deployed to production to **_test_** (note
   the continuous delivery scripting **_may fail_** when the tag is switched if
   there are changes that are unable to be rolled back, such as database schema
   modifications
2. Copy the files from **_prod_** to **_test_** (note the continuous delivery
   scripting **_may fail_** here when trying to flush the drupal cache)
3. Copy the database from **_prod_** to **_test_** (note the continuous
   delivery scripting **_should always_** succeed here with the same code,
   files, and database as production!)
4. Deploy the **_new tag_** to the test environment and carefully examine the
   deployment task log

### Pantheon Hosting
We can only dream about it 🤤.
