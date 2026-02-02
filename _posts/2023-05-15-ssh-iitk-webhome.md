---
title: "Connecting to IITK Webhome via SSH"
layout: post
permalink: ssh-iitk-webhome
date: 2023-05-15
categories: ['life']
tags: ['tech']
author: Sunil Dhaka
---

# Seamlessly Connect to Your IITK Web Home

Ever needed to update your IITK personal webpage but found the process clunky? Using SSH to connect directly to your web home space can save you tons of time. Here's how I set it up, with a few tricks I've learned along the way.

## The Basics: Direct SSH Connection

The straightforward way to connect is with the standard SSH command:

```bash
ssh username@webhome.iitk.ac.in
```

Replace `username` with your IITK username, enter your password when prompted, and you're in! From here, you can navigate to your web home directory:

```bash
cd /webhome/username
```

## Creating a Smoother Connection with SSH Config

Tired of typing that long command and your password every time? Here's my solution:

1. Create an SSH config file if you don't already have one:

```bash
touch ~/.ssh/config
chmod 600 ~/.ssh/config  # Set proper permissions
```

2. Add your IITK webhome configuration:

```
# IITK webhome connection
Host iitk-webhome
    HostName webhome.iitk.ac.in
    User your_username
    IdentityFile ~/.ssh/id_rsa_iitk  # Optional if using key-based auth
```

3. Now you can simply type:

```bash
ssh iitk-webhome
```

## Setting Up Password-less Login (Optional)

If you're like me and connect frequently, setting up key-based authentication can save you from typing your password every time:

1. Generate an SSH key pair (if you don't already have one):

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa_iitk
```

2. Copy your public key to the webhome server:

```bash
ssh-copy-id -i ~/.ssh/id_rsa_iitk.pub username@webhome.iitk.ac.in
```

Enter your password one last time, and future connections should be password-free!

## Transferring Files to Your Web Home

Need to upload files to your personal webpage? SCP makes it easy:

```bash
# Using the direct connection
scp your_local_file.html username@webhome.iitk.ac.in:/webhome/username/

# Or if you've set up the SSH config:
scp your_local_file.html iitk-webhome:/webhome/your_username/
```

## Useful Commands Once Connected

Once you're in, here are some handy commands for managing your web home:

```bash
# Check your storage quota
quota -s

# Set file permissions for a webpage (public readable)
chmod 644 yourfile.html

# Set directory permissions (navigable)
chmod 755 yourdirectory
```

That's it! With this setup, managing your IITK web space becomes much more convenient. No more struggling with FTP clients or cumbersome web interfaces. Any questions about optimizing your SSH workflow? Feel free to reach out!
