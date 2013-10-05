---
permalink: code/pull-requests/
layout: "bootstrap-post"
title: "Installation Instructions for Backdrop"
section: dev
---

#Submitting Pull Requests to Backdrop CMS

All development on Backdrop core happens on Github by pull requests. This involves:

- Forking the repository on Github into your own account.
- Cloning the repository to your own computer.
- Modifying the code.
- Committing and pushing the code to your repository.
- Making a pull request back to the original project.

A maintainer or fellow contributor should then review the changes and ensure tests pass. If everything looks good, the code will be merged into the main project by a committer.

This video covers all of these above concepts in a step-by-step walkthrough.

<div class="video-container">
<iframe width="640" height="480" src="//www.youtube.com/embed/HcD9f7o34Gw?rel=0&amp;vq=hd1080" frameborder="0" allowfullscreen></iframe>
</div>

A summary of things to keep in mind:

- Include full sentence in your commit message, with the format `Issue #xx: Description of the problem solved.`
- Always find or file an issue in the [Backdrop Issue Tracker](http://github.com/backdrop/backdrop-issues/) before making a pull request.
- Always cross-link the pull request and the matching issue.

##Trouble with Tests?

If you're encountering regular failures when submitting pull requests, you can use multiple approaches to resolve the test failures without needing to make pull requests.

- **Run the tests locally**

  You can run tests locally by enabling the "SimpleTest" module and then visiting `admin/config/development/testing` and running the individual test that failed.

- **Run the tests using the shell script**

  If you're running all the tests, you may benifit from using the shell script version of the test suite, which can run faster by executing tests in parallel. To do this, enable the SimpleTest module on your site, and then using a command line at the root of your Backdrop installation, run this command:

  `php ./core/scripts/run-tests.sh --php ``which php`` --concurrency 8 --url 'http://localhost/' --all`

  Note you'll need to replace "localhost" with the URL of your installation.

- **Enable Travis CI on your Github Repository**

  Travis CI can run the test suite for you without needing to file a pull request, simply [sign into Travis CI](https://travis-ci.org/auth) (via OAuth using your Github credentials), visit [your profile on Travis CI](https://travis-ci.org/profile), and enable testing on your fork of Backdrop. Now Travis CI will test every commit you make to your repository!

##Making Clean Pull Requests

If you make a lot of commits to your repository branch while trying to fix tests or solve other problems, it's a good idea to clean up your commits before filing a pull request. Because your commit history will be merged directly into the parent project, each commit message should be clear about its purpose. Having complete commit messages makes examining the history with the `git log` or `git blame` commands easier.



