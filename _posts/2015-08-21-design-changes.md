---
layout: post
title:  "Design Change Annotations"
date:   2015-8-21
author: Ben Robertson
categories: scss
---

Designs change during development. It happens. You might be working on something and the client asks for a change or something just doesn't look as good as it did in the comp.

If that happens, it's important that we document the change in our code, since we won't be receiving new comps but will want an authoritative way to say that this is right.

Here's how we're going to do it:

{% highlight scss %}
//comp-change 1 - A brief description of your change goes here.
{% endhighlight%}

Every comment should start with <code>//cd</code>, followed by a number. Start at 1 and go up as the changes add up. (Hopefully there won't be that many changes.)

 If the change you are making affects more than one area of the code, add a decimal to your number and increment from there (1.1, 1.2, 1.3, 2, etc). That way we can search our code for all instances of that code by searching for <code>//cd 1.</code> and that will pull up all the related changes.

If you end up making a ton of changes, it might even be good to document those changes in a separate markdown file, making sure to include the ID's of the changes.
