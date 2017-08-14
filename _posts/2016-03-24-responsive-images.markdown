---
layout: post
title:  "Responsive Images"
date:   2016-03-24
author: Ben Robertson
categories: general
---

It's 2016 and we are going to level up our responsive images. We have previously implemented responsive images with the good ole  `max-width: 100%; height: auto;` trick, but the problem with that is that even though we are displaying smaller images on smaller devices, we are still sending them the big one. This means slower page load on mobile, more expensive pages for our users, and overall frustration on mobile.

The trick to performant responsive images is letting the browser decide which version of an image to serve to a user. We can do this with some enhancements to the `<img>` tag or the new `<picture>` tag. Let's start with the `<img>` tag since we will be using that more frequently.

## Upgrade Your `<img>` Tags

Here's what a typical image tag looks like on one of our sites:

`<img class="some-class" src="path/to/image.jpg" alt="a picture of bill murray">`

To make these images more performant, we need to send more information to the browser using two new `<img>` attributes: `srcset` and `sizes`.

### srcset

The first new attribute we need to utilize is the `srcset` attribute. The `srcset` attribute tells the browser about different versions (ie different `src`'s) of the image that are available. Here's an example of an image that includes `srcset`:

~~~
<img srcset="/public/img/resp-img/shoes-large.jpg 1024w,
              /public/img/resp-img/shoes-medium.jpg 300w,
              /public/img/resp-img/shoes-small.jpg 150w"
      src="/public/img/resp-img/shoes-small.jpg"
      alt="a picture of a shoe">
~~~

So for this image, we tell the browser that there are three versions of the image available. To the right of the src path, we tell the browser how wide the image is. So image-large.jpg is 1440 pixels wide (you can also use 3x, or 2x for pixel density changes).

With this information in hand, the browser can start to figure out what version of the image is most appropriate in the current context.

Here's how that code actually renders:

<img srcset="/public/img/resp-img/shoes-large.jpg 1024w,
              /public/img/resp-img/shoes-medium.jpg 300w,
              /public/img/resp-img/shoes-small.jpg 150w"
      src="/public/img/resp-img/shoes-small.jpg"
      alt="a picture of a shoe">

### sizes

The browser can use all the help it can get however, and that's where the `sizes` attribute comes in. `sizes` attribute tells the browser how much space the image is supposed to take up in our design.

Here's our example from above with the sizes attribute added in:

~~~
<img srcset="/public/img/resp-img/shoes-large.jpg 1024w,
              /public/img/resp-img/shoes-medium.jpg 300w,
              /public/img/resp-img/shoes-small.jpg 150w"
     sizes="(min-width: 50em) 1024px,
            (min-width: 35em) 300px,
            150px"
    src="/public/img/resp-img/shoes-small.jpg"
    alt="a picture of a shoe">
~~~

The first two entries are media queries. The first entry says that when the viewport has a min-width of 50ems, the image will always be 1024px wide.

The second entry says that if the viewport has a min-width of 35ems, the image will be 300px wide.

The last entry says that the image will be 150px for viewports under 35em wide.

Here's the actual image below. If you are using Chrome, you will see a different sized image as you resize your browser window. In safari, you have to reload the page at each breakpoint to get the new size:

<img srcset="/public/img/resp-img/shoes-large.jpg 1024w,
              /public/img/resp-img/shoes-medium.jpg 300w,
              /public/img/resp-img/shoes-small.jpg 150w"
    sizes="(min-width: 50em) 1024px,
           (min-width: 35em) 300px,
           150px"
    src="/public/img/resp-img/shoes-small.jpg"
    alt="a picture of a shoe">

### `<img> summary`

Basically what we are doing with these two attributes is giving the browser insight into our CSS so that it can make decisions about what image will be the best for the user. We can serve images that are 10kb to users on mobile and images that are larger (in this case 124kb).

## The Bigger `<picture>`

Just to reiterate, `srcset` should be used when you are simply serving different sizes of the same image. It works best when your aspect ratio and content stay the same but the actual dimensions change.

But what if you want to serve different images? For example, you might need different crops of an image at different viewports. Square for mobile, large rectangles for desktop. If that's the case, you should use the `<picture>` element.

The `<picture>` element is a whole new HTML element, and it functions very similar to the `<video>` element. Here's the syntax:

~~~
<picture>
  <source srcset="images/still_life-large.jpg" media="(min-width: 1000px)">
  <source srcset="images/still_life-small.jpg" media="(max-width: 500px)">
  <img src="images_src/still_life.jpg" alt="still life">
</picture>
~~~

You can see we are still making use of the `srcset` attribute, however, each is wrapped in it's own `<source>` tag. This example is very similar to our `<img>` tag above, because it is simple. But inside the `<picture>` tag, you can do things like serve completely different source types (ie SVG vs PNG).

The `<picture>` element is also a good choice if you want granular control of the image. The browser will do exactly as you say with the `<picture>` element, but with the `<img>` tag it will do a little thinking for you. In most cases, `<img>` should be fine, and will likely even improve over time with features like the Network API coming down the line. Eventually, browsers could detect connection speed and send the most appropriate images in that context.

## Browser Support

As of this writing, `srcset` and `sizes` are supported in Microsoft Edge, Chrome, Firefox, Safari and iOS Safari.

http://caniuse.com/#feat=srcset

If you need to ensure Internet Explorer support, use the [Picturefill library](http://scottjehl.github.io/picturefill/) polyfill. It weighs 5.8kb and will more than make up for itself in peformance savings for serving smaller images.
