---
title: "git learning"
layout: post
date: 2021-09-24
categories: git
tags:
  - git start
  - git cli
---

Remember these are according to my understanding,not official or anything. Now, basic thing is to install git on your linux machine using terminal. Github has all nice documentations, try to use only these official sources. I have done this to get my git [CLI](https://github.com/cli/cli/blob/trunk/docs/install_linux.md),
```bash
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo gpg --dearmor -o /usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh
```

Other things like authentication and all can be followed [here](https://docs.github.com/en/get-started/quickstart/set-up-git). Don't forget to authorize your git CLI. I am going to do some clonning and committing in this post for my site.

**Clone:**
```bash
gh repo clone <https://rapo-lin>
```
Change into repo cloned directory. Do anything with any file/folder like: `edit`, `remove`, `whatever`. Now to check repo status, commit with a message and push on remote git server(here it is out online git website).

```bash
git status
git commit -am "Your message about changes that you are going to push"
git push # changes version of project goes online from local(offline) 
```