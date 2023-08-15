---
layout: post
title: gh auth login(UI) 
date: 2023-08-15
categories: [git]
tags: [cli, git]
description: set up git globally, to work from terminal
author: Sunil Dhaka
---


To set up `gh` (GitHub CLI) on macOS and use it to create new repositories, add files, and make commits from the terminal, you can follow these steps:

1. Install `gh`:
   - Open the terminal on your macOS.
   - Run the following command to install `gh` using Homebrew:
     ```shell
     brew install gh
     ```

2. Authenticate with GitHub:
   - Run the following command to authenticate `gh` with your GitHub account:
     ```shell
     gh auth login
     ```
   - Follow the prompts to authenticate using your GitHub credentials. This step is necessary for `gh` to interact with your GitHub account.

3. Create a New Repository:
   - To create a new repository from the terminal, navigate to the parent directory where you want the repository to be created.
   - Run the following command:
     ```shell
     gh repo create repo-name
     ```
     Replace `repo-name` with the desired name for your new repository. Follow the prompts to choose the organization, visibility, and other options for the repository.
   - `gh` will create the repository on GitHub and configure the local git repository accordingly.

4. Add and Commit Files:
   - Navigate into the newly created repository directory using `cd repo-name`.
   - Add files to the repository using the standard `git` commands. For example:
     ```shell
     git add file1.txt file2.txt
     ```
   - Commit the changes using the `git` command:
     ```shell
     git commit -m "Initial commit"
     ```
   - If prompted, set your git identity information (name and email) using the `git` command.

With these steps, you can set up `gh` on macOS and use it to create new repositories, add files, and make commits directly from the terminal. `gh` provides a convenient command-line interface to interact with GitHub, making it easier to manage your repositories without leaving the terminal environment.

5. Delete the Repository:
   - Run the following command to delete the repository:
     ```shell
     gh repo delete owner/repo-name
     ```
     Replace `owner` with the username or organization name that owns the repository, and `repo-name` with the name of the repository you want to delete.
   - Confirm by retyping the owner/repo-name

Please exercise caution when deleting repositories, as this action is irreversible. Double-check that you have selected the correct repository before confirming the deletion.
