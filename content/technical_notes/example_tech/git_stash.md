---
date: "2020-12-23T00:00:00+01:00"
draft: false
linktitle: Git Stash
menu:
  example_tech:
    parent: Git & GitHub
    weight: 1
title: Git Stash vs Git Commit
toc: true
type: docs
weight: 1
---

## Understanding Git Stash vs Git Commit

**Context**: You're on a feature branch and you want to switch to another branch, but there could be conflicts between a file that was edited on a current branch and the destination branch you're switching *into*. 

This only happens because git tries to preserve your unstashed/unstaged changes across branches. 

Your choices are to `commit` or `stash`. 

In the future, you can run:

`git restore name-of-conflicting-fie`

and if its a file that *should be tracked*, you can run:

`git commit` (but no need to run `git push`)

OR

`git stash` to save the changes on name-of-conflicting-fie for future retrieval. When you need to apply the changes, you can run:

`git stash apply`

**Background**: 

`git commit` adds a commit to your local environment. Local is just for you - no one else can see that you've added a commit. They can only see that you've added a commit once you push the commit to a repo, which is what happened when you ran: 
`git push origin branch`. 

`git push` takes your local commit and adds the new commit to the remote repo referenced by origin and the branch specified in `branch`. 





