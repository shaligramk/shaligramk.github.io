---
layout: post
title:  "Random git tips"
date:   2023-07-30 23:11:02 +0800
---

### Random Git Tips

This is going to be a small cheatsheet for random `git` commands that I find myself using *most* of the time.


## Rebasing the commits

`git rebase` is [really powerful](http://git-scm.com/docs/git-rebase) however I mostly find myself using it to rebase the commits in order to have a clean history. Here is how you rebase the commits i.e. merge the minor commits into one meaningful commit.

```bash
git rebase -i xxxxx
```

Where `xxxxx` is the hash of the commit immediately below the commit message till which you want to rebase the commits.


## Renaming a branch

You can do the following to rename a branch

```bash
git branch -m old_branch_name new_branch_name
```

Also, if you want to rename the branch which you are currently on, you can do the following

```bash
git branch -m new_branch_name
```

## Detached head problem

Sometimes you might get the problem of detached head. Most common cause for this to occur is, you checkout some specific commit and you start getting this detached head warning since you are not any branch. So how do you solve this?! You simply checkout the branch you were on. For example if you were doing some work upon the `develop` branch, when you started getting this warning, do

```bash
git checkout develop
```

If you forget about the branch you were on, you may simply `checkout` some (any) branch and the problem will be solved.

## Checking the log

This one is pretty straight forward. You simply do:

```bash
git log
```

This will give you the detailed versoin. However, if you just want to have a look at the commit messages (and hashes), simply do the following:

```bash
git log --oneline
```

## Resetting the changes

If you want to get all the staged changes back i.e. revert the `git add .`, you can do the following

```bash
git reset HEAD
```

If you want to revert all the changes since the last commit do the following

```bash
git reset --hard HEAD
```

followed by `git clean -fd` which will remove all the untracked files.

## That SSL Verification Error


Sometimes, you might get SSL certificate error when cloning, pulling or pushing. The simplest way to make it go away is turn off the SSL verification i.e.

```bash
git config --global http.sslVerify false
```
