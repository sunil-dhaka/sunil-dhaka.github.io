---
layout: post
title: Automating YouTube Uploads - Work Smarter, Not Harder
date: 2023-05-23
categories: ['dev']
tags: ['youtube']
description: How to automate video uploads to YouTube
author: Sunil Dhaka
---

# Set It and Forget It: Automating YouTube Uploads

Ever found yourself spending hours manually uploading videos to YouTube? I recently faced this challenge and built a simple automation pipeline using YouTube's API. Here's how you can do it too!

## Getting Started with the YouTube API

First, you'll need to set up authentication credentials:

1. **Create Your OAuth Client ID**
   - Head over to the [Google API Console](https://console.developers.google.com/)
   - Create a new project (call it whatever you like)
   - Navigate to "Credentials"
   - Click "Create OAuth Client ID"
   - Follow the setup wizard and download your credentials file (this will be named `client_secret.json`)
   - Return to Credentials → OAuth 2.0 Client IDs
   - Set the application type as "Desktop App" and give it a name
   - This `client_secret.json` file is your key to accessing all API services

2. **Install the Required Libraries**
   - Get everything you need with pip:
   ```bash
   pip install --upgrade google-api-python-client google-auth-httplib2 google-auth-oauthlib google-auth
   ```

3. **Prepare Your Upload Script**
   - Check out [Google's official guide](https://developers.google.com/youtube/v3/guides/uploading_a_video)
   - Create a `client_secret.json` file in the same format as shown in Google's examples
   - If you're using Python 3 (which you should be!), edit the print statements and exception handling in Google's sample code
   - Remove any references to `httplib` (it's deprecated in Python 3)
   - Install the OAuth2 client library:
   ```bash
   pip install oauth2client
   ```
   - Run your script to test everything works

## Taking Automation to the Next Level

Now for the fun part—full automation:

1. Create a dedicated folder for videos you want to upload
2. Place your Python upload script in the same folder
3. Create a scheduler script that runs your uploader at regular intervals (I use cron for this)

With this setup, you can simply drop videos into your folder, and they'll automatically be uploaded to YouTube on your schedule. No more tedious manual uploading!

Have you automated any other parts of your content workflow? I'd love to hear about your solutions in the comments.
