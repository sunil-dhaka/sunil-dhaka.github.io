---
layout: post
title: "MySQL: From Installation to First Query"
date: 2023-08-15
categories: ['dev']
tags: ['mysql']
description: Essential steps for getting started with MySQL
author: Sunil Dhaka
---

# Getting Comfortable with MySQL

Diving into databases for the first time? MySQL is an excellent place to start. I've been using it for several projects lately and wanted to share my streamlined setup process. Let's break it down into manageable steps!

## Installing MySQL

First things first—installation. On macOS, Homebrew makes this incredibly simple:

```bash
brew install mysql
```

For Windows users, the MySQL Installer from the [official website](https://dev.mysql.com/downloads/installer/) works best. Linux users typically use their package manager:

```bash
# Ubuntu/Debian
sudo apt install mysql-server

# CentOS/RHEL
sudo yum install mysql-server
```

## Starting the MySQL Service

Once installed, you'll need to start the MySQL service:

```bash
# macOS
brew services start mysql

# Linux
sudo systemctl start mysql

# Windows
# MySQL typically runs as a service automatically after installation
```

## Securing Your Installation

A fresh MySQL installation comes with default settings that aren't very secure. Let's fix that:

```bash
mysql_secure_installation
```

This interactive script will guide you through:
- Setting a root password
- Removing anonymous users
- Disabling remote root login
- Removing test databases

I recommend answering "Y" to all these prompts for better security.

## Logging In as Root

Now you can log in with your newly set password:

```bash
mysql -u root -p
```

You'll be prompted for the password you just created.

## Creating Your First Database

Once logged in, let's create a database for your project:

```sql
CREATE DATABASE my_project;
```

To start using your new database:

```sql
USE my_project;
```

## Creating a Table

Every database needs tables to store data. Here's a simple example:

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## Adding Some Data

Let's insert a record into our new table:

```sql
INSERT INTO users (username, email) 
VALUES ('john_doe', 'john@example.com');
```

## Running Your First Query

Now you can retrieve your data:

```sql
SELECT * FROM users;
```

You should see the record you just inserted.

## Creating a New User

Working as root all the time isn't ideal. Let's create a dedicated user for your application:

```sql
CREATE USER 'app_user'@'localhost' IDENTIFIED BY 'secure_password';
GRANT ALL PRIVILEGES ON my_project.* TO 'app_user'@'localhost';
FLUSH PRIVILEGES;
```

## Exiting MySQL

When you're done, exit the MySQL shell:

```sql
EXIT;
```

And that's it! You've set up MySQL, created a database with a table, added data, and set up a dedicated user. From here, you can connect your application code or continue exploring more advanced MySQL features.

What are you building with MySQL? Let me know in the comments!

