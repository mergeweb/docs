---
layout: post
title:  "Crawling Sites to Generate a List of URLs"
date:   2016-03-07
author: Ben Robertson
categories: general
---

Here's the command to crawl a site and generate a list of urls:

`wget --spider -r http://www.example.com 2>&1 | grep '^--' | awk '{ print $3 }' | grep -v '\.\(css\|js\|png\|gif\|jpg\|JPG\)$' > urls.txt`

It will crawl http://www.example.com and spit out a list of urls at `urls.txt` inside of whatever directory you are currently in. Have fun!
