# Best Practices

A collection of best practices and wisdom nuggets, from and for the developers at Up&amp;Up.

 - [URLs](#urls)
 - [Local Dev](#local-dev)
 - [Deployment](#deployment)
 - [Other Notes](#other-notes)
 - [Todos](#todos)

## URLs

 - Live: http://docs.upupdev.net
 - Local: http://127.0.0.1:4000/

## Local Dev
 1. Clone the repo.
 2. run `gem install jekyll bundler` to install jekyll and bundler.
 3. run `bundle install` to install everything you need for this project
 4. run `bundle exec jekyll serve` to spin up a local version of the site at `http://127.0.0.1:4000/`

## Deployment

### Staging
 This is now hosted on Github Pages. To deploy, merge your changes into the master branch and push to master.

## Other Notes

### Adding a new article:

Articles live in the `_posts` directory. Copy one of those files, rename it and you are on your way (markdown formatting).

You don't need to install Jekyll in order to write an article. Just write an article in Markdown, commit it, and deploy it and it will show up on the live site. If you want to do dev, change css, etc, then you will want to install Jekyll.

## Todos
 - ...
