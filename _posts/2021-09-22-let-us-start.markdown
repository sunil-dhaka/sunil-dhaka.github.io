---
title:  "How to scrape youtube playlist using python"
layout: post
permalink: youtube-scrapping-using-python
date:   2021-09-22
categories: ['scrapping']
tags: ['python']
author: Sunil Dhaka
---
In this post we are going to parse information on [some](https://www.youtube.com/playlist?list=PLRVhX1weTUd_hs_mcpfwcOq18tSdL0y60) youtube channel's content and store it into a csv file. Mainly information about video title, video discription, how many time it has been watched etc. I have tried my best to give a step by step procedure, with that you should be able to parse detail about YT videos on any playlist. Explanations are in `code` comments.

1. IMport required module,
```python
from bs4 import BeautifulSoup as bs
import requests
```
2. use `GET` requests,
```python
# particular link is used
link='https://www.youtube.com/playlist?list=PLRVhX1weTUd_hs_mcpfwcOq18tSdL0y60'
# to tell website server processer to get us particular html page we use python library `requests`
r=requests.get(link) # requests data form link
page=r.text #convert into text
soup=bs(page,'html.parser') #parse using BS
#print(soup.prettify()) # if want to see the structure of parsed html page uncomment it.
```
3. to get titles of videos,
```python
title_soup=soup.find('meta',property='og:title')
vid_title=title_soup['content'] if title_soup else 'not found'
print(vid_title)
```
4. to get discriptions of videos,
```python
disrip_soup=soup.find('meta',property='og:description')
vid_discription=disrip_soup['content'] if disrip_soup else 'not found'
print(vid_discription)
```
5. to get upload date etc,
```python
date_soup=soup.find('meta',itemprop='uploadDate') # does not work with playlist
upload_date=date_soup['content'] if date_soup else 'not found'
print(upload_date)
#->not found
```
Scrapping python is not that hard, it is a standard website and everything is nice and clean. To scrape other information you can use other property tags. I hope you found it intersting enough to get started on your own in python. Thank you.
