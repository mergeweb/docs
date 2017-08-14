---
layout: post
title:  "Documentation Template"
date:   2016-09-28
author: Ben Robertson
categories: general
---

<em>Updated 2016-11-18 by Ben.</em>

Use this to start off your github readme for new projects:

```

# {Site Name}

{A description of the project here, if necessary.}

 - [URLs](#urls)
 - [Local Dev](#local-dev)
 - [Deployment](#deployment)
 - [Important Notes](#important-notes)
 - [Todos](#todos)

## URLs

 - Live: { https://www.upandup.agency }
 - Staging: { http://upup.upupdev.net }
 - Local: { https://lc.upup.com }

 - Live server: {server name}

## Local Dev
{This should include all the steps necessary to take you from 0 to a working local environment. Sample steps are included below:}
 1. Clone the repo into /var/www/websites/upup/app
 2. Grab the database from Luigi or the site and import it locally.
 3. {Include steps to compile `scss` and `js` if necessary.}

## Updating
{Include special instructions for updating CMS, plugins and modules, so anyone can do it.}

{Wordpress boilerplate:}

You can either update Wordpress and plugins in the wordpress backend on your local or you can use the `wp-cli`.
 - In your vagrant directory, `vagrant ssh`, then `cd /var/www/websites/{sitename}/app`
 - To update Wordpress core: `wp core update`
 - For plugins:
  - see available updates with `wp plugin list`
  - update plugins with `wp plugin update {plugin-name}`

{Drupal 8 boilerplate:}

- Updating: `composer update`
- Install new module `composer require drupal/modulename:~8.0`
- Remove module `composer remove drupal/modulename`

{Drupal 7 boilerplate:}

The best way to update Drupal 7 projects is with `drush`.

If you have `drush` installed on your vagrant box, you can `cd /var/www/websites/post/app` and then run `drush up --security-only`. Drush will check all modules, themes, and core for security updates, and perform the updates for you.

## Deployment
{Include instructions on deploying to staging and production environments.}
## Deployment
 - **Staging**: Merge your changes into the testing branch and `cap staging deploy`.
 - **Production**: Merge your changes into the master branch and `cap prod deploy`.

## Important Notes
 {You may want to include other info that may be unique to this project or framework. For instance, how to install new modules in a Drupal project managed by composer.}

## Todos
{A list of items that could be done in the codebase to make the site better for people who work on it. Not for client requests, but you could make a note about some code that needs to be refactored here.}

```
