---
layout: post
title: download music from spotify using cli 
date: 2023-12-09
categories: [cli]
tags: [cli, tools, plug-n-play]
description: basic steps to follow when setting up your own pipeline for downloading spotify music locally through cli 
author: Sunil Dhaka
---

### Downloading Music from Spotify Using CLI

So, you're itching to grab some tunes from Spotify using the command line, huh? Here's a quick guide to get you groovin':

**Step 1: Create a Spotify App**

- Head over to [Spotify Developer Dashboard](https://developer.spotify.com/dashboard).
- Create a new app and note down the client ID and client secret from the app's settings.
- Export these credentials in your shell script file, like .zshrc or .bashrc.

**Step 2: Get the Tools**

- Visit [SathyaBhat's Spotify-dl Repo](https://github.com/SathyaBhat/spotify-dl).
- Follow the instructions to download the required files.
- Don't forget to create a virtual environment using Python3.

**Step 3: Activate Virtual Environment**

- Activate your virtual environment: `source venv/bin/activate` (for example).
- You're all set!

**Step 4: Read the Docs and Use the Tool**

- Explore the documentation to understand all the cool things you can do.
- Some basic examples to get you started:

```bash
# Download a track
$ spotify_dl -l "https://open.spotify.com/track/your_track_id"

# Download a playlist
$ spotify_dl -l "https://open.spotify.com/playlist/your_playlist_id"
```

Now, enjoy your music library without breaking a sweat. Happy listening! 🎵

Links:
1. [spotify web api](https://developer.spotify.com/documentation/web-api)
2. [youtube api v3](https://developers.google.com/youtube/v3/?hl=en)]
