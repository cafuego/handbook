---
permalink: about/organization/
layout: bootstrap-post
title: "Project Organization"
section: about
---

# Project Organization

Backdrop CMS started its existence as a fork of [Drupal](http://drupal.org). Although Backdrop shares a common codebase with Drupal, its [core philosophies]({{ BASE_PATH }}/about/philosophy/) and organization vary from its parent project. Backdrop practices a policy of focused releases, taking into account the feedback from the community. The overall direction is guided by the Backdrop Project Management Committee (PMC), modeled off of the [Apache project](http://www.apache.org/dev/pmc.html).

## Leadership

The direction of Backdrop is guided by the Backdrop Project Management Committee (PMC). The individuals that make up the committee are often but not necessarily committers to the project code repository. Their mission is to ensure that Backdrop follows the ideals of the project philosophy. If needed, it is also within their power to change the project philosophy, by movement of a unanimous vote. The PMC handles any issue that has been escalated by one or more core committers or any issue at their discretion, at which time they will take a majority vote on the issue at hand.

The PMC currently consists of members **Nate Haug** and **Jen Lampton**.

Additional individuals may be added to the committee by a unanimous vote from the PMC. Individuals may be removed from the committee by a simple majority vote. In the event of a tie within the PMC, an elected member of the PMC may break the tie. The tie-breaker may be elected at any time by a majority vote of the committee.

The currently elected tie-breaker is **Nate Haug**.

## Core Committers

Core committers are responsible for the day-to-day business of reviewing and committing code to the core Backdrop repository. Core committers are enabled to use their descretion when merging pull requests that are in-line with the goals of the project.

Core committers are trusted with upholding the philosophies of the Backdrop project, but are not entitled to set the direction of the project or making sweeping changes to subsystems or modules. If there is a question about a change conflicting with the philosphies of the project, the issue should be escalated to the PMC for a vote.

The current core committer is **Nate Haug**. (Although we only have a single core committer currently, this management structure is intended to have a larger number of committers and a smaller PMC guiding the project; see diagram below.)

## Conflict Resolution

In the event of a question or dispute between core committers, a member of the PMC, or the wider community, an issue may be escalated to the PMC for a vote. The vote may be taken via e-mail, phone call, or other means, as long as all members of the PMC are notified of the arranged time and means of the vote. Any PMC member may report the outcome of the vote, but it is required that at least one member of the PMC post notification to the escalated issue in the public issue tracker.

<figure>
<img src="/img/pmc-structure.png" alt="Diagram of the PMC Structure." />
<figcaption>Diagram showing structure for decision making on issues related to code. Note that the number of members in the PMC and number of committers is for illustrative purposes only.</figcaption>
</figure>

This structure is intended to allow core committers to handle the regular day-to-day work of reviewing and merging code, attempting to avoid a review bottleneck.

## Community Module Authors

Anyone may contribute to Backdrop CMS by writing add-on modules. These modules extend the functionality of Backdrop but are not included directly in the core project. For add-on modules, individual authors may adopt their own management structures. For smaller projects, a single author may be the sole reviewer and committer. Larger modules may decide to take a more collaborative approach where several maintainers share access to a repository. For more information on how contributed modules should register themselves with the central Backdrop repository, see [Writing Modules and Themes]({{ BASE_PATH }}contribute/modules).


