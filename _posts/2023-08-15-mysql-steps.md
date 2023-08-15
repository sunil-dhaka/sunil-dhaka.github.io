---
layout: post
title: MYSQL setup guide 
date: 2023-08-15
categories: [mysql]
tags: [mysql, cli, database]
description: setting up mysql database in macOS 
author: Sunil Dhaka
---


# Installing MySQL on macOS using Homebrew

MySQL is a popular open-source relational database management system that allows you to store and manage your data efficiently. In this guide, we will walk you through the process of installing MySQL on macOS using Homebrew, a package manager for macOS.

## Prerequisites

Before proceeding with the installation, ensure that you have Homebrew installed on your macOS system. If you don't have Homebrew, you can install it by following the instructions on the [Homebrew website](https://brew.sh/).

## Installing MySQL

To install MySQL using Homebrew, open your terminal and execute the following command:

```bash
brew install mysql
```

This command will fetch the MySQL package from the Homebrew repository and install it on your system.

## Securing Your MySQL Installation

Once the installation is complete, it is highly recommended to secure your MySQL database by setting a root password. To do this, run the following command:

```bash
mysql_secure_installation
```

This command will guide you through a series of prompts to set up the root password, remove anonymous users, disallow remote root login, and more. It is advised to follow the on-screen instructions to secure your MySQL installation properly.

## Connecting to MySQL

To connect to your MySQL server, use the following command:

```bash
mysql -u root -p
```

This command prompts you to enter the root password you set during the secure installation process. After entering the password, you will be connected to the MySQL server.

## Managing the MySQL Service

To start the MySQL service, use the following command:

```bash
brew services start mysql
```

This command starts the MySQL service and ensures that it automatically starts on system boot.

If you wish to stop the MySQL service, you can use the following command:

```bash
brew services stop mysql
```

To check the status of the MySQL service, you can use the following command:

```bash
brew services info mysql
```

This command displays information about the MySQL service, including whether it is currently running or not.

## Running MySQL in the Background

If you prefer to run MySQL in the background without using the Homebrew services, you can execute the following command:

```bash
/opt/homebrew/opt/mysql/bin/mysqld_safe --datadir=/opt/homebrew/var/mysql
```

This command starts the MySQL server in the background. However, if you choose this method, you will need to manage the server manually.

## Conclusion

Congratulations! You have successfully installed MySQL on your macOS system using Homebrew. You have also learned how to secure your installation, connect to the MySQL server, and manage the MySQL service using Homebrew commands. MySQL is a powerful database management system, and now you can start leveraging its capabilities to store and manipulate your data effectively.

Keep exploring and stay curious!

---

*Note: The commands and paths mentioned in this blog are based on the assumption that you have installed Homebrew and MySQL on a standard macOS system. Your specific installation may have different paths or configurations.*

