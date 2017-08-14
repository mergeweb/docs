---
layout: post
title:  "Wordpress CSS Versioning - Override Browser Caching"
date:   2016-02-03
author: Jordan Barber
categories: general
---

Browser caching is a wonderful thing.  It speeds up our site's performance and creates a better experience for repeat visitors. But when we push new changes to a live Wordpress site, we need to make sure that any previous user doesn't view our new updated site with an older version of our site's CSS.

There are many ways to make sure this doesn't happen and (looking at a few of our Wordpress sites) we are currently using all of these methods.  We can:

+ Manually update the version number of our enqueued CSS when we push a new change
+ Add a timestamp to our version number in our enqueued stylesheet
+ Or append the Release directories version number to our CSS

Manually updating the version number is a hassle and adding a timestamp fixes the issue but it does not allow us the benefits of browser caching for our users.  The best method of these three is definitely to append the 'Release' directories version to our CSS as it allows the benefits of browser caching while making sure to override the cache when a change is pushed live.  But...

Turns out there is a simpler, automated and easier way to accomplish this using PHP's built in <code>filemtime</code> function. To do this simply use the following method when you enqueue your stylesheet.

<code>wp<u> </u>enqueue<u> </u>style( 'main<u> </u>css', get<u> </u>template<u> </u>directory<u> </u>uri() . '/compiled<u> </u>css/main.css' , false, filemtime( get<u> </u>template<u> </u>directory() . '/compiled_css/main.css' ), 'screen' );</code>

The above method adds a new version to the stylesheet anytime the <code>main.css</code> file is updated.  This allows us the benefits of browser caching while making sure that the new css version is updated when code is pushed live.  And it's all right there in the <code>enqueue_style</code> function.
