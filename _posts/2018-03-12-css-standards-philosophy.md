---
layout: post
title:  "CSS Standards and Normalization Part 1: Philosophy"
date:   2018-03-12
author: Josh Boland and Ben Robertson
categories: css
---

## Purpose
The purpose of this series will be to define some standardized best practices for how our CSS is composed and organized in our projects.

This is part 1 of 4: Philosophy.
In this article, we will explore our CSS philosophy and provide a high-level overview of how we write css.

## Principles
Our CSS philosophy borrows from several different frameworks and systems:
 - SMACSS by Jonathan Snook
 - BEM Architecture
 - Drupal 8
 - Bootstrap
  
The Goals of Drupal 8's CSS philosophy will serve as a good starting point for our CSS philosophy. Well-architected CSS should be:

>#### 1. Predictable
>CSS throughout Drupal core and contributed modules should be consistent and understandable. Changes should do what you would expect without side-effects.

>#### 2. Reusable
>CSS rules should be abstract and decoupled enough that you can build new components quickly from existing parts without having to recode patterns and problems you’ve already solved. – Philip Walton, CSS Architecture

>#### 3. Maintainable
>As new components and features are needed, it should be easy to add, modify and extend CSS without breaking (or refactoring) existing styles.

>#### 4. Scalable
>CSS should be easy to manage for a single developer or for large, distributed teams (like Drupal’s).

>Source:  [CSS architecture (for Drupal 8)](https://www.drupal.org/docs/develop/standards/css/css-architecture-for-drupal-8)

In addition, we should keep Mark Otto's Golden Rule in mind:

> Every line of code should appear to be written by a single person, no matter the number of contributors.

> [Code Guide](http://codeguide.co/#golden-rule) by Mark Otto

## Code Structure & Style

Here's an overview of how we structure and organize our code (borrowed from [css guidelines](http://cssguidelin.es/#syntax-and-formatting)):

 - Sass (not scss)
 - Four (4) space indents, no tabs
 - Multi-line css
 - Meaningful use of white space
 - Limit line length to 80 characters
 - Use single line comments
 - Clean Import Paths

### Sass
 Sass is our CSS preprocessor of choice.
 Sass is bracket-less {} and does not use semi-colons to terminate lines. Overall it provides a cleaner feel to the code.

 Each project should include Sass linting. Use our [default .sass-lint.yml file](https://gist.github.com/mergeweb/d317ffde258f718a2aa0d033161ac2fc). It will point out the rules below for you.

### Four space indents
 Since we aren't using brackets, this keeps the Sass clearer and also is aligned with our Javascript and PHP standards.
 Most code editors can be set up to use four spaces as the default indent.

### Multi-line css
Selectors and properties each get their own line.
This makes code legible and easier to parse visually

Do it like this:

 ```sass
.container
    width: 95%
    max-width: 40em
 ```
 
Not like this (with no brackets in SASS, single-line format is not valid): 
 ```css
.container{width: 95%; max-width: 40em;}
 ```

### Meaningful use of white space
White space can give context to class definitions. Include two empty lines between top-level css blocks, and one line between properties and nested classes.

Example:

```sass
.card
    padding: 1rem
    margin: 1rem

    &__header // leave one empty line above this since it is nested
        border-bottom: 1px solid gray


.card--white // two empty lines above this new class block
    background-color: white


```

### Nesting Depth
Nesting depth should be limited to 2.
This prevents un-necessary specificity of selectors and makes future maintenance easier.
The code is also easier to understand with limited nesting depth.

Example:

```sass
.block
    &__element
        &--modifier // Deepest Nest Allowed


```

### Limit line length to 80 characters
This helps make the code more readable without horizontal scrolling. 
Because you're writing multi-line css, this should really only come into play for comments. 
Limiting your line length will make your comments easier to read and therefore more useful. 
For instances like long urls or gradient syntax, don't worry about it.
Most code editors also have a setting for maximum line length.

Example:

```sass
// This file is where you override default Bootstrap variables. You
// can find a list of the default Bootstrap variables
// in _variables.scss
```

### Use single line comments
Your comments don't have to be on one single line, just don't use traditional multi-line comment format, ie: `/* These kinds of comments */`, 
because they will end up in the compiled css. Your comments, even if they take up multiple lines, should look like this:

```sass
// This file is where you override default Bootstrap variables. You
// can find a list of the default Bootstrap variables
// in _variables.scss
```

### Clean Import paths
You don't need to include leading underscores or filename extensions in your import paths. 
To stay consistent, your imports should look like this:

```sass
@import "base/typography" // where this file is base/_typography.sass
@import "base/colors"

@import "layout/grid"
@import "layout/containers"
```

## Writing Selectors
We have a few preferences when it comes to writing CSS selectors.
   - Write selectors for humans
   - Use BEM architecture for component structure
   - Avoid ID selectors
   - Avoid Vendor Prefixes

### Write Selectors for Humans
Class names should use full words rather than abbreviations. 
Remember that your class names are written for the benefit of other developers, not the computer. 
Prefer `class="block"` to `class="blk"`.

Class names for components should use dashes between words for legibility. 
Prefer `class="component-name"` to `class="componentname"`.

### Class Name Format (Using BEM)
We use a BEM-style class naming system. 
BEM stands for Block Element Modifier. You can also think about it as Component, Sub-object, Variant.
At a high level, BEM seeks to:

- Provide clear and purposeful naming of classes
- Create modular blocks that do not depend on cascading styles
- Avoid inheritance as much as possible

For more on BEM philosophy: [BEM Philosophy](http://getbem.com/)

More on how we interpret BEM philosophy will be explored in later articles. 
In general, BEM treats the highest level of a component as a "Block". A menu is a good example of a "Block":

```sass
.primary-menu
    //some menu styles
```

Related sub-parts of that menu are considered "Elements." In this case, items in the menu are a good example. 
Elements are given their own classes. Element class names use their block class name, followed by 2 underscores, then the element name.
Elements are NOT nested underneath their block selector in the SASS code.
Like so:

```sass
.primary-menu
    //some menu styles

.primary-menu__menu-item
    //styles for the menu item
```

"Modifiers" or "variants" of elements create slightly different versions of an element.
To create a modifier, add two dashes and the modifer name to the end of the element class name.
For our small example, lets say we want some menu items to have a bottom-border.

```sass
.primary-menu__menu-item--border-bottom
    //styles for the variant menu item with a bottom border
    border-bottom: 1px solid #000
```

If you prefer to leverage SASS's nesting capabilities, the example above can be written like so  
```sass
.primary-menu
    //some menu styles
    &__menu-item
        //styles for the menu item
        &--border-bottom
        //styles for the variant menu item with a bottom border
        border-bottom: 1px solid #000
```

In BEM architecture, you should not write classes with more than 1 set of "__". Each element in the BEM approach gets its own class,
They do not depend on each other. For example, If you had an `<a>` tag insid your `primary-menu__item`, it may tempting to
add a class of `primary-menu__menu-item__link`. This implies element dependency, that the link element depends on being inside the menu item.
Element dependency is something BEM strives to avoid. You would instead add a class to the `<a>` element of `primary-menu__link`.

```sass
// Do this
.primary-menu__menu-item
    //menu item styles
.primary-menu__link
    //link styles

// Don't do this
.primary-menu__menu-item__link
    //nope nope nope
```

### Avoid Using id Selectors
You can use them for Javascript or for providing anchor-links, but don't use them for styling.

### Avoid Vendor Prefixes
For sites that are using a build tool like Grunt or Gulp, we can skip vendor prefixes in the css in favor of using the [PostCSS Autoprefixer plugin](https://github.com/nDmitry/grunt-postcss). 
It will automatically determine what prefixes are necessary based on browser support requirements and only include the necessary ones.
If you build the auto-prefixer into a tooling-chain, don't do it on every compile during dev, it can slow down your build time.

### Use relative units for font-sizing.
Prefer relative units like `rem` or `em` for font-sizing. This allows for more flexible, more maintainable font styling. 
It also allows us to better control font styles based on what fonts have been loaded or not loaded.

Likewise, avoid specifying units for line-height. Line-height should be a ratio of font-size. 
You will need to write a lot less css if your line-height ratios are on to begin with.

### Namespacing
Consider name-spacing layout and javascript specific classes.

For instance, instead of using a class name like `.container`, you could use `.layout-container`. Or `.layout-grid`, over `.grid`. 
This gives the benefit of clear separation between component-specific and layout-specific styles.

Or for javascript that is manipulating the DOM, use something like `.js-behavior-hook`
 instead of `.behavior-hook` to make sure that it is a dedicated class not used for styling.

<i>Source: [Drupal 8 CSS Architecture | Class Name Formatting](https://www.drupal.org/docs/develop/standards/css/css-architecture-for-drupal-8#formatting)</i>


## Linting

The easiest way to put these guidelines into practice will be to use `sass-lint`. You can install it globally like so: `npm install -g sass-lint` or on a per-project basis like this: `npm install sass-lint --save-dev`

Add a copy of our [.sass-lint.yml](https://gist.githubusercontent.com/mergeweb/d317ffde258f718a2aa0d033161ac2fc/raw/5d47ebe144f691d96e0885bfd24c2d47d2905fde/.sass-lint.yml) to the root of your project.

To lint your project from the command line, you can do this:

`sass-lint -v`

Or, for as-you-go linting, install the sass-lint plugin for your respective editor:

 - [PHPStorm Lint Plugin](https://github.com/idok/sass-lint-plugin)
 - [Atom Plugin](https://atom.io/packages/linter-sass-lint)

 [sass-lint documentation](https://github.com/sasstools/sass-lint/)

## Resources
We leaned heavily on the following resources for these guidelines:

 - [Drupal 8 CSS Architecture](https://www.drupal.org/docs/develop/standards/css/css-architecture-for-drupal-8)
 - [CSS Guidelines](http://cssguidelin.es)
 - [SMACSS by Jonathon Snook](https://smacss.com/book/)
