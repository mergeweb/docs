---
layout: post
title:  "Drupal Development FAQ"
date:   2015-9-11
author: Ben Robertson
categories: drupal
---

#### [Where are the sites theme files?](#themefiles)
#### [Where do I add css/javascript?](#enqueue)
#### [What css compiler do we use?](#compiler)
#### [Where does the css compiler go?](#cssfiles)
#### [Where do I add external javascript files (ie. Typekit, Google Fonts)?](#externaljs)
#### [Where are the theme templates?](#templates)
#### [What is the naming convention for theme template files?](#naming)
#### [How do I get content to appear on the page?](#content)
#### [Where do I install modules?](#modules)
#### [What modules should I use in development?](#develmodules)
#### [What non-core modules should I use?](#non-coremodules)
#### [What folders need to be excluded from git?](#gitignore)

### <a name="themefiles"></a> Where are the sites theme files?
app/sites/all/theme/themename

### <a name="enqueue"></a> Where do I add css/javascript?
CSS and JS that need to be included in the theme should be added in the themename.info file. Each time you add a new stylesheet or script, you will need to clear the Drupal cache before your changes will become active.

Stylesheets go like this:

`stylesheets[all][] = compiled_css/merge.style.css`

Scripts go like this:

`scripts[] = js/main.js`

### <a name="compiler"></a> What css compiler do we use?
SASS with Compass

### <a name="cssfiles"></a> Where does the css compiler go?
Sass should be configured to compile to `themename/compiled_css/merge.style.css`.

sass files should be in `themename/scss`. Please use scss syntax

### <a name="externaljs"></a> Where do I add external javascript files (ie. Typekit, Google Fonts)?
External javascript should be added in the `themename/template.php` file.
Do it like this:

{% highlight php %}
<?php
function themename_preprocess_html(&$vars) {
	// Helpful comment that describes what you are adding
  drupal_add_js('//use.typekit.net/mok7tep.js', 'external');
}
?>
{% endhighlight %}
[(see here for more info)](https://www.drupal.org/node/171213)

### <a name="templates"></a> Where are the theme templates?
Theme templates should be kept in a folder called templates within the theme folder. The templates should be organized into folder within the template folder based on what content they template.
Here is the Drupal template hierarchy (most general to most specific):

1. Page
2. Region
3. Block
4. Node
5. Fields

If you are using flexible content (ie. Paragraphs module and Field Collection module), then you will also have paragraph items modules and field collection modules.

Most of the default Drupal templates are located in `app/modules/system`. Default field template is in `app/modules/field`, default node template is in `app/modules/node`. You can override these templates in your theme by creating an identically named file in your theme and styling it.

### <a name="naming"></a> What is the naming convention for theme template files?
A comprehensive list of naming conventions for theme template files is located [here](https://www.drupal.org/node/1089656).

### <a name="content"></a> How do I get content to appear on the page?
To get content that you have entered in the backend to appear, put this in your `page.tpl.php`:
{% highlight php %}
<?= render($page['content']); ?>
{% endhighlight %}
For any other template, you can use

{% highlight php %}
<?= render($content); ?>
{% endhighlight %}

`$content` is an array with various sections. You can show or hide various sections (ie comments and stuff).

### <a name="modules"></a> Where do I install modules?
sites/all/modules

### <a name="develmodules"></a> What modules should I use in development?

####Devel and Devel Themer

Devel enables a bunch of development features. The one I use the most is the `krumo()` function, which when you pass a variable (like `$content`) to it shows you all the arrays/properties of that variable.

Devel Themer will give you a nice overlay that lets you see what variables are available in different regions on the page.

### <a name="non-coremodules"></a>What non-core modules should I use?

#### Flexible Content
 - [Entity API](https://www.drupal.org/project/entity)
 + [Paragraphs](https://www.drupal.org/project/paragraphs)
 - [Field Collection](https://www.drupal.org/project/field_collection)
 - [Link](https://www.drupal.org/project/link)
  - use this for any CTA / Link field. Makes CTAs so much better.

#### Parent/Child Relationship
 + [Entity Reference](https://www.drupal.org/project/entityreference)

#### WYSIWIG Editor
 + [CKEditor](https://www.drupal.org/project/ckeditor)
 + [Better Formats](https://www.drupal.org/project/better_formats)
  + Allows us to control which fields get a CKEditor or plain text.  
  + [There are some good recommendations here for WYSIWYG considerations in Drupal](http://www.chapterthree.com/blog/drupal-wysiwyg-best-practices)

#### Image Management
 - [IMCE](https://www.drupal.org/project/imce)

#### jQuery version
 - [jQuery Update](https://www.drupal.org/project/jquery_update)
  + Allows you to set Front-End jQuery version and Backend jQuery

#### Better URL Structure
 - [Pathauto](https://www.drupal.org/project/pathauto)

### <a name="gitignore"></a> What folders need to be excluded from git?


`sites/*/*settings*.php`

`/sites/*/files`

`/CHANGELOG.txt`

`/COPYRIGHT.txt`

`/INSTALL*.txt`

`/LICENSE.txt`

`/MAINTAINERS.txt`

`/UPGRADE.txt`

`/README.txt`

`sites/all/README.txt`

`sites/all/modules/README.txt`

`sites/all/themes/README.txt`

`**/.sass-cache`

`**/.sass-cache/*`

`**/compiled_css/*`

`/cache`
