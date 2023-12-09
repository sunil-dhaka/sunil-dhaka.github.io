---
title:  "working with conda environments"
layout: post
permalink: working-with-conda-environments
date:   2022-05-01
categories: ['conda']
tags: ['conda','python']
author: Sunil Dhaka
---
Conda environments help in handling multiple projects and their requirement dependencies seperatly. It is like creating seperate folders for seperate tasks/projects.

- **install conda:** [official docs](https://docs.anaconda.com/anaconda/install/linux/)
- **create env:**
```bash
# version of python as per ones preference
conda create --name/-n <env-name> python=3.8
# create conda env useing YAML files
conda env update -f <file-path>
```
- **actiavte and deactivate env:**
```bash
conda activate <env-name>
conda deactivate
```
- **remove env:**
```bash
# method-1
conda env remove --name/-n <env-name>
# method-2
rm -rf <path-of-env-dir>
# method-3
conda remove --name <env-name> --all
```
- **install modules:**
```bash
# first activate and then pip
conda activate <env-name> | pip install <module-name>
# directly install using conda
conda install -n <env-name> <module-name>
```
- **misc:**
```bash
# to see env list
conda info --envs
```

- **a demo YAML file:**
```yaml
# YAML file for Conda Environment

# Name of the Conda environment
name: my_environment

# Conda channels where packages will be searched
channels:
  - defaults
  - conda-forge
  # Additional channels can be added as needed

# Main dependencies installed using Conda
dependencies:
  - python=3.8
  - numpy>=1.18

  # Additional Conda packages can be listed here

  # Example: Uncomment the following line to include matplotlib
  # - matplotlib

  # Example: Uncomment the following line to include pandas
  # - pandas

  # Specify versions, constraints, or additional packages as needed

# Dependencies installed using pip (Python packages not available in Conda)
# Useful for packages that are not available in Conda repositories
# or for more recent versions of certain packages
# Note: 'pip' itself is automatically included
pip:
  - requests>=2.22
  - Flask>=1.1

  # Additional pip packages can be listed here

  # Example: Uncomment the following line to include pytest
  # - pytest

  # Specify versions or additional packages as needed

```
