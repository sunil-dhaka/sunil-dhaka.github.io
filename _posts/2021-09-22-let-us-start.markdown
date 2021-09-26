---
title:  "Let us start!"
layout: post
header:
  teaser: /assets/images/let-us-start.png
date:   2021-09-22
categories: ['scrapping']
tags: ['python']
author: Sunil Dhaka
---
In this video we are going to parse information on [some](https://www.youtube.com/playlist?list=PLRVhX1weTUd_hs_mcpfwcOq18tSdL0y60) channel's content and store it into a csv file. Mainly information about video title, video discription, how many time it has been watched etc. I have given a step by step procedure, with that you should be able to parse detail about YT video on any playlist. Explanations are in `code` comments.

1. I will be using only three modules to do it.
```python
from bs4 import BeautifulSoup as bs
import requests # self explanatory
```
2. Request and store parsed data.
```python
link='https://www.youtube.com/playlist?list=PLRVhX1weTUd_hs_mcpfwcOq18tSdL0y60' # particular linkis used
r=requests.get(link) # requests data form link
page=r.text #convert into text
soup=bs(page,'html.parser') #parse using BS
#print(soup.prettify()) # if want to see the structure of parsed data uncomment it.
```
3. Get title of videos.
```python
title_soup=soup.find('meta',property='og:title')
vid_title=title_soup['content'] if title_soup else 'not found'
print(vid_title)
```
4. Get Discriptions of videos
```python
disrip_soup=soup.find('meta',property='og:description')
vid_discription=disrip_soup['content'] if disrip_soup else 'not found'
print(vid_discription)
```
> And,

```python
date_soup=soup.find('meta',itemprop='uploadDate') # does not work with playlist
upload_date=date_soup['content'] if date_soup else 'not found'
print(upload_date)
#->not found
```

**Reference**: [Medium article](https://medium.com/brainstation23/how-to-become-a-pro-with-scraping-youtube-videos-in-3-minutes-a6ac56021961)

**Language Identifier**: [Git Page](https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers)

**Syntax Themes**: [Pagments CSS Themes](http://jwarby.github.io/jekyll-pygments-themes/languages/python.html)

**Article**: [Article to follow](https://www.digitalocean.com/community/tutorials/how-to-control-urls-and-links-in-jekyll)
