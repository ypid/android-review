<!--
SPDX-FileCopyrightText: 2021 Robin Schneider <ypid@riseup.net>

SPDX-License-Identifier: CC-BY-SA-4.0
-->

# Android review

**The solution put together here is deprecated. Use git range-diff. See https://grapheneos.org/faq#audit This was somehow missed when research was done how to diff rebases.**

This repository represents an independent ongoing code review of Android Open
Source Project (AOSP) or custom ROMs that are based on it.
ypid sees this review effort as part of a community working on AOSP based
hardening. See:

* https://grapheneos.org
* https://github.com/AOSPAlliance/README

## GrapheneOS

https://grapheneos.org/ is currently the main subject under review.

## Why this repo?

Google itself uses Gerrit for code review and development with their own
workflow.

* Most custom ROMs use a `git rebase` based development model where they
  maintain a patchset on top of their upstream (mostly AOSP). This makes
  reviews difficult however because there is no native tooling to do reviews
  across rebases. At least that is the conclusion of ypid.
* The official Gerrit instance is not interesting here as ypid wants to only
  review code that is released.
* The review effort should be cryptographically verifiable.

So Gerrit seems not ideal for those requirements.

## I want to have a quick look what changed. How?

First of all, when you don’t generate the git log contained in this repo
yourself you need to trust me that I do it properly. With that out of the way,
you have two options:

### Web based (limited)

Check out the [git tags of this
repo](https://github.com/ypid/android-review/releases). For now, only
GrapheneOS is reviewed so you can click on a tag and then the git commit
referred to by the tag. It will show you the diff to the previous git tag
contained in this repo. I might not push/review all releases.

### CLI based (advanced)

This has the advantage that we can normally skip the boring stuff and only show
real file changes.

You can than review the changes across rebases using something like this:

```Shell
git diff RQ1A.210105.002.2021.01.05.03..RQ1A.210205.004.2021.02.02.09 -I '^(commit|index) '
```

Note that this requires git 2.30.0 which was released 2021-01.

## How does the review work?

ypid put together existing building blocks with as little as possible glue code
to achieve the goal. Basically it boils down to:

```Shell
git log --patch
```

The main focus is on being easily reviewable in an editor like Vim. There is
another more common form of distributing patches `git format-patch`, it is not
sure yet if ypid might switch to that.

`git format-patch` pro:

* Can be reapplied.
* Commonly known, see https://www.kernel.org/doc/html/v4.17/process/submitting-patches.html
* ~~Patches could be annotated with review notes in a file `*.notes`.~~ Would not survive renames of patches.

`git log --patch` pro:

* One big file that is easy to review online and in a local editor regardless of skill level. This is the main reason.
* Date is ISO 8601 compliant.

The file `additional_repo_path.list` lists additional repository paths that are
not present in AOSP. It is not sure yet if references to them should also be
included here using git submodules. We will see how my workflow evolves.

For details, see:

* https://github.com/hashbang/aosp-build
* https://github.com/hashbang/os

## Feedback

If you have feedback or contributions how to improve this review, please let me
know.

## License

* [./GrapheneOS/](/GrapheneOS/): Please refer to https://grapheneos.org/faq#copyright-and-licensing.
* Documentation: CC-BY-SA-4.0
