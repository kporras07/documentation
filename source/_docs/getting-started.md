---
title: Getting Started
description: Learn how to create a Drupal or WordPress site on Pantheon.
tags: [getstarted]
categories: []
---
<img src="/source/docs/assets/images/icon-launch-k.svg" alt="Launch Icon" style="float:left;margin-left:0px;margin-right:20px;margin-top:15px;margin-bottom:0px;border:0;max-height:80px;"><p style="padding-top:20px;margin-right:0px;">Welcome to Pantheon, the world’s fastest hosting for Drupal and WordPress. In a few simple steps, your site will be up and running on our distributed infrastructure with no single points of failure.</p>
## Create Your Free Account
If you haven't already done so, create your [free Pantheon account](https://pantheon.io/register). Agencies (even freelancers) who build websites for third parties can [sign up for Pantheon for Agencies](https://pantheon.io/agencies/pantheon-for-agencies).
## Create a New Site
<img src="/source/docs/assets/images/icon-developer-dashboard.svg" alt="Launch Icon" style="float:right;margin:5px;border:0;max-height:80px;"><p style="padding-top:0px;">From your Dashboard, click <strong>Create a New Site</strong> or visit <a href="https://dashboard.pantheon.io/sites/create" target="blank">https://dashboard.pantheon.io/sites/create</a>. We allocate two free Sandbox sites for all user accounts. If you have reached your limit of Sandbox sites, delete an unused site or take a site live. Pantheon for Agencies Organizations do not have the two-sandbox restriction for sites associated with their organization.</p>

1. **Choose your site type**.  
To start development directly on Pantheon, select a start state from our available distributions. For details, see [Choosing Your Start State](/docs/start-state).

2. **Visit the Dev installation**.  
On the Site Dashboard, click **Visit Development Site** to complete the Drupal or WordPress installation process.

## Migrate an Existing Site to Pantheon

If you have an existing Drupal or WordPress site that you want to migrate to Pantheon, choose <strong>Migrate Existing Site</strong>. For details, see <a href="/docs/migrate">Migrate Sites to Pantheon</a>.</p>

## Interact With Your Code
<img src="/source/docs/assets/images/icon-tools.png" alt="Tools Icon" style="float:left;margin-right:15px;margin-top:10px;margin-bottom:0px;border:0;max-height:80px;"><p style="padding-top:5px;padding-bottom:0px;">

There are two ways to add and update your site's code: Git source code management and SFTP.</p>

### Git Connection Mode
Git is a well-known, industry standard version control system for managing code. For more information, see [Starting with Git](/docs/git).

We recommend enabling passwordless access to the site's codebase for Git by [loading an SSH key](/docs/ssh-keys) into the [User Dashboard](https://dashboard.pantheon.io/users/#account). For detailed instructions, see [Generating and Adding SSH Keys](/docs/ssh-keys). Users without an SSH key loaded on their account can interact with code via Git by entering their Dashboard password when prompted in terminal.

### SFTP Connection Mode
You can develop directly on the server using [SFTP mode](/docs/sftp/).

## Pantheon Credentials
Your [Pantheon Dashboard](https://dashboard.pantheon.io) credentials are used to manage sites, organizations, and support requests. Users on your Pantheon hosted sites will be distinct and are not managed by Pantheon; there is no single sign-on between the Dashboard and Drupal or WordPress site.

Find more Pantheon provided credentials in the site Dashboard's connection information. For more details, see:

- [Developing on Pantheon Directly with SFTP Mode](/docs/sftp#sftp-connection-information)
- [Starting with Git](/docs/git/)
- [Accessing MySQL Databases](/docs/mysql-access/)
- [Redis as a Caching Backend](/docs/redis#use-the-redis-command-line-client)
- [Generating and Adding SSH Keys](/docs/ssh-keys/)
## The Pantheon Workflow
Learn more about the [Pantheon workflow](/docs/pantheon-workflow/), both for code deployment and content (database and files).

## Known Limitations
Familiarize yourself with Pantheon's [platform considerations](/docs/platform-considerations) now to reduce headaches later.

## Prepare for Launch
Ensure a successful launch by following our best practice checklist for [Going Live](/docs/going-live) on Pantheon.

## The Pantheon Platform
Read [All About Application Containers](/docs/application-containers/) for a better understanding of the Pantheon architecture.

## Essential Reading
<img src="/source/docs/assets/images/icon-document-hand.png" alt="Tools Icon" style="float:left;margin-right:15px;margin-right:15px;margin-top:5px;margin-bottom:0px;border:0;max-height:80px;"><p style="padding-top:10px;padding-bottom:20px;">
If you're ready to move beyond the basics, check out our curated list of <a href="/docs/required-reading">Required Reading: Essential Pantheon Documentation</a>.</p>

## See Also
- [Frequently Asked Questions](/docs/faq)
