---
layout: post
title:  "CSS Standards and Normalization Part 2: Framework"
date:   2018-03-12
author: Josh Boland
categories: css
---

## Purpose
The purpose of this series will be to define some standardized best practices for how CSS is composed and organized in our projects.

This is part 2 of 4: Framework.
In this article, we will discuss Bootstrap as our framework of choice and in what ways we lean on Bootstrap to style a site.

It is worth noting, this article focuses primarily on how we use Bootstrap in the context of building a "designed" or "brochure" site
as opposed to an application.

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

In pursuit of these goals, we only use the parts of Bootstrap that do not require javascript and that are not highly opinionated in terms of markup structure.

For example, the button styles in Bootstrap provide some great defaults and can be customized to match the theme of a site.
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
<!-- container-fluid is also available for fluid scaling -->
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
In most cases, outputs can be customized by using variable overrides. Bootstrap 4 defines almost all modifiable properties of its modules as variables.
By overriding these variables with our own values, we can customize the modules.
To override variables, we include a "bootstrap_variables" file above the Bootstrap include in the main sass file.
 
```sass
//override bootstrap default variables
@import "bootstrap_variables";

//bootstrap styles
@import "node_modules/bootstrap/scss/bootstrap";
```

As an example, lets remove the border radius from the default bootstrap buttons.
All of bootstrap's variables are located in `node_modules/bootstrap/scss/_variables.scss` for reference (assuming you use npm to install bootstrap).
They define the following variables for buttons:

```sass
// Buttons
//
// For each of Bootstrap's buttons, define text, background and border color.

$input-btn-padding-y:       .5rem !default;
$input-btn-padding-x:       .75rem !default;
$input-btn-line-height:     1.25 !default;

$input-btn-padding-y-sm:    .25rem !default;
$input-btn-padding-x-sm:    .5rem !default;
$input-btn-line-height-sm:  1.5 !default;

$input-btn-padding-y-lg:    .5rem !default;
$input-btn-padding-x-lg:    1rem !default;
$input-btn-line-height-lg:  1.5 !default;

$btn-font-weight:                $font-weight-normal !default;
$btn-box-shadow:                 inset 0 1px 0 rgba($white,.15), 0 1px 1px rgba($black,.075) !default;
$btn-focus-box-shadow:           0 0 0 3px rgba(theme-color("primary"), .25) !default;
$btn-active-box-shadow:          inset 0 3px 5px rgba($black,.125) !default;

$btn-link-disabled-color:        $gray-600 !default;

$btn-block-spacing-y:            .5rem !default;

// Allows for customizing button radius independently from global border radius
$btn-border-radius:              $border-radius !default;
$btn-border-radius-lg:           $border-radius-lg !default;
$btn-border-radius-sm:           $border-radius-sm !default;

$btn-transition:                 background-color .15s ease-in-out, border-color .15s ease-in-out, box-shadow .15s ease-in-out !default;
```

In our `_boostrap_variables.sass` file in out project, we can add the following to re-set the border-radius property to 0:

```sass
$btn-border-radius: 0
```

For more on how to override variables, see Bootstrap's theming guide:
[Bootstrap Themeing Guide](https://getbootstrap.com/docs/4.0/getting-started/theming/)

## Setting up Bootstrap with NPM
Our preferred method for installing Bootstrap is by using npm.
Inside your site directory (typically at the root level) run the following:

```
npm install bootstrap
```

This will install the Bootstrap package in your node_modules folder.

Then, to include Bootstrap in your style build, import Bootstrap from the node_modules folder inside your main.sass file.
You should import Bootstrap at the top of the file, which will allow any of your components to tweak the default styles if needed.
Depending on the where node_modules is in relation to your Sass files, your import path may differ.
 
```sass
@import '../node_modules/bootstrap/scss/bootstrap';
```
 