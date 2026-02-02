---
layout: post
title: "Keep Your GitHub Garden Green: Automating Contributions"
date: 2023-08-25
categories: ['dev']
tags: ['git']
description: How to automate GitHub contributions to maintain your streak
author: Sunil Dhaka
---

# Never Miss a Day: Automating Your GitHub Contribution Streak

We've all been there—life gets busy, and suddenly you realize you've broken your precious GitHub contribution streak. While consistency is admirable, sometimes automation can help bridge those gaps when you're truly swamped. Here's how I set up a simple system to keep my contribution graph green, even on my busiest days.

## The Ethical Disclaimer

Before diving in, let's address the elephant in the room: this approach is meant as a learning exercise and a safety net for legitimate developers who occasionally miss days—not as a way to fake activity. The most meaningful GitHub profile is one that reflects genuine contributions to projects you care about.

## Setting Up the Automation

We'll use a combination of Git, a simple script, and cron jobs to make this happen:

### Step 1: Create a Dedicated Repository

First, create a repository that will host your automated commits:

```bash
mkdir auto-commit
cd auto-commit
git init
git remote add origin https://github.com/yourusername/auto-commit.git
```

### Step 2: Write the Automation Script

Create a file called `autocommit.sh`:

```bash
#!/bin/bash

# Navigate to your repository
cd /path/to/your/auto-commit

# Create or update a file with the current timestamp
date > autocommit.txt

# Add, commit, and push
git add .
git commit -m "Automatic commit - $(date)"
git push origin main
```

Don't forget to make it executable:

```bash
chmod +x autocommit.sh
```

### Step 3: Schedule with Cron

Open your crontab file:

```bash
crontab -e
```

Add a line to run your script daily at a specific time (for example, 2:30 PM):

```
30 14 * * * /path/to/your/autocommit.sh
```

Save and exit the editor. Your system will now run this script automatically at the scheduled time.

## Making It More Meaningful

If you're going to automate commits, at least make them somewhat useful! Here are some ideas:

- Have the script pull daily weather data and commit it
- Create an automated diary entry prompt
- Track your daily productivity metrics
- Collect news headlines or interesting facts

This way, your automated repository at least serves as an interesting data collection project rather than empty commits.

## Handling Authentication

If you're using a personal access token for authentication, you'll need to include it in your script. The safest approach is to store it as an environment variable rather than hardcoding it:

```bash
git push https://${GITHUB_TOKEN}@github.com/yourusername/auto-commit.git main
```

Then set up the token in your environment before running the script.

## The Better Alternative

While automation is a fun technical exercise, the healthier approach is to plan your GitHub activity around your actual workflow. Some days you might commit dozens of changes, while other days you're planning, learning, or simply taking a break—and that's perfectly okay.

What tools have you built to streamline your development workflow? I'd love to hear about your automation projects in the comments!
