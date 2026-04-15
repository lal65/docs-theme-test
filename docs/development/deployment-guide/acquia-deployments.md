---
layout: default
subtitle_before: Penn State World Campus
title: Deployment guide
subtitle_after: Acquia deployments
image: inline-assets/development/deployment-guide/acquia-deployments.svg
menu_title: Acquia deployments
menu_order: 1
---
## Deploying code on Acquia
This guide assumes that a pull request has already been accepted and merged
into the `main` branch in GitHub.

### Tagging the release
Deployments on Acquia are based on a code repository in Acquia's
infrastructure. The pull request that was merged in GitHub has been
automatically processed and a release artifact was added to the Acquia remote.

#### Accessing the Acquia Repository
The Acquia repository for the prospect site is
`wcprospect8@svn-23659.prod.hosting.acquia.com:wcprospect8.git` and must be
cloned locally.

```bash
git clone wcprospect8@svn-23659.prod.hosting.acquia.com:wcprospect8.git
```

Once the repository is cloned locally, the `pipelines-build-main` branch will
contain the release artifact code.  The HEAD of `pipelines-build-main` must be
tagged.

```bash
# Check the artifact build branch out
git checkout pipelines-build-main

# Make sure it's completely up to date
git pull origin pipelines-build-main

# Tag it with the current date
git tag PTL-$(date +"%Y%m%d")

# Push the tag up
git push origin PTL-$(date +"%Y%m%d")
```

Note - if more than one release happens per day, the following notation can be
used:
```bash
# More than one release must go out today
git tag PTL-$(date +"%Y%m%d")v2
git push origin PTL-$(date +"%Y%m%d")v2

# Or if it's a really bad day...
git tag PTL-$(date +"%Y%m%d")v3
git push origin PTL-$(date +"%Y%m%d")v3
```
Now that the tag exists on the `pipelines-build-main` branch, the **_actual
contents_** of the changeset between the new tag and the current tag deployed
to production should be **_manually inspected_**.

### Inspecting the release
Say for example, the last release was tagged `PTL-20260401` and today's tag is
`PTL-20260409`. The difference between those tags can be inspected via:
```bash
git diff PTL-20260409 PTL-20260401
```
Some releases may have **_many changed files_**. It is your job to act as the
final line of defense against anything unexpected. If anything doesn't look
right, **_STOP_** and ask questions.

### Performing a test deployment
First, deploy the current production tag to the `test` environment. After the
task completes, copy the files from `prod` to `test`.
After the task completes, copy the database from `prod` to `test`. Finally,
deploy the new tag to the `test` environment.

Carefully watch the task output. If anything doesn't seem right **_STOP_** and
ask questions. Otherwise, proceed.

### Backing up the prod database
Perform a database backup of the `wcprospect8` database on the `prod`
environment and wait for the task to complete. If the task does not complete
successfully, **_STOP_** and file an Acquia support ticket.

### Deploying the tag to production
If all previous steps went smoothly, it's time to deploy for real. Switch the
tag on the `prod` environment to the new tag, preferably having two sets of
eyes on a call.

There have been times when the primary developer loses internet access just
after switching tags. Having a backup online to take over is essential to
preventing a single point of failure.

Carefully monitor the task log until it completes. If it completes
successfully, communicate to the appropriate parties documented in the
communication plans of all tasks that were deployed and begin test plan
execution.

#### Rollback plans exist for a reason
> If something goes wrong **_don't panic_**.  Things have gone wrong before, and we've all lived to tell the tale. It's how senior developers are made. Baptism by fire!


In the event that it is determined that a rollback is necessary, execute the
rollback plan as it's written.  **_Usually_** the rollback plan is simply to
deploy the previous tag, although in some scenarios, additional steps may be
required.
