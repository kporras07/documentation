---
title: Locking Your Site
description: Learn how to keep your Drupal or WordPress site hidden from the public for development or updates.
tags: [services]
categories: []
---
There are occasions you are working on a site and want to keep your progress hidden from the world as you prepare to go live or make updates.



This can be done by putting a username and password on the environment similar to basic authentication on Apache. If a request for a resource on your environment is received and the site is private, the requesting client will have to supply the authentication credentials you set in order to access the site.

## Password Protect Your Site's Environments

1. Select the environment (e.g. Dev), and click **Security**.
2. Click **Locked**.
3. Choose a username and password, and click **Lock Environment**.

![Lock environment](/source/docs/assets/images/dashboard/lock-environment.png)
If other members of your team on the site need to access the site, they can view the authentication credentials when they log in to their accounts.
![Credentials](/source/docs/assets/images/dashboard/environment-access.png)
When your page refreshes, you will notice that the environment is now private. You can also see the credentials needed to access that environment.

You can set a different username and password for each environment. This is important if you only want the live site publicly viewable, while Dev and Test can be private as you work on your code and content.

To verify that everything is working correctly, visit the URL of the environment that you have made private. You should see an authentication form where you can enter the username and password for that environment to start your session.  
 ![Example of locked site](/source/docs/assets/images/auth-required.png)

 <div class="alert alert-info" role="alert">
<h3 class="info">Note</h3>
<p>The <a href="https://www.drupal.org/docs/8/core/modules/basic_auth">HTTP Basic Authentication</a> core module (Drupal 8) and <a href="https://www.drupal.org/project/basic_auth">Basic HTTP Authentication</a> contrib module (Drupal 7) conflict with <a href="/docs/security/#password-protect-your-site%27s-environments">Pantheon's Security tool</a> when both are enabled. We recommend using <a href="/docs/security/#password-protect-your-site%27s-environments">Pantheon's Security tool</a> or the module to restrict access to Pantheon environment URLs, not both. </p>
</div>

## Unlock a Site's Environment

When you are ready to make your environment public again, click **Public** on the Security page. This will clear the credentials you entered and make web accessible resources available without a basic authentication prompt.
