---
title:  "How to scrape youtube playlist using python"
layout: post
permalink: youtube-scrapping-using-python
date:   2021-09-22
categories: ['web']
tags: ['python']
author: Sunil Dhaka
---

Ever wanted to extract information from YouTube playlists? I recently needed to grab details like video titles and descriptions from a YouTube channel, and thought I'd share my approach. This simple tutorial will walk you through scraping a [YouTube playlist](https://www.youtube.com/playlist?list=PLRVhX1weTUd_hs_mcpfwcOq18tSdL0y60) using Python. Let's dive in!

1. First, let's import the essential packages:
```python
from bs4 import BeautifulSoup as bs
import requests
```

2. Next, we'll make a GET request to fetch the page content:
```python
# Target the specific playlist URL
link='https://www.youtube.com/playlist?list=PLRVhX1weTUd_hs_mcpfwcOq18tSdL0y60'
# Use requests to fetch the webpage content
r=requests.get(link) 
page=r.text # Convert to text
soup=bs(page,'html.parser') # Parse with BeautifulSoup
#print(soup.prettify()) # Uncomment to see the HTML structure
```

3. To extract video titles:
```python
title_soup=soup.find('meta',property='og:title')
vid_title=title_soup['content'] if title_soup else 'not found'
print(vid_title)
```

4. For video descriptions:
```python
disrip_soup=soup.find('meta',property='og:description')
vid_discription=disrip_soup['content'] if disrip_soup else 'not found'
print(vid_discription)
```

5. Trying to get upload dates (which doesn't work for playlists):
```python
date_soup=soup.find('meta',itemprop='uploadDate') # Doesn't work for playlists
upload_date=date_soup['content'] if date_soup else 'not found'
print(upload_date)
#->not found
```

Web scraping doesn't have to be intimidating! YouTube's structured format makes it relatively straightforward to extract the information you need. You can expand on this foundation to extract other metadata using different property tags. Happy scraping!
