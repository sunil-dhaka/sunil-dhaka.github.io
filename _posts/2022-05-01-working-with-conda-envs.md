---
layout: post
title: Managing Conda Environments Like a Pro
date: 2022-05-01
categories: ['dev']
tags: ['python']
description: A practical guide to working with Conda environments
author: Sunil Dhaka
---

# Mastering Conda Environments

If you're working with Python for data science or machine learning, you've likely encountered Conda. It's not just another package manager—it's a powerful environment manager that solves the notorious "it works on my machine" problem. Today, I'm sharing my workflow for keeping multiple projects neatly isolated using Conda environments.

## Getting Started with Conda

If you haven't installed Conda yet, I recommend the minimal [Miniconda](https://docs.conda.io/en/latest/miniconda.html) distribution. Once installed, verify it's working:

```bash
conda --version
```

## Creating Custom Environments

The beauty of Conda is creating separate environments for different projects. Here's how I typically set up a new environment:

```bash
conda create --name myenv python=3.8
```

This creates an environment named "myenv" with Python 3.8. I usually specify the Python version to avoid surprises later.

## Activating and Deactivating Environments

Before installing packages or running code, activate your environment:

```bash
conda activate myenv
```

When you're done, you can deactivate it:

```bash
conda deactivate
```

## Managing Packages

Once your environment is active, you can install packages using either conda or pip:

```bash
conda install numpy pandas matplotlib
# or
pip install tensorflow keras
```

I generally prefer using conda for packages available in Conda repositories, falling back to pip only when necessary.

## Viewing Your Environments

To see all environments you've created:

```bash
conda env list
```

To check what packages are installed in your current environment:

```bash
conda list
```

## Exporting and Recreating Environments

This is where Conda really shines for collaboration. You can export your environment configuration:

```bash
conda env export > environment.yml
```

Then anyone can recreate the exact same environment:

```bash
conda env create -f environment.yml
```

I always include the environment.yml file in my project repositories to make it easier for collaborators (and my future self).

## Removing Environments

Spring cleaning for your development setup:

```bash
conda env remove --name myenv
```

## Advanced Tips

- **Creating environments in specific locations**: Use the `--prefix` option to create an environment in a project directory:
  ```bash
  conda create --prefix ./env python=3.8
  ```

- **Updating all packages**: Keep your environment up-to-date:
  ```bash
  conda update --all
  ```

- **Clean up unused packages**: Free up disk space:
  ```bash
  conda clean --all
  ```

Proper environment management has saved me countless hours of debugging dependency conflicts. What's your Conda workflow? I'd love to hear your tips and tricks!
