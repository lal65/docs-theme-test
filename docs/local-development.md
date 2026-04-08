---
layout: default
subtitle_before: Penn State World Campus
title: Prospect Site
subtitle_after: Local development
image: assets/custom-modules.svg
menu_title: Local development
menu_order: 1
---
## Local Development Guide
There are three things that every local CMS environment needs: code, files, and
a database. Drupal is no exception.

## Prerequisites
There are a few prerequisite setup tasks that must be performed prior to
getting started with local development.

### Local Environment
Docker is highly recommended. There are utilities baked into a custom local
image that automate several common tasks.

### Code
All development happens in GitHub.

#### Accessing GitHub
In order to access the code for the prospect site, one has to be a member of
the Penn State GitHub Enterprise. For more information on getting started with
Penn State GitHub, see the
[Frequently Asked Questions](https://pennstate.service-now.com/sp?id=kb_article_view&sysparm_article=KB0019927&sys_kb_id=f9f60290c3cc0b5ca9562130a001313e&spa=1)
knowledge base article.

### Database
In order to access a development database for the prospect site, one has to
be a member of the Penn State Acquia account. For help with getting started
with Penn State Acquia email
[support@outreach.psu.edu](mailto:support@outreach.psu.edu).

After gaining access to the platform, one has to set up the Acquia Cloud API.
For more information on setting this up, see the
[Acquia Cloud Platform API Overview](https://docs.acquia.com/acquia-cloud-platform/help/94146-cloud-platform-api-overview)
documentation page.

### Files
In order to access a copy of the development files for the prospect site, one
has to have a working SSH key whose public certificate is allow-listed by
the Acquia Platform.  See the
[Generating an SSH Public Key](https://docs.acquia.com/acquia-cloud-platform/generating-ssh-public-key)
guide for more information.

#### SSH Key Best Practices
**_ALWAYS_** use a passphrase with SSH keys. Using a passphrase grants a grace
period for the security office to revoke access in the event of a private key
compromise.

## Getting Started
The first step in getting started is cloning the repository.

```bash
git clone git@github.com:PSU-OOE/wcprospect8.git
```

After cloning the repository, 