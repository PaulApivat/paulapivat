---
date: "2021-07-16T00:00:00+01:00"
draft: false
linktitle: Making a Pull Request
menu:
  example_tech:
    parent: Git & GitHub
    weight: 1
title: Making a Pull Request on a GitHub Repo
toc: true
type: docs
weight: 1
---

## Guide for contributing to another code base by making a pull request

## Public Repo

1. Identify a project you want to contribute to on GitHub
2. Fork that project
3. Clone it to your local machine*

**note** : Do not clone in another directory that was *already* cloned from another repo. For example, when I cloned [pytalentsolution](https://github.com/PaulApivat/pytalentsolution) on my local machine, I initially cloned it *inside* Saku directory, which was *itself* a clone of an already-existing-repo. The correct way was to create a *new* directory for pytalentsolution.

4. Make a new branch

- 4a: `cd` into directory of the cloned repo
- 4b: use `git branch` to confirm you're on the `*master` branch
- 4c: use `git checkout -b name_of_new_branch` 
- 4d: use `git branch` to check that you're on `name_of_new_branch`

5. Make changes

6. Push it back to your repo

**note** : You'll get a message `git push remote --` use this instead of the traditional `git push`

7. (Go back to the forked repo in GitHub) Click the **Compare & Pull Request** button.

- add a description to changes made in the pull request 

8. Click **Create pull request** to open a new pull request

## Private Repo

1-3. You cannot fork a private repo

4. Clone directly to local machine

5. `git branch -a` to see all branches in the project

6. `git branch` to confirm you're on master branch

6. [important] `git status` to check if ahead or behind (if behind, use `git pull` to fetch and download content from a remote repository)

7. [from git master] create a **new** branch use `git checkout -b name_of_new_branch`

8. Use `git branch` to make sure you're on new branch

9. Make changes, then normal: git add, git commit -m "message", git push 

**note** : You'll get a message `git push remote --` use this instead of the traditional `git push`

10. (Go back to the forked repo in GitHub) Click the **Compare & Pull Request** button.

- Add good PR description, [see here](https://www.pullrequest.com/blog/writing-a-great-pull-request-description/)

11. Click **Create pull request** to open a new pull request

## Production vs Development

Ongoing projects with several contributors will generally separate the **main** from **develop** branch. Making a PR in this context is *slighty* different:

1. In the command line, switch to development branch `git checkout develop` (even if you don't see the `develop` branch locally, you may just see `main` or a new branch you created `new_branch`).

2. Now that you're *starting* on `develop` branch, do `git pull origin develop` to make sure that any prior changes from the `develop` branch is pulled in locally, and you're up-to-date. 

3. Then from `develop`, create a new `feature` branch like so: `git checkout feature/new-branch`. (note: `feature/new-branch` is a naming convention that explicitly says you're creating a new feature branch).

4. Then make your changes or add new code. 

5. Then, `git push origin` (in this case, origin will be develop)

6. Once you go back to github, if you see `compare and pull request`, make sure it is being merged into `develop` and **not** `main`. 

## When Feature branch goes out of sync with Development

Sometimes you've already pushed a pull request, but the development branch goes ahead of your proposed changes. Here's how to handle:

1. Switch to a specific feature branch (no need to go back to develop): `git checkout branch_name`
2. Run `git pull origin develop`
3. Then `git push origin`

3a. If there's an `Index Error`, run
- `git reset --hard` *
- `git clean -df`

* `git reset` is to take the current branch and reset it to point somewhere else, also bringing the index and working tree along. Here's a more visual explanation ([source](https://stackoverflow.com/questions/2530060/in-plain-english-what-does-git-reset-d))

If your main branch is `C` and you want to point your current branch somewhere else:
```
- A - B - C (HEAD, main branch)
```
and you want to point to `B`, not `C`, then you use `git reset B` to move it there:

```
- A - B (HEAD, main branch)  # - C is still here, but there's no branch pointing to it anymore
```

## VIM

If for whatever reason you find yourself on VIM, you can escape by:

1. Press `esc` (escape)
2. Press `:` (colon)
3. Press `wq` (write and quit)

Or one-line command to get out of VIM

4. Press `:wq!` (colon, write and quit)




