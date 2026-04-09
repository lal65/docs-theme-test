---
layout: default
subtitle_before: Penn State World Campus
title: Prospect Site
subtitle_after: Processes
image: assets/processes.svg
menu_title: Processes
menu_order: 1
---
## Governance and Processes
The prospect site is a very mature application and has greatly benefitted from
over a decade of lessons learned. Nearly all maintenance, support, and new
feature development is guided by a series of processes and a strong governance
model, however due to recent organizational changes, a lot of this perfect
world is now in a state of confusion.

### Governance
Prior to the reorganization of both IT and Marketing, the governing body of all
application changes consisted of representatives from all primary partners and
stakeholder groups.

#### Director of User Experience and Insights
The director of web strategy is the senior that presides over governance and is
the escalation path for times when decisions need to be made. When things need
to get done, this is the contact.

- Dave Housley <ddh16@psu.edu>, Director of User Experience and Insights

#### Accessibility and Design
As the primary designer and the canonical source of truth for brand experience
this role has the final say on any end-user facing feature in terms of
presentation and function.

- Matt Ryan <mqr5815@psu.edu>, Assistant Director of User Experience and Insights

#### Content Management
The content managers are the primary staff users of the application.

- Denny Connolly <djc5014@psu.edu>, Director of Platform Strategy and Operations
- Emily Ansley <eca5091@psu.edu>, Platform Strategy and Operations Manager
- Melanie Swiftney <mzs6920@psu.edu>, Platform Strategy and Operations Manager

#### SEO / SEM
The SEO / SEM managers govern the search engine optimization and search engine
marketing aspect of the application. They utilize the application to fine tune
content metainformation to outperform competitors and ensure that relevant
content is always easily accessible to end-users.

- Levi Bloom <ldb21@psu.edu>, Assistant Director of SEO Strategy

#### CRM
The CRM team is a close partner due to the integrations into the application.

- Fred Peggs <fup4@psu.edu> Business Systems Manager
- Matt Cali <mjc5610@psu.edu> CRM Analyst
- Ganira Atash <gva5280@psu.edu> Applications Developer

#### Analytics
The analytics team ensures that marketing has a clear picture of how their
dollars spent are performing.

- Wendel Hullihen <wmh5034@psu.edu>, Business Intelligence Specialist
- Nora Price <nqp5361@psu.edu>, Business Intelligence Specialist

#### Applications Development
The applications developers manages the software and architecture to meet
the needs of all partners and stakeholders. They work closely with each group
to understand the business needs and translate those needs to actionable tasks.

This group in particular is now in a state of relative confusion given there
have been no new processes defined for how to interact with the Marketing
organization following the reporting line changes.

- Matthew David Webb <mdw15@psu.edu>, Applications Developer Manager
- Brianne Williams <bnh10@psu.edu>, Applications Developer
- Kyle Leber <kjl16@psu.edu>, Applications Developer
- Luke Leber <lal65@psu.edu>, Applications Developer
- Zachary Ishler <zri5004@psu.edu>, Applications Developer

### Processes
Despite organizational upheaval, several processes continue survive.

#### Applications Developer Peer Review
GitHub is the platform where all applications developer peer review happens.
Pull requests are required for any changes to the application and may not be
bypassed. This ensures that at least 2 Applications Developers are always
involved in any change. Without successfully obtaining an approval on the pull
request, it is **_impossible_** to merge a change based on repository settings.

#### Automated Tests **Must** Pass
For a change to be considered for production release, all automated tests must
pass. This includes PHPUnit based code tests, behat tests, and ghost inspector
tests.

PHPUnit tests run automatically on all pull requests and must pass in order to
unlock the merge button.

Behat tests can only run locally due to the expensive nature of running Chrome
in actions. Refer to the
[Automated Testing Guide]({{ "development/automated-testing.html#behat-testing" | relative_url }})
for running behat tests.

Ghost inspector tests should run through the dedicated GitHub action. These
tests require a pair of hosting environments to be set up with a stable
baseline and the release candidate. Refer to the
[Automated Testing Guide]({{ "development/automated-testing.html#ghost-inspector-testing" | relative_url }})
for running ghost inspector tests.

Only after all of these tests complete is a changeset eligible for production
release.

#### Change Management
The Change Management process remains intact for the most part. **_At least_**
one day prior to a release, a notification must be posted to the
[CRM SEO Testing / Change Management](https://teams.microsoft.com/l/chat/19:24498018f87842eb8db358e15ff8098e@thread.v2/conversations?context=%7B%22contextType%22%3A%22chat%22%7D)
Teams channel, which still has representation from all major governance actors.

Each change **_must_** be itemized and provide:
1. A short summary of the change
2. A longer description of the change (if applicable)
3. A link to the Workfront task associated with the change

Anyone in that channel can push back on any change for any reason. Significant
changes that are not adequately summarized in a teams message may have a more
formal meeting scheduled to interactively walk through the change as a group.
