---
layout: post
title:  "Conditional Loading"
date:   2016-03-31
author: Ben Robertson
categories: general
---

One way we can improve front-end performance for users is by better implementing progressive enhancement. We are used to using `display:none` to hide content that we don't want to display on mobile, but often that content is still loaded, it's just not visible.

For somethings, it might not matter, but as we all know, sites that start out small usually don't end up that way. They get added it and built up and eventually what you have is way bigger than what you started with. All of a sudden your hiding a bunch of content that is still loading, just not visible.

So how can we handle this responsibly? I suggest we try implementing the [Ajax Include Pattern](https://github.com/filamentgroup/Ajax-Include-Pattern/) pioneered by the Filament Group.

In this article, I'll give an example of how to implement this pattern and give some thoughts for consideration on how this might affect our dev and design process.

## Ajax Include Pattern example

Consider a page like [this](http://www.uky.edu/academics/undergraduate/business/accounting-0) on the University of Kentucky Admissions. Look at the sidebar and see all that content, and then scale your browser down to about 767px wide and see how the sidebar changes on mobile. When I built this site, I hid the desktop sidebar, and included code for a mobile menu and mobile contact section. Depending on the breakpoint, these sections are hidden, but they still load. And, with phase 2 and phase 3 of this project, we are adding even more content to this sidebar.

Desktop Sidebar:

~~~
<div id="detail-sidebar">
	<div class="links">
		<ul>
			<li><a href="http://gatton.uky.edu/" target="_blank">Gatton College of Business &amp; Economics</a></li>
			<li><a target="_blank" href="http://www.uky.edu/Admission/admissions">Admissions</a></li>
			<li><a target="_blank" href="http://www.uky.edu/financialaid/">Financial Aid</a></li>
			<li><a target="_blank" href="http://www.uky.edu/Admission/application">Apply Now</a></li>
		</ul>
	</div>
	<div class="sidebar-gps">
		<ul>
		  <li><a class="open-share" href="" onclick="event.preventDefault();"><i class="glyphicon glyphicon-share"></i>Share</a>
				<ul class="share-hidden">
					<span class="st_facebook_large" displaytext="Facebook" st_processed="yes"><span style="text-decoration:none;color:#000000;display:inline-block;cursor:pointer;" class="stButton"><span class="stLarge" style="background-image: url(http://w.sharethis.com/images/facebook_32.png);"></span></span></span>
					<span class="st_twitter_large" displaytext="Tweet" st_processed="yes"><span style="text-decoration:none;color:#000000;display:inline-block;cursor:pointer;" class="stButton"><span class="stLarge" style="background-image: url(http://w.sharethis.com/images/twitter_32.png);"></span></span></span>
					<span class="st_linkedin_large" displaytext="LinkedIn" st_processed="yes"><span style="text-decoration:none;color:#000000;display:inline-block;cursor:pointer;" class="stButton"><span class="stLarge" style="background-image: url(http://w.sharethis.com/images/linkedin_32.png);"></span></span></span>
					<span class="st_pinterest_large" displaytext="Pinterest" st_processed="yes"><span style="text-decoration:none;color:#000000;display:inline-block;cursor:pointer;" class="stButton"><span class="stLarge" style="background-image: url(http://w.sharethis.com/images/pinterest_32.png);"></span></span></span>
					<span class="st_email_large" displaytext="Email" st_processed="yes"><span style="text-decoration:none;color:#000000;display:inline-block;cursor:pointer;" class="stButton"><span class="stLarge" style="background-image: url(http://w.sharethis.com/images/email_32.png);"></span></span></span>
				</ul>
			</li>
	</ul>
	</div><!--end sidebar-gps-->
	<div class="faculty">
		<div class="faculty-photo">
			<img src="http://www.uky.edu/academics/sites/www.uky.edu.academics/files/cody_grayson.jpg" alt="" width="300px">
		</div>
		<div class="faculty-info">
			<p><strong>Grayson Jenkins</strong></p>
			<p>Undergraduate Recruiter</p>
			<p>Gatton College of Business &amp; Economics</p>
			<a class="email" href="mailto:grayson.jenkins@uky.edu">grayson.jenkins@uky.edu</a>
		</div>
	</div>
	<div class="college-info">
		<p class="college">Gatton College of Business &amp; Economics</p>
		<p>550 S Limestone</p>
		<p>Lexington, KY 40526</p>
		<p></p>
		<p><strong>(859) 257-8936</strong></p>
	</div>
	<div class="tags">
		<ul>
			<li class="degree ba"><span class="tip" title="Bachelor of Arts ">BA</span></li>
		</ul>
	</div>
</div>
~~~

Mobile sidebar content:

~~~
<div id="detail-menu" class="">
	<div class="menu-title">
		<h4>Menu</h4>
		<svg class="icon arrow-down-select" height="30px" width="30px"><use xlink:href="#arrow-down-select"></use></svg>
	</div>
	<div class="links">
		<ul>
			<li><a href="http://gatton.uky.edu/" target="_blank">Gatton College of Business &amp; Economics</a></li>
			<li><a target="_blank" href="http://www.uky.edu/Admission/admissions">Admissions</a></li>
			<li><a target="_blank" href="http://www.uky.edu/financialaid/">Financial Aid</a></li>
			<li><a target="_blank" href="http://www.uky.edu/Admission/application">Apply Now</a></li>
		</ul>
	</div>
</div>

// Then, further down the page:

<div class="mobile-contact section">
	<h4>Contact</h4>
	<div class="faculty">
		<div class="faculty-photo">
			<img src="http://www.uky.edu/academics/sites/www.uky.edu.academics/files/cody_grayson.jpg" alt="" width="300px">
		</div>
		<div class="faculty-info">
			<p><strong>Grayson Jenkins</strong></p>
			<p>Undergraduate Recruiter</p>
			<p>Gatton College of Business &amp; Economics</p>
			<a class="email" href="mailto:grayson.jenkins@uky.edu">grayson.jenkins@uky.edu</a>
		</div>
	</div>
	<div class="college-info">
		<p class="college">Gatton College of Business &amp; Economics</p>
		<p>550 S Limestone</p>
		<p>Lexington, KY 40526</p>
		<p></p>
		<p><strong>(859) 257-8936</strong></p>
	</div>
</div>
~~~

Those blocks of code are loaded no matter what.

But using the Ajax include method, we could avoid loading duplicate content like this. In this example, we would probably change the way the page was structured, and include the sidebar as a view in Drupal. If our view was accessible at `college/business/accounting/desktop-sidebar`, here's how our desktop code would change:

Desktop:

~~~

<div id="detail-sidebar">
  <a href="college/business/accounting/desktop-sidebar" data-append="college/business/accounting/desktop-sidebar" data-target="#detail-sidebar" data-media="(min-width: 1024px)"></a>
</div>

~~~

See the data-media attribute? We can include a media query in there and only load the content when the viewport hits that breakpoint--either at page load or when the window is resized. Using this method, we can avoid loading 40 lines of basically duplicate html that includes an image and five elements that fire javascript events, and this is without the new additions that this sidebar is getting.

We could simply default to load the mobile content and then conditionally load the desktop sidebar depending on viewport width, or if we wanted to optimize even further, we would conditionally load the mobile menu as well.

## Flexibility

One of the great things about this pattern is its flexibility. The above example show how to conditionally load content based on media queries, but we could use a similar patter to load content based on interaction. How about a a view more link that isn't just hiding content, but actually lets the user decide if they want to load the content? We could use it in zip-downs, where we often "hide" *a lot* of content.

## Considerations

While I think it is great that this is possible, it can be tricky to implement in development without it being considered in the design process.

We probably need to think carefully about progressively enhancing our designs from mobile up to desktop. Designing a mobile experience *first*, and then gradually enhancing that experience depending on user interaction, network connectivity, and viewport width is a better solution than starting with desktop experience and defaulting to `display:none` for mobile.

An example might be a related posts feature on a blog. Say we are showing 4 related posts for desktop users, with the post title, a link to the post, and the featured image. For mobile users, we might just want to display the post titles and links, and save them the bandwidth of the images. With this model, we can progressively add the images depending on media query.

We need to think carefully about what content should be loaded automatically for mobile users, and give them a way to access all content as well. We don't want to make it impossible for them to find content, we just don't want to make them load it if it's not necessary for their purposes.

---
Resources:

 - [Ajax Include Pattern](https://github.com/filamentgroup/Ajax-Include-Pattern/)
 - [Demos](http://filamentgroup.github.io/Ajax-Include-Pattern/test/functional/)
 - [Blog post introducing the concept](https://www.filamentgroup.com/lab/ajax-includes-modular-content.html)
