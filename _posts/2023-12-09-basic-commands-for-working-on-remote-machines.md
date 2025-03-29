---
layout: post
title: "Remote Machine Mastery: Essential Terminal Commands" 
date: 2023-12-09
categories: ['dev']
tags: ['linux']
description: A practical guide to working efficiently with remote machines
author: Sunil Dhaka
---

# Taming Remote Machines: Commands I Can't Live Without

Working with remote servers and machines is part of everyday life for many developers. Whether you're managing cloud instances, accessing university computers, or working with remote databases, knowing your way around the command line can save hours of frustration. Here are the essential commands I rely on daily.

## Connecting to Remote Machines

Let's start with the basics—getting connected:

```bash
# Simple SSH connection
ssh username@hostname

# SSH with a specific port
ssh -p 2222 username@hostname

# SSH with private key
ssh -i /path/to/private_key username@hostname
```

Pro tip: Create SSH config files to avoid typing these commands repeatedly. In your `~/.ssh/config`:

```
Host myserver
    HostName server.example.com
    User myusername
    Port 2222
    IdentityFile ~/.ssh/special_key
```

After this, simply type `ssh myserver` to connect with all those parameters.

## File Transfers Made Simple

Moving files between local and remote machines is a common need:

```bash
# Copy from local to remote
scp localfile.txt username@hostname:/remote/path/

# Copy from remote to local
scp username@hostname:/remote/path/file.txt local_directory/

# Copy entire directories (recursive)
scp -r local_directory username@hostname:/remote/path/
```

For larger or more complex transfers, I prefer `rsync` for its ability to resume interrupted transfers:

```bash
# Sync local to remote with progress bar
rsync -avP local_directory/ username@hostname:/remote/path/

# Sync remote to local
rsync -avP username@hostname:/remote/path/ local_directory/
```

## Remote Process Management

Need to run long processes that continue after you disconnect? Here's how:

```bash
# Keep processes running after logout
nohup long_running_command > output.log 2>&1 &

# Check running background processes
jobs

# Bring a background process to foreground
fg %job_number
```

For a more robust solution, I use `tmux` or `screen`:

```bash
# Start a new tmux session
tmux

# Detach from session (Ctrl+B then D)
# Later, reattach with:
tmux attach
```

## Monitoring System Resources

Understanding what's happening on a remote machine is crucial:

```bash
# Check system load and CPU usage
top
# or my personal favorite
htop

# Disk space usage
df -h

# Memory usage
free -m

# Check specific processes
ps aux | grep process_name
```

## Network Diagnostics

When things aren't working, network tools help identify issues:

```bash
# Test connectivity
ping hostname

# Trace network path
traceroute hostname

# Check if a port is open and accessible
nc -zv hostname port

# View open connections
netstat -tuln
```

## Quick File Edits

Need to make quick changes to configuration files? Nano is beginner-friendly:

```bash
nano filename
```

For more power with a steeper learning curve, vim is my go-to:

```bash
vim filename
```

Vim cheat sheet: press `i` to enter insert mode, make your changes, then press `Esc` followed by `:wq` to save and quit.

## Troubleshooting with Logs

When something goes wrong, logs are your best friend:

```bash
# View end of log file
tail -f /var/log/syslog

# Search logs for errors
grep -i error /var/log/application.log
```

These commands have saved me countless hours when working with remote systems. What are your essential remote work commands? Any clever shortcuts I've missed?
