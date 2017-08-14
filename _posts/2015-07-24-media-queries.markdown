---
layout: post
title:  "Media Queries in SCSS"
date:   2015-7-24
author: Ben Robertson
categories: scss
---

We write all our media queries within the _states.scss file. If the project you are working on follows our standard for SMACSS organization, you should be able to find that file inside:

<code>scss/smacss/_states.scss</code>

Instead of doing just a few big media query wrappers (one for mobile, one for tablet, etc), we nest our media queries inside of the module--or element--that they affect. This makes our code easy to read and it makes it a lot easier to maintain, since we know that all media queries are right with the element they affect.

We also use a really nifty mixin that makes our media queries super clean. Be sure to include this in <code>scss/partials/_mixins.scss</code> :

{% highlight scss %}
@mixin bp($point) {

  $bp-mobile: "(max-width: 767px)";
  $bp-tablet: "(max-width: 1024px)";
  $bp-xlarge: "(max-width: 1599px)";
  $bp-huge: "(min-width: 1600px)";

  @if $point == mobile {
    @media #{$bp-mobile}  { @content; }
  }
  @else if $point == tablet {
    @media #{$bp-tablet} { @content; }
  }
  @else if $point == xlarge {
    @media #{$bp-xlarge} { @content; }
  }
  @else if $point == huge {
    @media #{$bp-huge} { @content; }
  }

}

{% endhighlight %}

When you want to use the mixin to add a media query, you do it like so:

{% highlight scss %}

header{background: purple; height: 100px;
	@include bp(tablet){height:80px;}
	@include bp(mobile){height:50px;}
}

{% endhighlight %}

Here's your css output:

{% highlight css %}

header {
  background: purple;
  height: 100px; }
  @media (max-width: 1024px) {
    header {
      height: 80px; } }
  @media (max-width: 767px) {
    header {
      height: 50px; } }

{% endhighlight %}

<h3>What about performance?</h3>
Some people say this code is too bloated. Here's some <a href="http://presentations.kimberlyblessing.com/2013/rwdsummit/Optimizing%20Media%20Queries.pdf">awesome research</a> that Kimberly Blessing did on optimizing media queries, basically showing that it's not a big deal.

Chris Coyier and Dave Rupert also addressed the issue on <a href="http://shoptalkshow.com/episodes/175-rapidfire-48/#t=40:15">shoptalkshow</a>.
