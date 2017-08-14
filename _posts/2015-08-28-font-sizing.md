---
layout: post
title:  "Font-Sizing: An Overview of px, Em's, & Rem's"
date:   2015-8-28
author: Jordan Barber
categories: design
---

Until now, we have been using pixels for sizing fonts.  This is not necessarily a bad thing.  Some of the most popular and heaviest-traffic sites in the world currently use pixels when sizing their typography (Bootstrap did not move away from pixels until the release of Bootstrap 4 only a little more than a week ago).

But there are more modern, responsive approaches to web typography, namely the Em and Rem units.  The most significant difference between pixels and Ems/Rems is that the latter are relative, while the former is not.  These relative units are nothing new (the first [article](http://clagnut.com/blog/348/) on Ems was published in 2004).

This post summarizes these three approaches, pointing out their ups and downs and when is best to use which approach.

<h4>Pixels</h4>

Pixels are a safe, fixed unit of measure.  They are straightforward and exact.  Unfortunately, simplicity and precision are the only useful aspects of pixels (well, that and browser support). The downsides of pixels are that they are non-responsive and lack accessibilty.  Because pixels are not a relative unit, they cause much more work in responsive web design. Below is a basic set up of how we would handle responsive typography with pixels inside of our SMACCS 'states' environment (this sample assumes a 1:1.25 increment ratio in font-size).

{% highlight scss %}

html { font-size: 16px; 
	@include bp(large-screens){font-size: 20px}
}

h1 { font-size: 33px; 
	@include bp(large-screens){font-size: 41px}
}

h2 { font-size: 28px; 
	@include bp(large-screens){font-size: 35px}
}

h3 { font-size: 23px; 
	@include bp(large-screens){font-size: 29px}
}

h4 { font-size: 19px; 
	@include bp(large-screens){font-size: 24px}
}

.small-font { font-size: 13px; 
	@include bp(large-screens){font-size: 17px}
}

{% endhighlight %}

As displayed above, we must currently override each pixel declaration for every breakpoint (the above only shows the large-screen breakpoint, but in most cases there would be at least 3 more of these). We will touch further on how the relative nature of Ems and Rems can save time and stress for the Front-End Developer later in the post.

Pixels also have problems with accessibility.  Directly from the [Treehouse video](https://teamtreehouse.com/library/web-typography/basic-web-typography/fontsizing-px-em-and-rem) on typography, “When font-sizes are set in pixels, users cannot increase or decrease the text-size on a page through the browser preferences.” Therefore, the use of pixels does not allow users with visual impairments to alter font-sizes to their needs.  Because we are moving towards making our websites more accessible, this furthers our need to move to a more modern approach.

Let's take a look at how Em's can eliminate massive amounts of code

<h4>Em's</h4>

Ems are relative to their parent’s base size.  1 Em is equal to the current-font size.  If a parent is not set, then 1 Em is equal to the standard browser font-size of 16px.  But wait, if Ems are converted to px units, then why take the extra step?  Because Ems and Rems are to pixels what Sass is to CSS, basically a preprocessor. Using relative fonts will save the developer time and many lines of code while also allowing for a more fluid responsive typography system.  Below is an example of how the previous pixel media queries would look with an Em approach.

{% highlight scss %}

html { font-size: 1em; 
	@include bp(large-screens){font-size: 1.25em}
}

h1 { font-size: 2.074em; }

h2 { font-size: 1.728em; }

h3 { font-size: 1.44em; }

h4 { font-size: 1.2em; }

.small-font { font-size: .833em; }

{% endhighlight %}

First off, don't worry about the precise/offbeat values.  There are great resources and tools such as [this one](http://pxtoem.com/) and [this one](http://type-scale.com/?size=16&scale=1.333&text=A%20Visual%20Type%20Scale&webfont=Libre+Baskerville&font-family=%27Libre%20Baskerville%27,%20serif&font-weight=400&font-family-headers=&font-weight-headers=inherit&background-color=white&font-color=%23333) that provide Em scales and increment ratios.  As you can see in the example above, we have eliminated 6 lines of code.  But let's do some calculation to see what we're really saving.  The above technique (thanks, Ben!) is a clean Sassy way of writing media queries.  The <code>@include bp(large-screens)</code> results in an actual 3 lines of CSS.  So we're up to 18 lines of code eliminated from our CSS file.  But what about the fact that we always have, at a minimum, 2 font families.  We've saved 36 lines of code.  What about the fact that we have, at a minimum 5 breakpoints (usually more).  We've saved about 150 lines of final CSS code.  Equally important, we saved the developer tons of time.

Aside from the fact that the Em technique saves time and file size, it also allows for a more uniform scale for our typography.  This should be well-thought out in the design/UI/UX phase with help from this [awesome tool](http://type-scale.com/?size=16&scale=1.333&text=A%20Visual%20Type%20Scale&webfont=Libre+Baskerville&font-family=%27Libre%20Baskerville%27,%20serif&font-weight=400&font-family-headers=&font-weight-headers=inherit&background-color=white&font-color=%23333).  This is just another example of how performance is just as much the designer's job as it is the developers (later article to come on this topic!)  Now, of course there will be times when we don't want our typography to scale exactly the same at different breakpoints, so there will still be need for overriding in these cases.

So let's look at the one downside of this technique known as "compounding Em's". Because Em's are relative to their container, nested elements compound on top of eachother. For example, an li within an li within a ul within an h4 (you get the picture), would end up being super tiny.  In steps the Rem's...

<h4>Rems</h4>

Rem's (Roote Em's) are relative to the root size set on the html (or 16px if no value is set).  Rem's are supported in IE9 and up, so we can use them for all of our new sites.  If we need to support IE8, we can easily set pixel fall-backs.  Because Rem's are relative to the root, they avoid the compounding issue that Em's run into.

<h3>Summary</h3>

<ul>
	<li>Pixels are fixed, simple, and easy to use, but lack accessibilty and are non-responsive</li>
	<li>Ems and Rems are relative, responsive, and accessible</li>
	<li>Ems can run into issues with compounding</li>
	<li>There are tons of awesome tools to optimize relative font-sizing techniques</li>
	<li>If "Coumpound Ems" becomes an issue, Rems can save the day</li>

</ul>

<h4>Questions to consider/discuss</h4>

<ul>
	<li>Is there any reason to continue using pixels?  Will using a uniform approach to typography make sense with the way we design sites.  If not, then the overrides within the relative technique might not save that much time or code.  Need to have a meeting between design/dev to discuss.</li>
	<li>When to use Ems over Rems?  I would like to also discuss this in the next meeting.  Chris Coyier points out an interesting approach <a href="https://css-tricks.com/rems-ems/">here</a>.  Check out the 'conclusion' section of Jeremy Church's <a href="https://j.eremy.net/confused-about-rem-and-em/">article</a>.</li>
</ul>

<h4>Furthur Reading</h4>

[Font Size Idea: px at the Root, rem for Components, em for Text Elements](https://css-tricks.com/rems-ems/) by Chris Coyier.

[Font Sizing with REM](http://snook.ca/archives/html_and_css/font-size-with-rem) by Jonathan Snook.

[Confused About Rem and Em?](https://j.eremy.net/confused-about-rem-and-em/) by Jeremy Church.

[Web Design Basics: Rem vs. Em vs. Px — Sizing Elements In CSS](https://www.futurehosting.com/blog/web-design-basics-rem-vs-em-vs-px-sizing-elements-in-css/) by Matthew Davis.

[What’s the Deal with Em and Rem?](https://codemyviews.com/blog/whats-the-deal-with-em-and-rem) by Code My Views.

[Good Stack Overflow Thread](http://stackoverflow.com/questions/29686099/media-queries-px-vs-em-vs-rem)

[Treehouse Vid] (https://teamtreehouse.com/library/web-typography/basic-web-typography/fontsizing-px-em-and-rem)


<h4>Helpful tools</h4>

[REM Checker](https://offroadcode.com/prototypes/rem-calculator/)

[Pixels to Ems](http://pxtoem.com/)

[Type Scale](http://type-scale.com/) - You can choose visual scales on which to base your typography. You can even import fonts and check them on the scale you are setting up.



