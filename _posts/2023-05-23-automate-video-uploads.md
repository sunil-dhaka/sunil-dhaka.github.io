---
layout: post
title: Automate Video Uploads
date: 2023-05-23
categories: [automation]
tags: [automation, video, youtube]
description: How to automate video uploads to YouTube
author: Sunil Dhaka
---

# Automate Video Uploads to YouTube using YouTube V3 API

#### Get Client ID and Client Secret
- Go to [Google API Console](https://console.developers.google.com/)
- Create a new project
- Go to Credentials
- Create OAuth Client ID
- Then follow the steps to create a new OAuth Client ID and download the credentials file(.json)
- Now again go to Credentials and OAuth 2.0 Client IDs
- Select app type as Desktop App and give some name
- This client_secret.json file will be used to authenticate your application for all API services

#### Install Google API Client Libraries
- Install Google API Client Libraries using pip
```bash
pip install --upgrade google-api-python-client google-auth-httplib2 google-auth-oauthlib google-auth
```

#### Upload Python File
- Follow [these instructions](https://developers.google.com/youtube/v3/guides/uploading_a_video)
- Make sure to create a file named `client_secret.json` in the same format as given in the instructions
- Also edit print and exception statements to make it compatible with Python 3
- remove httplib from imports and wherever used
- Install oauth2client
```
pip install oauth2client
```
- Run the script and test it out

#### Automate Video Uploads
- keep a seperate folder for videos to be uploaded
- keep the python script in the same folder
- create a new python script to run the upload script every 24 hours or so using cron
