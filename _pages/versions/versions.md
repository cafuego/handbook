---
permalink: versions/
layout: bootstrap-post
title: "Release Cycles"
section: about
---

# Release Cycles

Backdrop aims to deliver features to end-users more quickly with more frequent ("minor") releases. At the same time, APIs move slowly over longer ("major") release cycles. Backdrop encourages both developers and end-users to upgrade through the features it delivers.

## Version Numbers

Backdrop uses semantic versioning (which is also under consideration for Drupal itself). Take a closer look at version 1.2.4, for example: 

<div class="version-number">
  <span class="major">1</span>.<span class="minor">2</span>.<span class="patch">4</span>
</div>
<div class="version-explaination">
  <span class="major"><h3>Major</h3>Incompatible API changes<br /><small>(3 years)</small></span>
  <span class="minor"><h3>Minor</h3>Backwards-compatible feature and API additions<br /><small>(4 months)</small></span>
  <span class="patch"><h3>Patch</h3>Backwards-compatible bug fixes<br /><small>(as-needed)</small></span>
</div>

## Branch Management

The amount of work can be kept to a minimum by organizing development branches cleverly with Git. Bugs will be fixed in the patch release version first, which is merged into the minor release, which in turn is merged into the major release. This means when fixing bugs, the fix is made to the production version of the software first, and then merged in to the future version. 

<figure>
<img src="/img/release-cycles.png" alt="Release cycle branch management diagram." />
<figcaption>"Current version first" branch management, where features and bug fixes are added to the current ("Primary") version while continuously being merged into the future version. Note that to save space, the "patch" versions for the 2.x branch are not shown.</figcaption>
</figure>

## Timelines

Backdrop aims to provide a new minor release every 3-4 months, providing new functionality and polish at regular intervals. Patch releases will be made as-needed.

Major releases are currently intended to be about every 3 years, but this length of time has not yet been officially established as policy.

## API Compatibility

Backdrop aims to provide API-compatibility even as new features are added within a major version. However, due to the number of publicly available APIs with Backdrop, absolute compatibility is not possible. In this case, API compatibility within a major version means no modifications to the following:

- The signatures of public functions (those not starting with underscores).
- The signatures of protected and public methods on classes and interfaces.
- All hooks and their signatures.
- The HTML markup of front-facing (non-admin) callbacks and templates, and the contents of their variables.
- Form API element properties as specified by their corresponding hook_element_info() implementation.

However the following things *may* be changed, even in a minor release:

- The signatures of private functions (those starting with underscores)
- The signatures of private methods on classes.
- The complete detail of $form (or other renderable) arrays, as exposed to hook_form_alter() implementations.
- Database and configuration schemas.
- Settings available in settings.php.

<small>Portions of this page modified from [Semantic versioning for Drupal 8.x proposal](https://drupal.org/node/586146).</small>
