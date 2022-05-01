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
