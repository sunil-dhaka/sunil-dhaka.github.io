---
layout: post
title: setting up nginx server on linux machine(Ubuntu 22.04)
date: 2023-12-11
author: Sunil Dhaka
categories: [cloud]
tags: [cli, cloud]
description: simple script that can be executed on a server machine easily
---

There is only one thing to keep in mind. I resolved it after 3-4 months, though I was not actively looking for the solution. Just set up OpenSSH/ssh via ufw by allowing them. Below script explains the thing. Enjoy!

```bash
#!/bin/bash

# Update sudo packages
sudo apt update

# Install Nginx with user confirmation
sudo apt install nginx -y

# Allow Nginx HTTP via ufw
sudo ufw allow 'Nginx HTTP'

# Allow OpenSSH for remote server access
# Note: Allowing OpenSSH is necessary for remote server administration.
# https://serverfault.com/questions/1099888/ec2-instance-ssh-accessnot-able-to-connect-after-the-nginx-installation
# or either allow ssh :: sudo ufw allow ssh
# https://stackoverflow.com/questions/55022757/unable-to-reach-ec2-instance-after-setting-up-nginx
sudo ufw allow OpenSSH

# Enable and start ufw
sudo ufw enable
sudo systemctl start ufw

# Start Nginx process
sudo systemctl start nginx -y

# Display status
sudo systemctl status nginx

echo "Nginx installation and setup completed successfully."

# this same issue happened with the previous aws ec2 instance, when i installed nginx and setup after following digital ocean article
# I got that connection timeout error, because I logged my self out without any ssh entry since I did not allow OpenSSH or ssh 
# you there make sure to keep that open, alright?
```

Bonus: a layman analogy for web servers.

```
Imagine Nginx as a traffic cop for websites. When you visit a website, Nginx helps manage the flow of information between your browser and the website's server. It directs and organizes this traffic, ensuring a smooth and efficient interaction. Nginx is like a multitasking traffic cop; it handles multiple requests simultaneously, efficiently managing the traffic, and ensures that each request reaches its destination without causing congestion. It's particularly good at handling lots of people (or requests) at the same time, making sure everyone gets where they need to go quickly and without any hiccups. So, when you access a website, Nginx is the behind-the-scenes hero making sure everything runs smoothly on the internet highway.
```
