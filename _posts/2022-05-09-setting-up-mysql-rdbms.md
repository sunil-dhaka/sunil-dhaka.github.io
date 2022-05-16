---
title: "setting up mysql rdbms"
layout: post
permalink: setting-up-mysql-rdbms
date:   2022-05-09
categories: ['mysql','sql']
tags: ['mysql','sql']
author: Sunil Dhaka
---
Here is helpful tips when you are working with mysql in linux. 
- installing mysql in ubuntu
```bash
sudo apt install mysql-server
```
- install sequre mysql server installation: to prop it with user and passwd
```bash
sudo mysql_secure_installation
```
above prompts to set password for server, and then prompts for bunch of other question, just press yes
	- Remove anonymous users? (Press y/Y for Yes, any other key for No) : y
	- Disallow root login remotely? (Press y/Y for Yes, any other key for No) : y
	- Remove test database and access to it? (Press y/Y for Yes, any other key for No) : y
	- Reload privilege tables now? (Press y/Y for Yes, any other key for No) : y

- start server and retriev queries
```bash
mysql -u root -p
# this asks for password and all set
```
---
- how to load csv files into tables
	- first create table with cols and their appropriate types that are in csv file
	- now to load data we do this
```sql
LOAD DATA LOCAL INFILE 'path_to_csv_file' INTO TABLE table_name FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n' IGNORE 1 ROWS;
```
	- but if things are not set correctly, it will throw previlage errors
	- one needs to allow local file load permissions to both cliend and server sides
	- to do that set global local_infile = 1 and when starting new sql session strt it with --local-infile=1 option 
	- one can also enable local-inflie in mysql config file 
