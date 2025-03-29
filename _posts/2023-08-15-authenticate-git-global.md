---
layout: post
title: Supercharge Your Terminal With GitHub CLI
date: 2023-08-15
categories: ['dev']
tags: ['git']
description: Set up git globally to work seamlessly from your terminal
author: Sunil Dhaka
---

# Unlocking GitHub's Full Power in Your Terminal

Want to manage GitHub repositories without constantly switching to your browser? The GitHub CLI (`gh`) is a game-changer for developers who live in the terminal. Here's my quick guide to getting it set up on macOS.

## Installing GitHub CLI

Getting started is simple with Homebrew:

```shell
brew install gh
```

Just like that, you've got GitHub's power at your fingertips.

## Authentication: The One-Time Setup

Before jumping in, you'll need to authenticate with your GitHub account:

```shell
gh auth login
```

Follow the interactive prompts, and you'll have options to authenticate via your browser or a personal access token. The guided process makes this surprisingly painless—no more wrestling with SSH keys or remembering complex commands.

## Creating New Repositories in Seconds

Once authenticated, creating a new repository becomes as simple as:

```shell
gh repo create repo-name
```

The CLI will walk you through choosing the organization, visibility settings, and other options. What used to take multiple clicks and page loads now happens in seconds without leaving your terminal.

## Working with Your New Repository

After creating your repository, you can:

1. Navigate into the repo directory: `cd repo-name`
2. Add files with standard git commands: `git add file1.txt file2.txt`
3. Make your first commit: `git commit -m "Initial commit"`

If this is your first time using Git, you might be prompted to set your identity information with:

```shell
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

And that's it! With GitHub CLI, I've cut down the time I spend managing repositories by at least half. How do you optimize your Git workflow?
