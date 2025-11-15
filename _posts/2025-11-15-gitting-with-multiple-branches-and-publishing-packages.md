---
layout: post
title: "Gitting with Multiple Branches and Publishing Packages"
date: 2025-11-15
categories: ['dev']
tags: ['git', 'python', 'release-management']
description: "Notes on branch workflows, releases, and GitHub CLI commands when publishing Python packages."
author: Sunil Dhaka
---
#### Intro

Git is a **distributed version control system**—a local tool that tracks code changes, branches, and merges.

GitHub is a **cloud platform built on Git**—it hosts repos and adds collaboration features like PRs, issues, reviews, and CI/CD.

#### Some basics on git:
- git config user name/email
- git init/clone
- create branches (checkout); switch branches
	- delete branches and push delete changes on main/other-head
	- set upstream(publish branch)
- stagechanges, commit with message, push changes upstream (-u/--setupstream)
	- git restore --staged {file-path(s)}" to unstage)
- git status; git diff (--staged)
- stash modified changes when switching branches
	- list, delete, view, pop stashes from the stash stack
- git merge {branch}
	- rebase(linear/clean history): git rebase main
- sync with remote: pull, fetch
- for lived branches after merging, bring up to speed with what main branch has
	- important step in dev/main(prod) branch sort of setups. 
- If your role + branch rules allow bypass, say admin
	- gh pr merge <PR#> --squash/merge/rebase --admin
	- or git merge {branch-name}

##### Tagging and releasing
- tags are essentially annotated commits to reference or **to use as release itself**
- create tags from current or past commit(since tags are created on particular commit)
	- current: git tag -a v1.2.0 -m "Publish tag v1.2.0"
	- past: git tag -a v1.2.0 {commit} -m "...."
	- push: git push origin v1.2.0(--tags to push all local tags)
- **GitHub “Releases” are a GitHub feature.**
	- notes/assets page
	- use UI(via gh API possible but not fun)
#### Some basics for github cli api(gh)
- gh auth login(setup)
	- gh auth status(verify): what protocol(https/ssh), what logging method(exa: keyring)
- gh repo create(create repos): interactive TUI
- gh pr create/status/checkout/merge (PR lifecycle)
- gh issue create/list
- gh run list(list down recent github workflows on repo)
- gh repo view -w (open current repo fast in browser)

#### Rebase, merge, or squash?
- rebase: linear history (most versatile and steeper learning curve)
- squash and merge: merge small branches in one commit(individual commits removed), single merge commits 
- merge is usual stuff; fast-forward possible without merge-commit when the base branch has not diverged(basically base does not have commits that are not there in the feature branch)

![rebase-merge-squash](https://mattrickard.com/static/image/squash-merge-or-rebase/1.webp)
