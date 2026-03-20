---
layout: post
title: "Building CLIs for Product Research"
date: 2026-03-20
categories: ['dev']
tags: ['cli', 'python', 'productivity', 'terminal']
description: "I built dedicated CLIs for Amazon and Myntra to automate the product research I was doing manually with browser agents."
author: Sunil Dhaka
---

![Terminal showing amz searching for a pull up bar with price and ratings, and myn showing Nike Air Force product details with sizes and measurements](/assets/images/amazon-myntra-cli.svg)

I had been using browser agents to research products I wanted to buy. Feed it what I'm looking for, my use case, my budget -- and let it browse Amazon or Myntra, compare options, read reviews. It worked, but it was slow. A browser agent clicking through pages, waiting for renders, parsing screenshots. For something I do often on just two platforms, it felt super slow.

Most of my online spending happens on Amazon and Myntra. I wanted something faster for the repetitive part: pulling prices, reading what people actually say in reviews, checking if a product fits what I need. So I built dedicated CLIs for both -- `amz` for Amazon.in and `myn` for Myntra. Click-based, terminal-native, the kind of interface agents are actually good at using. No MCP server, no browser automation. Just commands.

Both platforms use server-side rendering. The data is already in the HTML -- prices, ratings, specs, size charts, even AI-generated review summaries. There's no useful API in the network tab, so parsing the SSR output was the only way. In the past, this would have meant me staring at View Source, hunting for the right `div` and `class` in BeautifulSoup, and watching it break every few weeks when the layout shifted. The difference this time: I let the agent figure out the selectors. It fetched the pages, mapped out the DOM structure, identified stable anchor points (element IDs that Amazon and Myntra depend on internally), and wrote the parsers. The whole thing came together in a long single session over couple of hours.

It's not bulletproof -- these are scrapers, and HTML structures can change. Amazon's price selectors, for instance, have broken before. But the anchor points we rely on most -- `#productTitle` on Amazon (stable since at least 2016), `window.__myx` on Myntra (their SSR hydration variable) -- have held up because there's simply no business reason to rename them. It's not that they can't change; it's that the coordination cost across teams and internal tooling makes it unlikely without a major rewrite. Good enough for a personal tool.

**Repos:**
- [amazon-cli](https://github.com/sunil-dhaka/amazon-cli) -- search, compare, product details, reviews, customer insights
- [myntra-cli](https://github.com/sunil-dhaka/myntra-cli) -- search, product details with size charts, orders, cart, wishlist
