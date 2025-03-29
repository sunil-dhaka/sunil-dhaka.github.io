---
layout: post
title: "Creating this site ☻"
date: 2021-09-23
categories: ['web']
tags: ['jekyll']
author: Sunil Dhaka
published: false
---

Hello again! ☻ I thought I'd document my journey of creating this website using Jekyll. This is mainly for my own reference, but you might pick up some useful tips along the way. Since I'm working on Linux, all terminal commands will reflect that environment.

## Getting Started with Jekyll

First things first—installation. To get Jekyll up and running, we need to install Ruby and some dependencies:

```bash
sudo apt install make build-essential ruby ruby-dev
sudo apt install gem
sudo gem jekyll bundler
```

Why all these packages? Well, Jekyll runs on Ruby, and Gem manages Ruby libraries by handling downloads and installations. Bundler ensures consistent versions of Jekyll and its plugins across different environments—super important for reliable builds!

## Creating Your Jekyll Site

Once everything's installed, creating a new site is simple. I'll create one called "blog_cum_portfolio":

```bash
cd ~/
jekyll new blog_cum_portfolio
```

To preview your new site locally, run:

```bash
jekyll serve -o --liverload
```

The `-o` flag automatically opens your default browser to show the site, while `--livereload` refreshes the page whenever you make changes—super handy while developing! To stop the server, just hit `Ctrl + C`. You can always access your local site at [http://localhost:4000](http://localhost:4000).

Tip: If you run into build issues, the [Jekyll troubleshooting pages](https://jekyllrb.com/docs/troubleshooting/#configuration-problems) are incredibly helpful.

## Linking to Other Posts

One of Jekyll's strengths is how it handles internal links. To link to another post without worrying about breaking the URL structure:

```bash
{% post_url 2021-09-22-let-us-start %}
[Name of Link]({ /post_url 2021-09-22-name-of-post })
```

## Adding Images and PDFs

Adding media is straightforward too:

```jekyll
![Alternative image text.](/assets/postcard.jpg)
![go to this pdf file](/assets/postcard.pdf)
```

Here's my postcard image, and here's my [pdf](/assets/postcard.pdf)
![Postcard image](/assets/images/postcard.jpg)

## Troubleshooting

Ever seen a "Jekyll server is in use" error? You can resolve it by finding and killing the process:

```bash
ps aux | grep -ID-
```

## Managing Assets

For organization, create an assets directory with subdirectories for different file types:

```bash
mkdir pdfs ppts images html css js
```

## Math and Code Highlighting

For equations, you can use inline math like `$E=mc^2$` which renders as $E=mc^2$, or block equations:

`$$ \bar{X}=\frac{\sum_{i=1}^n x_i}{n} $$`

$$ \bar{X}=\frac{\sum_{i=1}^n x_i}{n} $$

Jekyll doesn't include MathJax by default, but you can add it by inserting this script at the end of `_layouts/default.html`:

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

For syntax highlighting with Rouge, add these lines to your `_config.yml`:

```yml
#conversion
markdown: kramdown
highlighter: rouge

# markdown processing
kramdown:
  input: GFM
```

Jekyll uses GitHub Flavored Markdown (GFM) by default, but it's good practice to specify it with `input: GFM`. You can customize syntax highlighting by editing the `_syntax-highlighting.scss` file.

[Check Out Scrapping!]({%- post_url 2021-09-22-let-us-start -%})
