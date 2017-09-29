# postgres

This repository represents the version of [postgres](https://www.postgresql.org/)
that is used as part of the [Manta
project](https://github.com/joyent/manta) in the
[manatee](https://github.com/joyent/manta-manatee) project.

We have changes to postgres currently covering the following broader
features:

* Ensure ISM (Intimate Shared Memory) is taken advantage of on systems that
  support it (i.e. illumos distributions and Solaris)

## Repository Management

This repository is downstream of the [github postgres
mirror](https://github.com/postgres/postgres).

To better understand and maintain our differences from postgres, we try to
manage branches and tags in a specific fashion. First and foremost, all
branches and tags from the upstream postgres repository are mirrored here.

Anything that is Joyent-specific begins with a `joyent/` prefix.

Branches with Joyent modifications are named `joyent/<version>`, such as
`joyent/9.6.3`. This is a branch that starts from the postgres
`REL9_6_3` tag. These branches will have all of our patches
rebased on top of them. Currently, this repository is consumed by
`manta-manatee`, which contains a git submodule for this repository. That
submodule will point to a tag in this repository that uses the form
`joyent/v<version>j<branch release num>`. The first release as described
above would be: `joyent/v9.6.3j1`. If we need to cut another release
from this branch, we would tag it `joyent/v9.6.3j2` and continue to
increment the number after the `j`. Note we use the `j` instead of `r`
which would more traditionally be used to indicate a revision.  We use
`j` in case postgres for some reason wants to use it in its version strings
for whatever reason.

When it comes time to update to a newer version of postgres, we would take
the following steps:

* Ensure that we have pushed all changes from `postgres/postgres` and synced
  all of our branches and tags.
* Identify the release tag that corresponds to the point release. For
  this example, we'll say that's `release-9.6.4`.
* Create a new branch named `joyent/<version>` from the tag. In this
  case we would name the branch `joyent/9.6.4`.
* Rebase all of our patches on to that new branch, removing any patches
  that are no longer necessary.
* Test the new postgres binary.
* Review and Commit all relevant changes.
* Create a new tag `joyent/v9.6.4j1`.
* Update the [manta-manatee](https://github.com/joyent/manta-manatee)
  submodule to point to the new tag.

