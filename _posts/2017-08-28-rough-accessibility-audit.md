---
layout: post
title:  "Accessibility Resources"
date:   2017-11-20
author: Ben Robertson
categories: accessibility
---
## Resources
 - [Web Content Accessibility Guidelines 2.0](https://www.w3.org/TR/WCAG20#intro)
 - [A Simple Intro to Web Accessibility](https://www.upandup.agency/accessibility/web-accessibility-intro)
 - [Getting started with VoiceOver &amp; Accessibility](https://bocoup.com/blog/getting-started-with-voiceover-accessibility)
 - [Understanding Layout for Screen Readers](https://www.upandup.agency/accessibility/understanding-layout-screen-readers)
 - [Focus Rings for Keyboard Interactions Only](https://benrobertson.io/accessibility/focus-ring-keyboard-only)
 - [4 JavaScript Techniques for Building Accessible Web Interfaces](https://benrobertson.io/accessibility/javascript-accessibility)

## Running a quick and dirty accessibility audit
Here are some quick things you can check to perform a quick and dirty accessibility audit:

 - Can you navigate using the tab key?
 - Are there discernible focus states for everything that receives focus?
    - as you tab through the page, can you tell where the current focus is?
 - Are any hidden / off-screen items focusable?
 - Is the content of the page screen reader friendly?
    - Fire up your screen reader and start working through the page.
    - [Getting started with VoiceOver &amp; Accessibility](https://bocoup.com/blog/getting-started-with-voiceover-accessibility)
 - Do all images have meaningful `alt` attributes?
 - Are any custom elements interactive with a screen reader?
 - Is the user alerted to new content added to the page?
    - Is the focus moved to new content added to the page?
 - Check the page structure (use the VoiceOver rotor (CMD+U) to quickly see all headings / landmarks)
     - are heading tags used correctly?
     - are there any landmarks?
 - Check color contrast using [axe](https://chrome.google.com/webstore/detail/axe/lhdoppojpmngadmnindnejefpokejbdd) and/or [Chrome Accessibility DevTools](https://chrome.google.com/webstore/detail/accessibility-developer-t/fpkknkljclfencbdbgkenhalefipecmb)


## Tools for More Detailed Audits

### [WAVE](http://wave.webaim.org/extension/)
The best thing about this tool is that it is fast. It is also easier for people who are not familiar with Chrome DevTools.

Good for:
 - page audits
 - Quickly identifying the structure of a page
 - Pointing to errors exactly where they occur on a page
 - quickly stripping CSS from a site to see the html structure
 - built in color contrast checker
 - identifying misused ARIA attributes


### [aXe](https://chrome.google.com/webstore/detail/axe/lhdoppojpmngadmnindnejefpokejbdd)
This tool will give you more specific, code-level references to accessibility issues. Good for people who are comfortable in DevTools.

Good for:
 - page audits
 - quickly identifying the code that is causing an accessibility issue
 - gives suggestions for fixing
 - shows what guidelines you might be breaking

### [Chrome Accessibility DevTools](https://chrome.google.com/webstore/detail/accessibility-developer-t/fpkknkljclfencbdbgkenhalefipecmb)
Gives you element-level accessibility information in Chrome DevTools.

Good for:
 - inspecting specific elements
 - identifying color contrast issues
 - showing what text screen readers will see


