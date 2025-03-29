---
title: "Git learning"
layout: post
date: 2021-09-24
categories: ['dev']
tags: ['git']
author: Sunil Dhaka
---

I've been diving into Git lately, and thought I'd share some notes to help myself (and maybe you!) remember the key concepts. While you can certainly work on projects locally without GitHub, the platform offers so many productivity tools for free—who wouldn't want to save time and focus on what truly matters?

## Getting Started with Git CLI

GitHub's documentation is excellent, but here's the quick version of how I installed the Git CLI on my Linux machine:

```bash
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo gpg --dearmor -o /usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh
```

For authentication and setup details, check out the [official guide](https://docs.github.com/en/get-started/quickstart/set-up-git). Don't forget to authorize your Git CLI!

## Cloning and Committing

Cloning a repository is straightforward:

```bash
gh repo clone <https://rapo-lin>
```

After making changes to files, you'll want to commit them:

```bash
git status                           # See what's changed
git add <name-of-new-files-created>  # Stage new files
git commit -am "Your message"        # Commit changes with a descriptive message
git push                             # Push changes to the remote repository
```

When you clone a repo, it automatically connects to your remote account, making the push process seamless.

## Creating a New Repository from CLI

Creating a new repo from scratch is as simple as:

```bash
gh repo create /name of repo/  # Follow the prompts to complete setup
```

You can also initialize Git in an existing project folder:

```bash
cd ~/project-directory
git init
```

This creates a hidden `.git` directory to track your revisions. Don't worry—running `git init` repeatedly won't override previous settings.

## Managing Files and Folders

Removing files works similarly to bash commands:

```bash
git rm file-name               # Remove from both local directory and git tracking
git rm -r file-or-folder       # Remove recursively
git rm --cached                # Remove from git tracking but keep the local file
```

Extra careful with `rm`—it's powerful! If you've accidentally committed sensitive information, you can remove it from the entire Git history:

```bash
git filter-branch --force --index-filter --prune-empty "git rm --cached --ignore-unmatch <path_to_file>" HEAD
```

## Working with Branches

Creating a branch locally and pushing it to remote:

```bash
git branch name-of-branch                         # Create a local branch
git push --set-upstream origin name-of-branch     # Set up tracking with remote
```

To work with a branch created on GitHub:

```bash
git branch --list              # List all branches
git fetch origin               # Get updates from remote
git checkout branch-name       # Switch to the branch
```

When you're ready to commit changes to your branch:

```bash
git checkout branch-to-switch-to    # Switch to your branch
# Make your changes
git status                          # Check what's new
git add new-files                   # Stage new files
git commit -am "Your changes"       # Commit all changes
git push                            # Push to your branch
```

## Merging Branches

To merge your changes into the main branch:

```bash
git checkout main              # Switch to receiving branch
git merge branch-to-merge      # Merge your changes
```

Things get more interesting when you have conflicting files between branches. When that happens, I've found the [Atlassian guide on merge conflicts](https://www.atlassian.com/git/tutorials/using-branches/merge-conflicts) extremely helpful.