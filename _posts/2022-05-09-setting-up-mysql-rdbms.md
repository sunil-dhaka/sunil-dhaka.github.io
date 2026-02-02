---
layout: post
title: Setting Up MySQL on macOS - A Beginner's Guide
date: 2022-05-09
categories: ['dev']
tags: ['mysql']
description: A step-by-step guide to installing and configuring MySQL on macOS
author: Sunil Dhaka
---

# Getting Started with MySQL on macOS

Diving into databases? MySQL is one of the most popular open-source relational database systems out there, and setting it up on macOS is surprisingly straightforward. Let me walk you through the process I use to get MySQL up and running.

## Installing MySQL with Homebrew

The easiest way to install MySQL on macOS is through Homebrew. If you don't have Homebrew installed yet, start with:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Once you have Homebrew ready, installing MySQL takes just one command:

```bash
brew install mysql
```

## Starting MySQL Service

After installation, you'll want to start the MySQL service:

```bash
brew services start mysql
```

This command ensures MySQL runs in the background and automatically starts when you boot your Mac.

## Securing Your MySQL Installation

Fresh installations of MySQL have no root password by default—not exactly secure! Let's fix that by running the security script:

```bash
mysql_secure_installation
```

You'll be guided through several security questions:
- Setting a root password (highly recommended)
- Removing anonymous users
- Disallowing remote root login
- Removing test databases
- Reloading privilege tables

I typically answer "Y" to all these to create a more secure setup.

## Logging In to MySQL

Now you can log in to your MySQL server:

```bash
mysql -u root -p
```

Enter the password you just created in the secure installation step.

## Creating Your First Database

Once you're logged in, let's create a database:

```sql
CREATE DATABASE my_first_db;
```

To verify it was created:

```sql
SHOW DATABASES;
```

You should see your new database in the list.

## Creating a New User

Working as root all the time isn't the best practice. Let's create a regular user:

```sql
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'password';
```

Remember to use a strong password!

## Granting Permissions

Now give your new user some permissions:

```sql
GRANT ALL PRIVILEGES ON my_first_db.* TO 'myuser'@'localhost';
FLUSH PRIVILEGES;
```

## Stopping MySQL Service

When you're done working with MySQL and want to free up resources:

```bash
brew services stop mysql
```

## Troubleshooting Common Issues

### Can't Connect to Server
If you see "Can't connect to local MySQL server through socket," try:
```bash
brew services restart mysql
```

### Reset Root Password
Forgot your password? You'll need to:
1. Stop MySQL: `brew services stop mysql`
2. Start in safe mode: `mysqld_safe --skip-grant-tables`
3. In another terminal: `mysql -u root`
4. Reset the password:
   ```sql
   USE mysql;
   UPDATE user SET authentication_string=PASSWORD('new_password') WHERE User='root';
   FLUSH PRIVILEGES;
   EXIT;
   ```
5. Stop the safe mode instance and restart normally.

Setting up a MySQL database might seem intimidating at first, but once you get the hang of it, you'll find it's a powerful tool for your development projects. What are you building with MySQL?
