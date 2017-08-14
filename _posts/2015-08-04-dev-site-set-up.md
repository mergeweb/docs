---
layout: post
title:  "Setting Up a Fresh Dev Site"
date:   2015-08-04
author: Ben Robertson
categories: general
---

##Step 1: Create the repo

Sign into our github account, create the repo (name the repo something short but easy to identify, we will be using this name for a lot of things) and then be sure to add anyone who will be working on the project as a collaborator. That's usually the whole dev team.

## Step 2: Add the site to the Vagrant config.yaml

Open up terminal and cd to your vagrant folder. Once there, checkout the master branch. Cd to <code>puphpet/</code>, and open up the config.yaml in your text editor.

Find the <code>vhosts:</code> section of the file and add a new entry where your new site fits alphabetically with the other entries. For example, I'm adding the UK College of Pharmacy site, and the repo name is 'cop.' I'll place my new entry in between clemsonvip and dhg. Here's what the entry should look like:

{% highlight yaml %}
cop:
    servername: lc.cop.com
    docroot: /var/www/websites/cop/app
    port: '80'
    override:
        - All
    engine: php
    custom_fragment: ''
{% endhighlight %}

You should replace every instance of 'cop' with your repo name.

Next - still in your <code>config.yaml</code> scroll down to the mysql/databases section. It's also organized alphabetically, and you're going to add an entry that looks like this:

{% highlight yaml %}
cop:
    name: cop
    sql_file: '/var/www/databases/vagrant/cop.sql'
    user: ******
    password: *******
    host: localhost
    grant:
        - ALL
{% endhighlight %}

As before, replace all instances of cop with your repo name and enter the username and password you'll use for the database.

Commit your changes, push them, and then merge them into your local branch. Run <code>vagrant provision</code> in your terminal. Then go get a snack while you wait for vagrant to do its thing.

##Step 3: Add databases

In Sequel Pro, or your database program of choice, login to Luigi and add a new database with the same name as your repo. Do the same for your localhost.

##Step 4: Add site to your hosts file

In your terminal, do this: <code>sudo vim /etc/hosts</code> and enter your password. This will open up your hosts file in the terminal editor, VIM. We're going to add a new entry to this file for our new site. Following the College of Pharmacy example from above, here's what your new line should look like:

<code>192.168.0.100 lc.cop.com</code>

Once you've add the line, type <code>:wp</code> to exit and save the file.

Navigate to the url you entered - in this case lc.cop.com - and if you see a page that says file not found you're doing great!

##Step 5: Dev Set Up

There's a few things we need to do to get the dev environment set up.

We're going to <code>cd /var/www/websites/cop/app</code>, then init a new git repo: <code>git init</code>.

What happens next depends on the site you're starting. For a Drupal or Wordpress install, you'll move a fresh copy of the lastest version into this directory.

No matter what it is you're doing, you'll definitely have to do these things:

+ Add a .gitignore (it's usually easiest to copy one from a similar project)
+ In your app directory, run <code>vagrant init</code>
+ In your app directory, run <code>capify .</code>
	+ this creates a config folder and a config/deploy file. Delete everything in that file and replace it with this:

	{% highlight ruby %}
		set :stages, %w(staging prod)
		set :default_stage, "staging"
		require 'capistrano/ext/multistage'
	{% endhighlight %}

+ within the config directory, create a deploy directory and a staging.rb file within it. Find a staging.rb file from a similar project and copy it's contents into this file. Update the site specific sections with your new repo's info.
+ add the compass config.rb file/structure from a similar project

At this point, you'll want to go through the install process for Wordpress or Drupal, if you're using one of those for your project. Just navigate to your local (lc.yoursite.com) and you should get the installation prompt.

##Step 5: Initial Commit

Now it's time for the initial commit! After you've committed, you'll run the following commands to connect your repo to the remote (replace cop with the name of your repo):

<code>
git remote add origin https://github.com/mergeweb/cop.git
</code>

<code>
git push -u origin master
</code>

##Step 6: Add the dev site to the server

We need to add the site to the server's 'sites availables' folder so that we can deploy the site.

SSH into the server where the dev site will live and <code>cd /etc/apache2/sites-available</code>.  Once in the sites available folder copy one of the existing urls and rename it as your new url.  Replace all older urls with the new url you want.

To finalize this step, you must run the following two commands:

1:  <code>sudo a2ensite yoursite.upupdev.net</code> (of course replace the fake url with your new url)
2:  <code>sudo service apache2 reload</code>

Your site should now load and you can now deploy to it!

##Step 7: Take a Deep Breath

You're about to start a new project. Take a deep breath. Think about all the things you've learned since the last project you started and plan how you might want to implement one or two new things that will fit well into this project. When you're ready -- get started!
