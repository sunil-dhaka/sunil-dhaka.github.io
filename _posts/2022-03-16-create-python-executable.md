---
title:  "make python scripts executable"
layout: post
permalink: make-python-scripts-executable
date:   2022-03-16
categories: ['python']
tags: ['python']
author: Sunil Dhaka
---
To make a python script executable there are simple three steps:
- add shebang line
	- in first line of .py script add this line
```bash
#!usr/bin/env python
...
python code
...
```
- allow execute mode
	- make script executable along with read and write that are there already
```bash
chmod +x <script_file.py>
```
- make bin directory in home folder and mv script there
```bash
cd ~
mkdir bin
mv path/to/script_file.py ~/bin/.
```

That is it. It is applicable only on Linux. Path to python env also might be different depending on how and where you install python. If want to specify python version then n step 1 replace python by python*. Place version numbers at *.
