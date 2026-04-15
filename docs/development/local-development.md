---
layout: default
subtitle_before: Penn State World Campus
title: Development
subtitle_after: Local development
image: inline-assets/development/local-development.svg
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
[support@outreach.psu.edu](mailto:support@outreach.psu.edu) or open a
ServiceNow Request with the following fields:

| SNOW Field           | Value                 |
|----------------------|-----------------------|
| **Service Offering** | OOE: Drupal on Acquia |
| **Assignment Group** | OOE-SYSOPS            |

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

### Getting the code

```bash
git clone git@github.com:PSU-OOE/wcprospect8.git
```

### Running Docker

After cloning the repository, the application can be started up in Docker by
running the bootstrapping utility script.

```bash
./scripts/start.sh
```

The first time starting the docker container up, it is normal to receive an
error message containing:

> Command pm:list was not found. Drush was unable to query the database. As a result, many commands are unavailable. Re-run your command with --debug to see relevant log messages.


This happens because there is presently no database set up.  Ultimately the
following message will be presented.

```bash
  This container is running on port 8080

  Set VCS user to 'Luke Leber <lal65@psu.edu>'

  To refresh files, run 'refresh-files'.
  To refresh data, run 'refresh-database'.
    An optional environment argument can be provided.
    IE. 'refresh-files dev && refresh-database dev'

  To fire phpunit tests, run 'run-phpunit <group>'
  To fire behat tests, run 'run-behat <tag>'

  To install PHP dependencies, run 'composer install'
  To install NPM dependencies, run 'npm install'

  To rebuild theme CSS and JS, run 'npm run build'

  To flush the drupal cache, run 'drush cr'

  Access the site on your host at https://local.drupal.psu.edu:8080
```

### Installing dependencies
Now that there's a shell inside docker, dependencies can be installed.

#### Composer
Composer is the dependency manager for PHP.

```bash
composer install
```

#### NPM
NPM is the dependency manager for NodeJS.

Note - it is always more secure to run `npm ci` than `npm install`!
```bash
npm ci
```

### Refreshing files
Once dependencies are installed, the files can be fetched.

```bash
refresh-files
```
yielding
```bash
www-data@19431e5cf954:~/html$ refresh-files
You are now connected to Acquia Cloud
 Session: 191bb06a-3dc0-4a1b-926f-45718d896e13
Enter passphrase for key '/var/www/.ssh/id_rsa': 
     26,863,197   0%   49.51kB/s    0:08:49 (xfr#6840, to-chk=0/51719)
```

### Refreshing database
Similarly, the database can be fetched and imported.

```bash
refresh-database
```
yielding
```bash
www-data@5e3271e79b84:~/html$ refresh-database
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 88.4M  100 88.4M    0     0  1419k      0  0:01:03  0:01:03 --:--:-- 1216k
88.5MiB 0:01:17 [1.14MiB/s] [================================================>] 100%            

Initializing local environment... [success] Cache rebuild complete.
 [success] No pending updates.
+------------+---------------------------------------+-----------+
| Collection | Config                                | Operation |
+------------+---------------------------------------+-----------+
|            | core.extension                        | Update    |
|            | psu_webform_readonly.settings         | Update    |
|            | simple_sitemap.settings               | Update    |
|            | system.logging                        | Update    |
|            | block.block.worldcampus_salesforcemfw | Update    |
|            | simplesamlphp_auth.settings           | Update    |
+------------+---------------------------------------+-----------+
 [notice] Synchronized extensions: install purge_ui.
 [notice] Synchronized extensions: install views_ui.
 [notice] Synchronized extensions: install restui.
 [notice] Synchronized configuration: update psu_webform_readonly.settings.
 [notice] Synchronized configuration: update simple_sitemap.settings.
 [notice] Synchronized configuration: update system.logging.
 [notice] Synchronized configuration: update block.block.worldcampus_salesforcemfw.
 [notice] Synchronized configuration: update simplesamlphp_auth.settings.
 [notice] Finalizing configuration synchronization.
 [success] The configuration was imported successfully.
 [success] Unblocked user(s): psuomc
 [success] Changed password for psuomc.


Log in with user 'psuomc' and password 'psu1855'
```

### Restarting docker
With the dependencies installed, files fetched, and database loaded, the last
step is to restart docker.  To restart docker, simple `exit` out of the shell
and re-run `./scripts/start.sh`.

This time, instead of an error message, a set of development credentials will
be fetched. A fully successful startup should look like this:

```bash
lleber@lleber:~/projects/wcprospect8$ ./scripts/start.sh 
latest: Pulling from drupal-local-dev
Digest: sha256:a17eb1125b0704fb293b412a31754eb50040aef591d57617f8a0ce21e7f2a190
Status: Image is up to date for 140860689751.dkr.ecr.us-east-1.amazonaws.com/drupal-local-dev:latest
140860689751.dkr.ecr.us-east-1.amazonaws.com/drupal-local-dev:latest
Downloading and installing node v20.20.2...
Downloading https://nodejs.org/dist/v20.20.2/node-v20.20.2-linux-x64.tar.xz...
############################################################################################ 100.0%
Computing checksum with sha256sum
Checksums matched!
Now using node v20.20.2 (npm v10.8.2)
Creating default alias: default -> v20 (-> v20.20.2)
Now using node v20.20.2 (npm v10.8.2)
Warning: Permanently added 'wcprospect8dev.ssh.prod.acquia-sites.com' (ED25519) to the list of known hosts.
You are now connected to Acquia Cloud
 Session: 2698e925-1d5f-4fa9-985b-b7d6c91a2395
Enter passphrase for key '/var/www/.ssh/id_rsa': 
            767 100%   16.28kB/s    0:00:00 (xfr#13, to-chk=0/15)

  This container is running on port 8080

  Set VCS user to 'Luke Leber <lal65@psu.edu>'

  To refresh files, run 'refresh-files'.
  To refresh data, run 'refresh-database'.
    An optional environment argument can be provided.
    IE. 'refresh-files dev && refresh-database dev'

  To fire phpunit tests, run 'run-phpunit <group>'
  To fire behat tests, run 'run-behat <tag>'

  To install PHP dependencies, run 'composer install'
  To install NPM dependencies, run 'npm install'

  To rebuild theme CSS and JS, run 'npm run build'

  To flush the drupal cache, run 'drush cr'

  Access the site on your host at https://local.drupal.psu.edu:8080
```
