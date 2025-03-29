---
layout: post
title: "Targeted Git Access: Connecting to Specific Repositories"
date: 2023-08-15
categories: ['dev']
tags: ['git']
description: How to set up CLI access to specific Git repositories
author: Sunil Dhaka
---

# Access Just What You Need: Repository-Specific Git Setup

Sometimes you need access to just one repository without setting up your entire Git environment. Maybe you're working on a friend's project, contributing to open source temporarily, or accessing a work repo from a personal device. Whatever the reason, here's my streamlined approach for repository-specific Git access.

## Step 1: Generate a Personal Access Token

First, you'll need a personal access token (PAT) from GitHub:

1. Go to GitHub → Settings → Developer settings → Personal access tokens
2. Click "Generate new token"
3. Give your token a descriptive name
4. Select appropriate permissions (for read-only access, just "repo" is sufficient)
5. Set an expiration date based on how long you need access
6. Click "Generate token"
7. **Important:** Copy the token immediately—you won't see it again!

## Step 2: Clone with Your Token

Now you can clone the repository using your token for authentication:

```bash
git clone https://{YOUR_TOKEN}@github.com/username/repository.git
```

Replace `{YOUR_TOKEN}` with your actual token, and adjust the repository path accordingly.

## Step 3: Configure Repository-Specific Credentials

To ensure your credentials are only used for this specific repository:

```bash
cd repository
git config credential.helper store
git pull
```

When prompted for credentials, enter your GitHub username and use the token as your password. These credentials will only be associated with this repository.

## Step 4: Verify Your Setup

To check that everything's working properly:

```bash
git fetch
```

If you don't get any authentication errors, you're all set!

## Security Considerations

A few important notes about this approach:

- **Token storage**: Your token is stored in plain text in `.git-credentials`. Ensure your system is secure.
- **Limited scope**: Always set the minimum permissions necessary for your token.
- **Expiration**: Use short-lived tokens when possible, especially on shared systems.
- **Revocation**: If you suspect your token has been compromised, revoke it immediately on GitHub.

This approach has saved me tons of time when I need quick access to specific repositories without configuring global Git settings. Have you found other clever ways to manage repository-specific access? I'd love to hear about them!
