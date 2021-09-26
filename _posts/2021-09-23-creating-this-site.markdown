---
layout: post
title: "Creating this site ☻"
date: 2021-09-23
categories: ['learning']
author: Sunil Dhaka Rani Malukana
---

Hello again ☻. Here I am planning to write a journal about creating this website using jekyll. I am using linux os, and code snippets from terminal will be according to linux. So be aware. Now to install you need to type these commmands in terminal window.

```bash
sudo apt install make build-essential ruby ruby-dev
sudo apt install gem
sudo gem jekyll bundler
```

**Note:** here sudo is being used to make changes like installing or unistalling or writting something in a file etc, as an administrator. We are installing ruby libraries like jekyll and bundler using gem because gem manages ruby software libraries, by manages I mean downloading, installing and managing the software history. BUndler is being installed so that we are running the same version of Jekyll and its plugins across different environments. Now to create a new website named `--blog_cum_portfolio--` type these, after changing to your home directory.

```bash
cd ~/
jekyll new blog_cum_portfolio
```

Now, to serve this newly created static website run,

```bash
jekyll serve -o --liverload
```

To stop press `Ctrl + C`. I have used `-o` to show build site on my default web browser automatically and `--livereload`(read as: live reload) to reload automatically whenever there is some changes made. To view and learn more about these use `--help` page in terminal. Also your website locally can be viewd at [http://localhost:4000](http://localhost:4000). If you find some issue during build use [jekyll troubleshooting pages](https://jekyllrb.com/docs/troubleshooting/#configuration-problems).

**Link to other posts using `post_url`**

Now without worrying about breakdown of other links, we can give url to other posts in your current post. This is how you use it. I also have used below to give link to my previous post.

```bash
{% post_url 2021-09-22-let-us-start %}
[Name of Link]({ /post_url 2021-09-22-name-of-post })
```

**Add an image and a pdf**

To add image,

```jekyll
![Alternative image text.](/assets/postcard.jpg)
![go to this pdf file](/assets/postcard.pdf)
```

Here is the image of my postcard, and here is my [pdf](/assets/postcard.pdf)
![Postcard image](/assets/images/postcard.jpg)

**Note:** When there is error like *jekyll server is in use* get the *ps* ID and kill it like this,

```bash
ps aux | grep -ID-
```
and then re-build the site.

**Assets:** Create a new directory named `assets` to include your pdfs, images, javascripts and other files. You can mkae different directories for all types of files. I have done,

```bash
mkdir pdfs ppts images html css js
```
**Mathjax and Code:** This is how we include an equation, inline: `$E=mc^2$` shows as $E=mc^2$, and block as, `$$ \bar{X}=\frac{\sum_{i=1}^n x_i}{n} $$`

$$ \bar{X}=\frac{\sum_{i=1}^n x_i}{n} $$

Although mathjax is not included in jekyll. We can include it by inserting these scripts at the end of `_layouts/default.html`

```html
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    processEscapes: true
  }
});
</script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
```
I have used two `back-ticks` to show what is being wriiten in english to write math equations. Two back ticks can be use to highlight any text in paragraphs. For code `rouge` highlighter, add these lines in your `_config.yml`

```yml
#conversion
markdown: kramdown
highlighter: rouge

# markdown processing
kramdown:
  input: GFM
```

Although jekyll uses GitHub Flavored Markdown (GFM) processor by default for syntax highlighting but you can specifiy by `input: GFM`. 

[Check Out Scrapping!]({% post_url 2021-09-22-let-us-start %})
