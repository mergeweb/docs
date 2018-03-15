---
layout: post
title:  "CSS Standards and Normalization Part 2: Framework"
date:   2018-03-12
author: Josh Boland
categories: css
---

## Purpose
The purpose of this series will be to define some standardized best practices for how our CSS is composed and organized in our projects.

This is part 2 of 4: Framework.
In this article, we will discuss Bootstrap as our framework of choice and in what ways we lean on Bootstrap to style a site.

## Why Bootstrap
Our choice to use Bootstrap is preferential. There are merits to other frameworks, Bootstrap is just the one we like best.
Using Bootstrap on multiple projects is beneficial for our development team in several ways. 
1. We can assume a basic set of patterns and elements are available to us in every site we build.
2. We know that common classes will always produce the same output and don't have to spend time learning one-off approaches unique to each site.
3. Because of the predictability in the code, sites can be mutually understood, allowing for better collaboration.

## Using Bootstrap
Using Bootstrap does require knowing about Bootstrap. 
It will be beneficial to take a look through the [Bootstrap Docs](https://getbootstrap.com/docs/4.0/getting-started/introduction/) to get familiar with the framework.
Focus on the following sections, as they are used the most often in our sites:
 - Layout
    - Grid
    - Utilities for Layout
 - Components
    - Alerts
    - Badge
    - Buttons
    - Card
    - Forms
    - Input Groups
    
### Relying on what exists
Generally, we like to leverage Bootstrap to accomplish 2 major tasks:
- Setting up page layouts
- Providing base styling for common components

In pursuit of these goals, we primarily use the parts of Bootstrap that do not require javascript and that are not highly opinionated in terms of markup structure.

For example, the button styles in Bootstrap provide some great defaults and can be overridden to match the theme of a site.
Button styles require only one class: `.btn`, and they do not rely on any particular markup (though button styles should generally be applied to `<button>` elements) 

Conversely, the "Jumbotron" component requires some specific markup structure and may be harder to naturally work into a specific design.
We would only use the Jumbotron component if it fit our needs closely for a specific project. 
Otherwise, a component like Jumbotron would be created as a custom component, with markup and styles that served out needs exactly.

The components listed in the following sections we would like to ALWAYS rely on.
Other components can be used at the author's discretion if they fit the needs of a given site.

#### Layout Constructs
For layout purposes, we use Bootstrap to do the following:
- Provide a grid structure
- Provide layout modifier classes
    - Display property alterations (using classes like d-flex for flexbox)
    - Margin and padding alterations (like mb-1 for adding some margin on the bottom of something)
 
When building a site, grid structure and layout modifiers should always rely on Bootstrap.
Generally, grid layouts are defined as follows:

```html
<!-- container limits the width of the grid -->
<div class="container">
    <div class="row">
        <div class="col-6">
            Column 1 content
        </div>
        <div class="col-6">
            Column 2 content
        </div>
    </div>
</div>
```
For more documentation on grids, refer to Bootstrap's [Grid System](https://getbootstrap.com/docs/4.0/layout/grid/) overview.

Using layout utility classes properly is trickier, and comes with some practice. 
Often, things like display, margin or padding are defined at the component level on a component class. Defining these properties as part of a component is typically best.
Layout utility classes should not be the de-facto method for adding spacing or changing the display behavior of a layout.
However, there are instances where they can be useful.

Here is an example of when using a layout utility class would be good:

Let's say you have some cool buttons on your site. Because things often appear below the buttons, the default button class has some reasonable margin bottom applied.

```html
<button class="btn">Cool Button!</button>
<p>Some extra content</p>
```

```sass
.btn
    margin-bottom: 1rem
```

Now, in most cases, these buttons will look good in a layout. But in one spot, you have a small line of disclaimer text you want to show right under the button.
You wonder if you should create a modifier for the button class like `.btn--discalimer-under` or something like that, but that seems silly, it's only one spot!
This would be a good place to do use Bootstrap's `mb-0` class to remove the margin on the bottom of the button.

```html
<button class="btn mb-0">Submit Form</button>
<p class="small">Disclaimer Text</p>
```

#### Common Elements
When adding common elements to a layout, we like to rely on Bootstrap to define base styles for the following things:
- Alerts
- Badges
- Buttons
- Cards
- Forms
- Input Groups

Other components are used on a discretionary basis. Refer to the Bootstrap docs to dee examples of each of the components listed above.
These common components are often tweaked or customized for a given site or theme. Our preferred modification technique is discussed in the following section.

### Modifying where necessary
Bootstrap is made to allow for its outputs to be customized.
There are 2 primary ways that we customize Bootstrap's default styles to fit the needs of a given site.
- Overriding Bootstrap variables
- Overriding default styles for common components

#### Variable Overrides
Bootstrap uses a lot of Sass variables when it builds its styles.
Sometimes, it is helpful to customize these variables. For example, you may wish to provide some additional breakpoints for your layout.
To override variables, we include a "bootstrap_variables" file above the Bootstrap include in the main sass file.
 
```sass
//override bootstrap default variables
@import "bootstrap_variables";

//bootstrap styles
@import "node_modules/bootstrap/scss/bootstrap";
```

For specifics on how to override variables, see Bootstrap's theming guide:
[Bootstrap Themeing Guide](https://getbootstrap.com/docs/4.0/getting-started/theming/)

#### Modified Elements
Theming also requires tweaking some default components to match the look and feel of the site.
One common example of this is buttons. Bootstrap provides great defaults for button styles, but we may want to tweak them a bit.
To accomplish this, we define our own button component, using the same `.btn` class, and import it below the Bootstrap styles in the main.sass file.

```sass
//bootstrap styles
@import "node_modules/bootstrap/scss/bootstrap";

//custom button component
@import "components/button";
```

Inside the button component, we may do things like remove the border radius or change the font of the buttons.
The rest of the button styles will be inherited from the bootstrap `.btn` class. 

```sass
//custom button rules
.btn
    border-radius: 0
    font-family: "Avenir", Helvetica, sans-serif
```

## Setting up Bootstrap with NPM
Our preferred method for installing Bootstrap is by using npm.
Inside your site directory (typically at the root level) run the following:

```
npm install bootstrap
```

This will install the Bootstrap package in your node_modules folder.

Then, to include Bootstrap in your style build, import Bootstrap from the node_modules folder inside your main.sass file.
You should import Bootstrap at the top of the file, which will allow any of your component customizations to tweak the default styles if needed.
Depending on the where node_modules is in relation to your SASS files, your import path may differ.
 
```sass
@import '../node_modules/bootstrap/scss/bootstrap';
```
 