---
layout: post
title:  "CSS Standards and Normalization Part 3: File Structure and Types of Rules"
date:   2018-03-12
author: Josh Boland
categories: css
---

## Purpose
The purpose of this series will be to define some standardized best practices for how CSS is composed and organized in our projects.

This is part 3 of 4: File Structure and Types of Rules.
In this article, we will give an overview of how we break down our Sass into separate files and give a brief overview of each major file grouping.
This article is to serve as a primer for the tutorial in part 4, and to familiarize you with how we think about file structure and types of CSS rules.  

## Using multiple Sass files
We adhere (loosely) to principles of the [SMACSS Philosophy](https://smacss.com/) written by Jonathan Snook when it comes to organizing Sass rules and files.
Our practice is to create separate Sass files for each component or for each group of Sass rules that serve a common purpose.
As an example, a slideshow component would have its own "slideshow.sass" file. 
Or, all styles dealing with styling form elements would be grouped in a "forms.sass" file.   

Generally, files and rules are broken down into 3 subsections based on their intended use. 
1. Base
2. Components
3. Layouts

We'll explore the purpose of each of these sections in greater detail below.

### Overall file structure
For the CSS portion of a project, the general file structure looks like this:

```
\dist
\src
    \base
    \components
    \layout
_bootstrap_variables.sass
_variables.sass
_mixins.sass
main.sass
```
`dist` - this directory is where our final CSS files are output once they are built by the preprocessor

`src` - all the source Sass files are contained here

`src\base` - the base directory holds all of the Sass files with base-level rules

`src\components` - all component Sass files are stored here

`src\layout` - all layout Sass files are stored here

`_bootstrap_variables.sass` - any bootstrap-specific variables are contained in this file. 
This file is included in main.sass before importing bootstrap to customize any variables.
 
`_variables.sass` - contains any variables used in our Sass

`_mixins.sass` - contains any mixins utilized by our Sass

`main.sass` - the main file where all other sub files are imported. main.sass is what gets compiled into CSS.

 
## Base Styles
Our concept for what constitutes a "base" level rule is rooted in the SMACSS Philosophy.
To Quote Mr. Snook:

> A Base rule is applied to an element using an element selector, 
a descendent selector, or a child selector, along with any pseudo-classes. 
It doesn’t include any class or ID selectors. 
It is defining the default styling for how that element should look in all occurrences on the page.
>Source: [SMACSS Base Rules](https://smacss.com/book/type-base)

We do make the exception of having some modifier classes that count as "base" rules. In these instances,
the modifier classes are globally available. Examples might be `.small` for reducing the size of text
or `.animated-underline` to create a fancy effect when hovering over text.

For us the definition may better be stated:
> A base rule is a rule that is applied to an element via an element selector on a global scale, 
OR a rule that can be applied via a globally available class that is not specific to any component or layout.

Common files that are included in the base/ directory for a project are
```
base/_acessibility.sass  //adds styles that aid screen readers and assitive technology
base/_elements.sass  //styles that apply to all elements of a given type in a site
base/_forms.sass  //global form styles
base/_typography.sass  //declaring default font face and font sizes for the site
```

Each of these files deals with things on a global level. 
Base rules should be added to a site sparingly since they have a wide impact.

## Component Styles
Our concept for component styles is similar what the SMACSS Philosophy calls "Module Rules". 
Components make up the majority of styles for a site and deal with individual sub-parts of a site.
A component might be something like a navigation bar, a slideshow or a faculty profile card.

For each unique component in the site, a separate Sass file should be created. 
  
For example, we may look at a page and see that we have a large featured image section with a headline that overlays it.
As a unit, this set of elements belongs together. We might call this section the "Hero Banner." 
To add styles for it, we would create a new file in the components directory

```
components/_hero_banner.sass
```

After writing styles for the hero banner, we would then move on to the next component.

### Handling conditional styles
Conditional styles for a component should be handled inside that component file.
The most common example of this is handling responsive styling for a component.
By keeping all conditional styles contained in the component file, there is no guessing or searching around to find styles related to that component.

## Layout Styles
Layout styles are used to give the page structure and shape. While components deal with isolated and repeatable units of content,
layout styles deal with providing a structure or skeleton for those components to live in. 

A basic page will usually have at least 3 layout sections:
- Header
- Page Content
- Footer

The styles that dictate the appearance of these higher-level containers are considered layout rules.
The primary purpose of layout rules is to create large-scale structure.

For example, if we look at the design for a site and see that the header spans the full width of the page, has a blue background and is 100px high,
we would want to define those styles as layout styles by adding a separate `_site-header.sass` file in the `layout` directory.