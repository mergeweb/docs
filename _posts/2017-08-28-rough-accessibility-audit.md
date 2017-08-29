---
layout: post
title:  "Quick and Dirty Accessibility Audit"
date:   2017-08-28
author: Ben Robertson
categories: accessibility
---

Here are some quick things you can check to perform a quick and dirty accessibility audit:

 - Can you navigate using the tab key?
 - Are there discernible focus states for everything that receives focus?
    - as you tab through the page, can you tell where the current focus is?
 - Are any hidden / off-screen items focusable?
 - Is the content of the page screen reader friendly?
    - Fire up your screen reader and start working through the page.
 - Do all images have meaningful `alt` attributes?
 - Are any custom elements interactive with a screen reader?
 - Is the user alerted to new content added to the page?
    - Is the focus moved to new content added to the page?
 - Check the page structure (use the VoiceOver rotor (CMD+U) to quickly see all headings / landmarks)
     - are heading tags used correctly?
     - are there any landmarks?
 - Check color contrast using [axe](https://chrome.google.com/webstore/detail/axe/lhdoppojpmngadmnindnejefpokejbdd) and/or [Chrome Accessibility DevTools](https://chrome.google.com/webstore/detail/accessibility-developer-t/fpkknkljclfencbdbgkenhalefipecmb)
