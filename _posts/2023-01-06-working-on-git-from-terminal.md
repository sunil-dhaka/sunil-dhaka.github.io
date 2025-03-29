---
title:  "Git Magic from the Terminal"
layout: post
permalink: working with git from terminal
date:   2023-01-06
categories: ['dev']
tags: ['git']
author: Sunil Dhaka
---

Ever get tired of switching between your code editor and GitHub's web interface? The GitHub CLI tool (gh) is a game-changer for managing repositories directly from your terminal. Here's how I set it up and use it daily.

## Quick Setup

First, install the GitHub CLI on your system. On Ubuntu/Debian:

```bash
sudo apt install gh
```

For other systems, check out the [official installation guide](https://github.com/cli/cli#installation).

## Authentication Made Simple

Before you can use gh, you'll need to authenticate with your GitHub account:

```bash
gh auth login
```

Just follow the prompts—you can choose between password-based authentication or using a personal access token (which you'd generate on GitHub's website). The guided process makes it surprisingly painless.

## What Can You Do?

Once authenticated, you've unlocked a whole world of GitHub functionality right from your terminal. You can:

- Create new repositories
- Clone existing ones
- Create and manage issues
- Review and merge pull requests
- Manage releases
- And much more

No more context-switching between terminal and browser—everything's at your fingertips!

Get started exploring what's possible with `gh help` or check out the [official documentation](https://cli.github.com/manual/).

It's amazing how much more productive I've become since adding this tool to my workflow. What terminal tools have revolutionized your development process?
