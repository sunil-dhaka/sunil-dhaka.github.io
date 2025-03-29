---
layout: post
title: Creating Python Executables 
date: 2022-03-16
categories: ['dev']
tags: ['python']
description: How to create standalone executables from Python scripts
author: Sunil Dhaka
---

# Turning Python Scripts into Standalone Applications

Ever wrote a useful Python script and wanted to share it with non-technical friends? Converting your Python code into standalone executables makes it possible for anyone to run your program—no Python installation required! Let's explore how to do this using PyInstaller.

## Setting Up PyInstaller

First things first, we need to install PyInstaller:

```bash
pip install pyinstaller
```

## Creating a Basic Executable

Let's start with a simple example. Say you have a Python script named `hello.py`:

```python
# hello.py
print("Hello, World!")
input("Press Enter to exit...")  # Keeps the terminal window open
```

To convert this to an executable:

```bash
pyinstaller hello.py
```

This generates several files and folders:
- `dist/`: Contains your executable
- `build/`: Temporary build files
- `hello.spec`: Configuration file for your build

Your executable will be inside the `dist/hello/` directory.

## Creating a Single-File Executable

For a cleaner, more portable solution, use the `--onefile` option:

```bash
pyinstaller --onefile hello.py
```

This creates a single executable file in the `dist/` directory. Much simpler to distribute!

## Adding an Icon

Want to make your app look more professional? Add a custom icon:

```bash
pyinstaller --onefile --icon=path/to/icon.ico hello.py
```

## Handling Hidden Imports

Sometimes PyInstaller doesn't automatically detect all the modules your script uses, especially if they're imported dynamically. In these cases, you'll need to specify them manually:

```bash
pyinstaller --onefile --hidden-import=module_name hello.py
```

## Adding Data Files

If your application needs access to external files (like images or configuration files), you can include them:

```bash
pyinstaller --onefile --add-data "path/to/file;destination_folder" hello.py
```

Note: On Windows, use semicolons (`;`) as separators. On Linux/Mac, use colons (`:`).

## Troubleshooting Common Issues

1. **Missing modules**: If your app crashes with an "ImportError," try adding the module with `--hidden-import`.
   
2. **File not found errors**: Make sure any files your app needs are included with `--add-data`.

3. **Anti-virus false positives**: Unfortunately, some anti-virus software flags PyInstaller executables as suspicious. You might need to add exceptions or sign your code.

Converting Python scripts to executables has made sharing my tools so much easier—no more walking friends through Python installation just to run a simple script! Have you tried creating executables from your Python code? Let me know how it went!
