
# Maintenance of this project

## Syncing with the upstream repository

This is a fork of https://github.com/dpla/platform. Use cherry-picking with Git
to bring in updates from the upstream repository. Do *not* just `git merge`.
This is a fork, with functionality that's incompatible with the upstream.

Example:
```
$ git remote -v
origin  git@github.com:dpla/platform-cqa.git (fetch)
origin  git@github.com:dpla/platform-cqa.git (push)
upstream    git@github.com:dpla/platform.git (fetch)
upstream    git@github.com:dpla/platform.git (push)
$ git fetch upstream
remote: Counting objects: 13, done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 13 (delta 2), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (13/13), done.
From github.com:dpla/platform
   4be8131..43936ef  develop    -> upstream/develop
   7f42529..f48f6a5  master     -> upstream/master
 * [new tag]         v3.2.2     -> v3.2.2
$
$ git cherry -v HEAD upstream/develop
+ 6be23b64ad6d23b008493e56e7646ca963ac71cd Some change that I want
+ 5a08d764da68bcd62da99ef086cc1b3795ce1e29 Some change that I don't want!
$
$ git cherry-pick 6be23b64
```

Though it's been done accidentally before, do not pull in "Prepare Release"
commits from the upstream repository.

## Maintaining Gemfile.lock

When preparing a release, you edit `v1/lib/v1/version.rb` with _this_
application's version, such as: `VERSION = "3.2.1cqa1.0.0"`. Because this Rails
application has an Engine, the engine's version string is recorded in
`Gemfile.lock`.  Edit the `Gemfile.lock` value for `dpla_search_api_v1`.  For
example, say I'm updating `VERSION` as indicated above.  I'd make `Gemfile.lock`
look like this:
```
[...]
PATH
  remote: v1
  specs:
    dpla_search_api_v1 (3.2.1cqa1.0.0)
[...]
```
