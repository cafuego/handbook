---
permalink: versions/roadmap/
layout: bootstrap-post
title: "Backdrop CMS Roadmap"
---

# Backdrop CMS Roadmap

Backdrop is currently working on its initial release. This release focuses on delivering key functionality from Drupal 8 without the massive architectural changes.

## Goals for the 1.0 release

  1. **Appeal to existing Drupal 6 and 7 Developers**: We will deliver modern features (configuration management, content listings, rich-text editing, etc.) on a traditional Drupal-based architecture.
  2. **Maintain an Easy-to-understand Architecture**: We will make sure that new developers can understand the basics of the Backdrop architecture (info files, hooks, and callbacks) with minimal effort. An hour should be enough to summarize the architecture.
  3. **Keep APIs Stable ("Major" versions)**: We will make the upgrade process easier on end-users by maintaining slower-moving core APIs.
  4. **Deliver New Features Often ("Minor" versions)**: We will deliver new features at regular intervals with minimal risk of API breakage.
  5. **Ensure Great Performance**: We will focus on direct implementations and speed rather than accounting for edge-cases through abstraction.
  6. **Provide a Better End-User Experience**: We will constantly be improving the editorial and site-building experience. The intial release aims to improve content creation (matching Drupal 8's advancements), and deliver a universal layout manager for site-builders (replacing blocks).
  7. **Grow the community**: We will focus on attracting new developers at all points of the spectrum, but especially remembering the entry-level developer.

## Key Features for 1.0

As Backdrop aims to backport functionality from Drupal 8 for its initial release, many of the target features for Backdrop 1.0 should look familiar to those following Drupal 8 development:

  1. Configuration Management (CMI)
  2. Built-in Views Module
  3. Built-in WYSIWYG Support
  4. Improve the Editorial Experience
  5. Improve Mobile Support
  6. HTML5 Markup and Fields
  7. Revamped Block System and Layout Support
  8. Improve Performance over Drupal 7
  9. Reduce theme system complexity
  10. Improve Multilingual Support

## Exclusions from 1.0

However, with Backdrop's different target audience; shorter release cycle; and approach to implementation, we are have some notable exceptions on what is planned for the first version in comparison to Drupal 8. Features that are **not planned** at this point include:

  1. Built-in Web Services
  2. Typed data or "EntityNG"
  3. Converting all template files to Twig
  4. Symfony-framework based HTTP Kernel
  5. Dependency Injection
  6. PHPUnit testing (we still have SimpleTest Unit testing however)

## Backport List

The full list of backports and their statuses are listed in Backdrop Issue Tracker at https://github.com/backdrop/backdrop-issues/issues/106.
