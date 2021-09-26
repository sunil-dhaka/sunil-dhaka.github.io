---
title: "Git learning"
layout: post
date: 2021-09-24
categories: ['git']
tags: ['git']
author: Sunil Dhaka
---

Remember these are according to my understanding,not official or anything. Now, basic thing is that you can work on a project locally without using `github`, but git provides many productive and useful tools for `free` and `easy`, and who won't want to save their time and be more productive on what really matters. I think that is one of the reason of `github` being so popular. [Github](https://github.com/) has all things stated nice on its website. To install git on your linux machine using terminal. Github has all nice documentations, try to use only these official sources. I have done this to get my git [CLI](https://github.com/cli/cli/blob/trunk/docs/install_linux.md),
```bash
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo gpg --dearmor -o /usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh
```

Other things like authentication and all can be followed [here](https://docs.github.com/en/get-started/quickstart/set-up-git). Don't forget to authorize your git CLI. I am going to do some clonning and committing in this post for my site.

**`Clone and Commit:`**
```bash
gh repo clone <https://rapo-lin>
```
Change into repo cloned directory. Do anything with any file/folder like: `edit`, `remove`, `whatever`. Now to check repo status, commit with a message and push on remote git server(here it is out online git website).

```bash
git status
git add <name-of-new-files-created>
git commit -am "Your message about changes that you are going to push"
git push # changes version of project goes online from local(offline) 
```

**`Make a git repo from CLI:`**

Using command line interface is what I like when creating a new repo from scratch you just need to give one line and git automatically asks you some questions and creates your remote repo as well as clones it locally. We can also initiate a new repo from our exisiting project folder.
````bash
```
gh repo create /name of repo/ # creates a new repo and then follow the prompt #
```
cd ~/move to the project subdirectory
git init
```
````
What git `init` does is it creates a hidden directory called `.git` and now we can start recording our revision of project. Even if you repeat git `init` on already git repo it won't over-ride previous init 👾. That is cool. To learn more about any command use `--help` manuals.

**`Delete files and folders from CLI:`**

To remove it is just same as bash commands. Without `--cached` rm removes file from local directory and then we have to commit and push to remote one. With `--cached` we can remove remote file from git repo and keep local file, to avoid checking that file in future git status runs, we can add that file's path into `.gitignore`. As always when using `rm` be extra careful.
```bash
# go to the directory 
git rm file-name
git rm -r file or folder
git rm --cached 
```
When you have pushed a file that contains sensetive info like passwords etc, we might want to remove that file from entire git histoty. To remove a file from entire git history use below command. To learn specific operations meaning use `--help`. Official documents sort out things most of the times.
```bash
git filter-branch --force --index-filter --prune-empty "git rm --cached --ignore-unmatch <path_to_file>" HEAD
```

**`Create new branch:`**
It is easy. You can create it first locally and then set an upstream branch by,
```bash
git branch `name-of-branch`
git push --set-upstream origin `name-of-branch`
```
Other way around, is even simpler: first create a new branch on github website repo and then make it local,
```bash
git branch --list # lists all branches

```
