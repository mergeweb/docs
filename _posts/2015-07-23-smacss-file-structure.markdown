---
layout: post
title:  "SMACSS File Structure"
date:   2015-7-23
author: Ben Robertson
categories: scss
---

<em>Updated 2016-11-18 by Ben</em>

We follow the SMACSS model of organizing our CSS. SMACSS stands for Scalable Modular Architecture for CSS. The SMACSS way of thinking about CSS is to divide your CSS rules into several categories. Each of these categories will get their own directory in our Sass structure.

### Base Rules
Base rules are where we define all the base styles for our CSS--these are global styles that will be used everywhere and that act as a base for us to build our other styles off of.

Examples of things that would be in your `base` directory:

 - Global typography settings (`base/_typography.sass`)
 - Global utility classes (`base/_utilities.sass`)
 - Framework overrides, like Bootstrap (`base/_bootstrap_overrides.sass`)
 - Color variables (`base/_colors.sass`)
 - Any kind of reset, like normalize.css (`base/_normalize.sass`)


### Layout Rules
This is where we define the overall layout of the site. This for repeatable layout elements. Think containers, wraps, grids. Not for headers, footers, etc.

From the [SMACSS](https://smacss.com/book/categorizing) definition:
    <blockquote>Layout rules divide the page into sections. Layouts hold one or more modules together.</blockquote>

Take Bootstrap, for example. Classes like `.container`, `.row`, `.col-sm-4` are all layout components. Classes like `.nav-pill`, `.cards`, `.table` are not.

### Component Rules
This directory contains each component in a separate file. The name of the file should reflect the class name of the component. In a file named `components/_card.sass`, you should expect to find sass related to the `.card` class. This includes any variants (like `.card--white`, `.card--large`), media queries, and sub-element selectors (`.card__header`, `.card__footer`).

<i>These are called `modules` in SMACSS, but we will call them components. This avoids confusion in Drupal (where modules is used in a different context) and also lines up with the usage of components in React.js.
</i>

### State Rules
All states should be included in the component or layout file for that component. You don't need a separate directory for this.

State rules define when an object on our site gets a new state. States can include a mobile/tablet media query and if an element is active or inactive.

### The File Structure
Here's what the file structure should look like:

{% highlight sass %}
scss
 partials
   _mixins.scss
   _variables.sass
 smacss
   base/
   layout/
   components/
 config.rb
 upup.style.sass

{% endhighlight %}


### Other Resources

 - [SMACSS](https://smacss.com/book/)
 - [Drupal 8 CSS Architecture](https://www.drupal.org/docs/develop/standards/css/css-architecture-for-drupal-8)
 - [CSS Guidelines](http://cssguidelin.es/)
 - [Brad Frost | CSS Architecture for Design Systems](http://bradfrost.com/blog/post/css-architecture-for-design-systems/)
 - [Up&Up CSS Standards](http://bp.mergeweb.net/scss/2016/11/01/css-standards/)
