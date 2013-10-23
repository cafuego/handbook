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

**Things to avoid in pull requests:**

- An excessive number of commits.
- Debugging or test commits.
- Poorly formatted commit messages, missing "Issue #xx: " prefix.

<figure>
<img src="/img/sad-pull-request.png" width="810" height="635" />
<figcaption>A pull request that has multiple poorly formatted commits. This pull request would not be suitable for merging into the project.</figcaption>
</figure>

If you've made a pull request that needs cleaning up, it's easy to solve this problem by deleting the pull request, fixing the commits locally, and then making a new pull request. Let's assume you had filed a pull request on the branch with the name `my_branch`, you could clean up it's commits with the following commands:

```
# Delete the remote branch (this will automatically close related pull requests).
git push origin :my_pull_request_branch

# Edit the last 4 commits together with an interactive rebase.
git rebase -i Head~5
```

This will open your default text editor with an interface like this:

```text
pick f6dcf28 blah
pick 0ee28af Issue #77: Removing further files[] instances from .info files.
pick 22ade13 issue 77: Removing registry building from SimpleTest.
pick 9e33534 debugging
pick c5f376b 77 Fixing incorrect path to filetransfer classes

# Rebase 1e5974e..c5f376b onto 1e5974e
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x, exec = run command (the rest of the line) using shell
#
# If you remove a line here THAT COMMIT WILL BE LOST.
# However, if you remove everything, the rebase will be aborted
```

To combine these commits into a single commit, change the word "pick" to "fixup" (or just "f" for short). At the same time, you may change the overall commit message by modifying the first commit and changing it to "reword" (or "r" for short). You may leave all the lines that start with a hash sign, they won't affect your rebase.

```
reword f6dcf28 Issue #77: Replacing the registry with a more reliable alternative.
f 0ee28af Issue #77: Removing further files[] instances from .info files.
f 22ade13 issue 77: Removing registry building from SimpleTest.
f 9e33534 debugging
f c5f376b 77 Fixing incorrect path to filetransfer classes
```

This will take all 5 commits and combine them into a single commit with a clean message. If you make any mistakes, just close your editor and use `git rebase --abort`` to cancel the rebasing.

Now with all the commits nicely merged together, you can push up your branch to Github a second time, and file the pull request again with the new, cleaner commit messages.

```
git push origin my_branch
```

<figure>
<img src="/img/happy-pull-request.png" width="814" height="524" />
<figcaption>A cleaned-up pull request after merging the commits together.</figcaption>
</figure>
