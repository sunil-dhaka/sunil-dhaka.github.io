---
layout: post
title: automate false commits on github 
date: 2023-08-15
categories: [git]
tags: [cli, git]
description: false commits check for automation on gcp vm
author: Sunil Dhaka
---

Title: How to Automate Dummy Commits on GitHub Using Crontab and a Virtual Machine

Introduction:
Automating dummy commits on GitHub is a useful way to simulate commit activity for testing, learning, or experimentation purposes. Utilizing crontab to schedule commits and a virtual machine (VM) for isolation allows you to conveniently manage and automate this process. In this blog post, we'll guide you through the steps of setting up a VM, creating a script for dummy commits, using crontab to schedule the script's execution, and using the GitHub CLI to initialize your GitHub credentials.

## Prerequisites:
1. Virtualization software (e.g., VirtualBox or VMware)
2. Basic knowledge of Git and GitHub
3. GitHub CLI (`gh`) installed on the virtual machine

## Steps to Automate Dummy Commits on GitHub Using Crontab:

### Step 1: Set Up a Virtual Machine
1. Install your preferred virtualization software.
2. Create a new virtual machine and install your chosen operating system (e.g., Ubuntu).
3. Set up networking to ensure the VM can access the internet.

### Step 2: Install GitHub CLI and Initialize Authentication
1. Open a terminal in your VM.
2. Install the GitHub CLI using the package manager:
   ```
   sudo apt-get install gh
   ```
3. Initialize GitHub CLI and follow the prompts to authenticate with your GitHub account:
   ```
   gh auth login
   ```

### Step 3: Create a Script for Dummy Commits
1. Create a shell script named `commit.sh` in your VM using a text editor.
2. Use the following code to generate dummy commits and update a text file:

   ```bash
   #!/bin/bash

   commit_count=$((1 + RANDOM % 8))

   for ((i = 1; i <= commit_count; i++)); do
       timestamp=$(date +%s)
       random_number=$((1000 + RANDOM % 9000))
       echo "$timestamp: $random_number" >> tmst.txt
       git add tmst.txt
       git commit -m "Dummy commit #$i"
   done

   git push origin main
   ```

3. Make the script executable:
   ```
   chmod +x commit.sh
   ```

### Step 4: Schedule Script Execution with Crontab
1. Open the crontab editor using:
   ```
   crontab -e
   ```
2. Add the following line to run the script every day at a specific time (e.g., 00:00 midnight):
   ```
   0 0 * * * /path/to/commit.sh
   ```
   Replace `/path/to` with the actual path to the script.

3. Save and exit the crontab editor.

### Step 5: Observe Automated Commits
1. Wait for the scheduled time, and the script will be executed automatically by crontab.
2. Visit your GitHub repository to see the generated dummy commits.

## Conclusion:
Automating dummy commits on GitHub using crontab and a virtual machine simplifies the process of simulating commit activity for various purposes. This approach allows you to experiment with Git workflows, version control, and commit scenarios while maintaining a separate and controlled environment. By following the steps provided in this blog post, you can gain valuable experience in automation, version control, and GitHub CLI usage, making it an ideal solution for those seeking to learn, test, or contribute to repositories without impacting their primary development setup.
