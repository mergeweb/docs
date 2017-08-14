---
layout: post
title:  "Switching a site to SSL on Apache"
date:   2017-06-27
author: Josh Boland
categories: apache, ssl
---

##Purpose
The purpose of this article is to give an overview of how to switch a site over to using an SSL certificate. These steps are not the only way, and likely have room for improvement and revision.

##1: Generate the Certificate
Certificates can either be purchased from a Certificate Authority or be generated for free using a service like Let's Encrypt. We are in the process of automating the Let's Encrypt method using an ansible playbook. 

###1.a: With A CA
####Generate a Signing Request
Navigate to the folder where the certificate will be stored. For our configurations, this is typically:
```$bash
cd /etc/apache2/ssl/
```
But it can differ depending on the server configuration.

Inside this directory, generate a new certificate signing request. This can be done from the command line using openssl (installed on most systems).
As you do this, use the Fully Qualified Domain Name (FQDN) for your site to name the files (so you can identify them easily).
```$bash
openssl req -new -newkey rsa:2048 -nodes -keyout www.sitename.key -out www.sitename.csr
```

Openssl will guide you through the creation of a CSR. When prompted for the "Common Name" of the site, enter the fully qualified domain name for the site. 
This will be the domain you want users to use to access your site. (Ex: education.fsu.edu or www.upandup.agency) The www is important here.
Some cert authorities will allow you to generate a wildcard cert, which covers all subdomains, these are specified with a * for the subdomain path: *.upandup.agency.

####Get the Certificate from the CA
Now that you have your csr, you will need to provide this file to the Certificate authority from whence you are purchasing your certificate. Most services allow you either pasts the csr in a text field or upload the file.
Now, get that csr on the clibpoard. You can use scp to download the file or, just:

```$bash
cat www.sitename.csr
```

and then copy the terminal output.

The CA will take this file (and your money) and issue you with download instructions for 2 things:
   - A Site Certificate
   - A Chain Certificate

###1.b: Using Let's Encrypt
Lets Encrypt provides a server utility called certbot that installs certificates automatically.
It is their recommended way for installing and updating certificates. 
Since there are different certbot versions and installation instructions for each server OS and server software, it is best to follow their online installation guide: 
https://certbot.eff.org/.

Depending on your server software, you will install some type of plugin for certbot as well that allows for generation and installation of certs on that server.

Since certbot will auto-generate and install the certificate all in one go, we'll look at running certbot in the following section. 

##2: Get The Certificate on the Server

###2.a: Certificate From a CA
Now that you have your site certificate and chain certificate, you need to copy these files into the ssl directory on your server where you keep certificates.
On most of our installs, this is located at:
```$bash
cd /etc/apache2/ssl/
```

Follow any known naming patterns, naming the certificate file using the FQDN like you did when generating the key and the CSR. The extensions are not super important, but be sure you can remember what each file is.
For most of the existing certs:
- The Site certificate is called domainname.crt
- The Full chain cert is called domainname.cer

###2.b: Certificate From Lets Encrypt
With certbot installed, there are lots of different ways to generate a certificate. Often, there are routes to both generate install the certificate or to only generate it and leave installation up to the user.
Since our configurations rely on multiple vhosts, we will opt for the latter.

To get started, you can run:
```bash
sudo certbot --apache certonly
```

This should fire up the certbot prompt window that will walk you through the cert generation. 
You will need to specify which domains the cert will cover and complete one of the different authentication challenges.

The http-01 challenge is perhaps the most straightforward. It will attempt to create and access a file in the public web directory of the domain you are trying to certify.
For our installations, you should be able to point it to the /current directory of whatever site you need to verify.

When the auto-installer is done, it will indicate where the certificate has been created and stored. Make a note of this path, it will be needed when setting up the vhost file.


##3: Install the Certificate and Test
Now that your certificate is created and stored, it is time to install it in the proper vhost config.
Open up the config using vim or your text editor of choice from apache's sites enabled

Since this site is moving to SSL,  new vhost entry is needed:
```
<VirtualHost *:443>
  ServerName education.fsu.edu
  
  SSLEngine On
  SSLCertificateFile /etc/apache2/ssl/education.fsu.edu.crt
  SSLCertificateKeyFile /etc/apache2/ssl/education.fsu.edu.key
  SSLCACertificateFile /etc/apache2/ssl/education.fsu.edu.cer
 
  DirectoryIndex index.html index.php
  DocumentRoot /var/www/websites/education.fsu.edu/current
 
  LogLevel warn
  ErrorLog /var/www/logs/education.fsu.edu-error.log
</VirtualHost>
```

Your DirectoryIndex and DocumentRoot can be the same as an existing vhost on port 80 for the same domain. 
In this configuration, the most important thing to note is that we have specified directives for the site certificate, the key and the chain certificate.
Also, it is important to note that this vhost is picking up traffic on port 443 (*:443) which is the port used by SSL.
 
If you used certbot, you will only have one fullchain.pem file, you only need the SSLCertificateFile directive in the vhost: 
```
<VirtualHost *:443>
  ServerName education.fsu.edu
  
  SSLEngine On
  SSLCertificateFile /path/to/certbot/storage/domain.fullchain.pem
 
  DirectoryIndex index.html index.php
  DocumentRoot /var/www/websites/education.fsu.edu/current
 
  LogLevel warn
  ErrorLog /var/www/logs/education.fsu.edu-error.log
</VirtualHost>
```
 
Once this configuration is in place, run a config test (you may need to sudo)
```
apachectl configtest
```

If the configuration is OK, you can re-start the server.
```
sudo service apache2 restart
```

After re-starting, you can go check the site over https. This is a good time to perform any other testing and check for Mixed-Content or Insecure Request errors.
Look particularly for assets that may be requested explicitly over https. (Internal assets requested over http will be redirected, but it can be nice to update those too.)
 
##4: Set up Temporary Redirection
Now that the 443 vhost is in place, AND you have tested your site over https, can to issue redirects from the insecure vhost(s) to the secure one. 
Take a look at other configurations for guidance, sometimes a site with multiple aliases and vhosts can become confusing. But generally, the redirect looks something like this:
```
<VirtualHost *:80>
  ServerName  education.fsu.edu
  Redirect / https://education.fsu.edu/
</VirtualHost>
```

A few notes here:
- We reach the :443 vhost by redirecting to a https:// url.
- Ensure a trailing slash appears at the end of the path you are redirecting to.
- Since we are redirecting from :80, we can drop things like DirectoryIndex and DocumentRoot from this vhost.
- It is IMPORTANT that you use Redirect only without the "permanent" modifier. Setting up the directive in this fashion will issue 302 redirects.

After setting up redirects and doing a config test, you can re-start the server! You did it!

##5: Possibly issue 301 status for the redirects
After living with the configuration you have created for a while (which should allow you to ensure it is running smoothly) you can consider changing the redirect types from 302 to 301.
301 is permanent and is better for SEO purposes. Before making this decision, it would be wise to evaluate any analytics or web dashboard data to ensure there are not any 404 (or other) 
errors caused by redirects that are poised to become permananet. Whe you are ready, you can alter the redirect directives in the :80 vhosts to read:
```
Redirect permanent / https://yourdomain.com/
```