---
layout: post
title: basic commands when working on remote machines 
date: 2023-12-09
categories: [cli]
tags: [cli, ssh, scp]
description: basic knowledge on ssh, scp, sftp, rsync; some of the useful commands when working on remote machines from local 
author: Sunil Dhaka
---

### SSH (Secure Shell):

**Usage:**
- Connect to a remote server: `ssh username@remote_server`
- Specify port: `ssh -p port_number username@remote_server`
- Generate SSH key: `ssh-keygen`
- Copy public key to server: `ssh-copy-id username@remote_server`

### SCP (Secure Copy):

**Usage:**
- Copy local file to remote server: `scp local_file.txt username@remote_server:/path/to/destination`
- Copy remote file to local machine: `scp username@remote_server:/path/to/remote_file.txt /local/destination`

### SFTP (Secure File Transfer Protocol):

**Usage:**
- Open SFTP session: `sftp username@remote_server`
- Upload local file to server: `put local_file.txt`
- Download remote file to local machine: `get remote_file.txt`
- Navigate directories: `cd`, `ls`, `pwd`

### Rsync (Remote Sync):

**Usage:**
- Sync local and remote directories: `rsync -avz /local/path/ username@remote_server:/remote/path/`
- Exclude files or directories: `rsync -avz --exclude='file.txt' /local/path/ username@remote_server:/remote/path/`
- Update only changed files: `rsync -avzu /local/path/ username@remote_server:/remote/path/`
