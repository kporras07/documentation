---
title: Use the Command Line to Create a WordPress Site Using Terminus and WP-CLI
description: Learn how to install and use Terminus and WP-CLI to control a WordPress site on Pantheon.
tags: [devterminus]
categories: []
type: guide
permalink: docs/guides/:basename/
contributors:
  - bmackinney
  - calevans
date: 2/25/2015
---
Many developers feel more at home at the command line than they do using a GUI. Edit a text file, issue a command, and bang—you've completed your task. There's just something about doing it all from the command line that makes it a little more exciting.

Until recently, WordPress didn't have a great answer for developers who are most at home on the CLI.

WP-CLI is a tool used to manage a WordPress installation. However, don't think of it as a simple backup or search and replace tool. Yes, it can do those things, but it's so much more than that. This guide will walk you through creating and configuring a site using WP-CLI and Pantheon's own CLI, called Terminus.

## Before You Begin

Be sure that you:

- Are familiar with your operating system's command line.
- Are using a Unix-based system (Linux or Mac OS X). Windows commands may vary slightly.
- Have created a [Pantheon account](https://dashboard.pantheon.io/register). Pantheon accounts are always free for development.

## Install Terminus
To use WP-CLI to manage your WordPress powered site hosted on Pantheon, you first need to install Pantheon's command line tool, Terminus. Terminus gives you access to your Pantheon account and sites, and allows you to execute WP-CLI commands on those sites. To install terminus, follow the [basic installation instructions](https://github.com/pantheon-systems/cli/wiki/Installation).

Once installed, test it using the following command:

```
$ terminus art
```

![The Pantheon logo represented in ASCII art](/source/docs/assets/images/command-line-terminus-art.png)

If you see the Pantheon lightning fist, you'll know that terminus is installed properly.

## Log In to Pantheon

Now we need to tell terminus who you are. You can do that with the `auth:login` command:

```nohighlight
$ terminus auth:login --email=<email> --machine-token=<machine_token>
```

You'll need to enter your password. If you are scripting a process, create a [machine token](/docs/machine-tokens/). You'll know you have successfully authenticated when you see the Pantheon logo.

## Create Your Site

Open a browser and log in to your Pantheon Dashboard so you can see the progress being made on some of the commands.

Creating a site is a function of the Pantheon API, not WP-CLI; however, Terminus handles both for us.

```nohighlight

$ terminus upstream:list | grep "WordPress"
  e8fe8550-1ab9-4964-8838-2b9abdccf4bf   WordPress                                                                 vanilla      core          wordpress
$ terminus site:create cli-test "Terminus CLI Create" e8fe8550-1ab9-4964-8838-2b9abdccf4bf

[notice] Creating a new site...

```
To create a site with one command, you'll need:
- **Upstream ID:** an internal Pantheon UUID for the different systems that you can install. WordPress on Pantheon is `e8fe8550-1ab9-4964-8838-2b9abdccf4bf`. To see all products, `$ terminus upstream:list`.
- **Site Name:** A machine-readable name, that will become a part of your environments' URLs. `cli-test` will yield a Pantheon development environment URL of `http://dev-cli-test.pantheonsite.io`. This name will also be used in all terminus commands against the site, so it's a good idea to keep it short. The site name must be unique on Pantheon.
- **Label:** A human-readable name, used to label your site on the Pantheon Dashboard. Can contain capital letters and spaces.

The format for creating a site with a single command is:

```nohighlight
site:create [--org [ORG]] [--] <site_name> <label> <upstream_id>
```

For my test site, I used the following:  
**Upstream** = WordPress
**Site Name** = cli-test  
**Label** = Command Line Test

```nohighlight
$ terminus site:create YOUR-ORG-ID cli-test "Terminus CLI Create" e8fe8550-1ab9-4964-8838-2b9abdccf4bf
```
**Note:** Copying this command will fail, because the site name is now taken. Choose a different name for your test.

Just like when you create a site from your Dashboard, this will only take a few minutes. You will see a status bar as terminus creates your new WordPress installation. Once complete, you will be notified that you site is ready to go.

From Terminus, you can get to your Site Dashboard with `$ terminus dashboard <site>.<env>`

## Clone the Codebase and Dotenv
Run the following command within backticks to [create a local repository of your site's codebase](/docs/git):

```
terminus dashboard:view <site>.<env>
```

## Install WordPress

Now that WordPress code is there, it's time for step five of the "[Famous 5-minute  Install](http://codex.wordpress.org/Installing_WordPress#Famous_5-Minute_Install)". Steps 1-4 were completed for you by Pantheon, and you don't need anything but the command line to finish. There's even a `wp-config.php` already created and ready to use.

All you need to do now is populate the database and your site will be ready to use. Using Terminus and WP-CLI running on the server, use the `wp core install` command. For this to work, it's necessary that you understand the [wp-cli core install](http://wp-cli.org/commands/core/install/) command:

```nohighlight
terminus wp -- core install --url=http://dev-cli-test.pantheion.io --title="WP-CLI-Test" --admin_user=admin --admin_password=something_incredibly_secure --admin_email=your@emailaddress.tld
Success: WordPress installed successfully.
```

Now go to your Dev environment
```bash
$ open http://dev-cli-test.pantheonsite.io/wp-admin
```
Log in using the username and password you set in the `wp core install` command. You've got a speedy WordPress admin ready to start developing!

There's not much to see at this point since we've only just created the site. However, click **Visit Development Site**, and you'll see a WordPress install ready for you to start creating your site.

Return to the terminal, we don't need no stinking mouse.

## Prepare for Development

I use a few plugins on every site, but  [WP-CFM](https://github.com/forumone/wp-cfm) is the most important. It allows me to track configuration changes, export them to code, deploy them as code, and import the config to my database without disrupting the content coming into the **Live Environment**. For more information on using WP-CFM on Pantheon, please see our article on [WordPress Configuration Management](/docs/wp-cfm).
```nohighlight
$ terminus wp <site>.<env> -- plugin install wp-cfm --activate
```
Results in
```bash
Running wp plugin install wp-cfm --activate=1  on cli-test-dev
Installing WP-CFM (1.3.1)
Downloading install package from https://downloads.wordpress.org/plugin/wp-cfm.zip...
Using cached file '/srv/bindings/17e08f1b8e1e465faae9927bb3e20730/.wp-cli/cache/plugin/wp-cfm-1.3.1.zip'...
Unpacking the package...
Installing the plugin...
Plugin installed successfully.
Activating 'wp-cfm'...
Success: Plugin 'wp-cfm' activated.
```

Now I can [use WP-CFM](/docs/wp-cfm) to create a bundle that will track my configurations and export them to code.

If you have the **Site Dashboard** open, you'll see the 19 files with changes ready to commit in a yellow box. You can expand that to see which files changed and commit through the UI, or use `$ terminus env:diffstat` and `$ terminus env:commit [--message [MESSAGE]] [--] <site_env>`

```nohighlight
$ terminus env:diffstat <site>.<env>
+---------------------------------------------------------------------------------+-----------+--------+-----------+
| File                                                                            | Deletions | Status | Additions |
+---------------------------------------------------------------------------------+-----------+--------+-----------+
| wp-content/plugins/wp-cfm/languages/wpcfm.po                                    | 0         | A      | 79        |
| wp-content/plugins/wp-cfm/includes/class-wp-cli.php                             | 0         | A      | 79        |
| wp-content/plugins/wp-cfm/assets/js/multiple-select/jquery.multiple.select.js   | 0         | A      | 484       |
| wp-content/plugins/wp-cfm/README.md                                             | 0         | A      | 70        |
| wp-content/plugins/wp-cfm/assets/css/admin.css                                  | 0         | A      | 186       |
| wp-content/plugins/wp-cfm/assets/js/multiple-select/multiple-select.png         | -         | A      | -         |
| wp-content/plugins/wp-cfm/includes/class-readwrite.php                          | 0         | A      | 294       |
| wp-content/plugins/wp-cfm/assets/js/pretty-text-diff/diff_match_patch.js        | 0         | A      | 49        |
| wp-content/plugins/wp-cfm/includes/class-helper.php                             | 0         | A      | 133       |
| wp-content/plugins/wp-cfm/assets/js/multiple-select/multiple-select.css         | 0         | A      | 190       |
| wp-content/plugins/wp-cfm/readme.txt                                            | 0         | A      | 132       |
| wp-content/plugins/wp-cfm/assets/js/pretty-text-diff/jquery.pretty-text-diff.js | 0         | A      | 71        |
| wp-content/plugins/wp-cfm/assets/js/admin.js                                    | 0         | A      | 189       |
| wp-content/plugins/wp-cfm/templates/page-settings.php                           | 0         | A      | 122       |
| wp-content/plugins/wp-cfm/includes/integrations/custom-field-suite.php          | 0         | A      | 170       |
| wp-content/plugins/wp-cfm/wp-cfm.php                                            | 0         | A      | 182       |
| wp-content/plugins/wp-cfm/includes/class-options.php                            | 0         | A      | 42        |
| wp-content/plugins/wp-cfm/includes/class-ajax.php                               | 0         | A      | 86        |
| wp-content/plugins/wp-cfm/includes/class-registry.php                           | 0         | A      | 111       |
+---------------------------------------------------------------------------------+-----------+--------+-----------+
```
Only the expected changes are reported. Let's commit.
```nohighlight
$ terminus env:commit --message "Install wp-cfm plugin" dev
"Install wp-cfm plugin"
                            Commit 1 changes? [y/n] y
                            Success: Successfully commited.
                            +---------------------+-----------------+-----------+------------------------------------------+---------------------------------------------------+
                            | Time                | Author          | Labels    | Hash                                     | Message                                           |
                            +---------------------+-----------------+-----------+------------------------------------------+---------------------------------------------------+
                            | 2015-02-12T22:22:45 | Brian MacKinney | dev       | a6ea17f55d6022bb4d9ad4c9deacc79c1b4e29c7 | Install wp-cfm plugin
```


## Customize Your Site
Now that you have a stock WordPress install, let's make it look a little better. WP-CLI can do a number of things to manipulate a WordPress site. The best sites are the ones with images, but sadly the stock WordPress installation doesn't come with any. Let's add some.

### Add Images
Using WP-CLI gives us the ability to upload images and modify posts. In this case, you can do both at once. The following command will add a featured image to the Hello Word post that WordPress installs automatically.

You can see the full documentation for `media import` on the [wp-cli media import documentation page](http://wp-cli.org/commands/media/import/). Since you're using the `--featured_image` flag, you also need to pass the `post_id`. You can pass in either the URL of an image or a local filename to `media import.` Understand that in this case, "local" means it's already uploaded to your site. Our command to upload an image and set it as the featured image of post #1 looks like this:
```
$ terminus wp <site>.<env> -- media import https://farm8.staticflickr.com/7355/16204225167_1e1bb198e5_b.jpg \
           --post_id=\"1\" \  
           --featured_image  \  
```

After a successful upload you'll see this message:

```
Success: Imported file https://farm8.staticflickr.com/7552/15827270506_ce62e709c9_o_d.jpg
as attachment ID 3 and attached to post 1 as featured image.
```
You can see the full documentation for `media import` on the [wp-cli media import documentation page](http://wp-cli.org/commands/media/import/). Since you're using the `--featured_image` flag, you also need to pass the `post_id`. You can pass in either the URL of an image or a local filename to `media import`. In this case, "local" means it's already uploaded to your site.

Go to your browser and refresh your WordPress website's front page to see the new image.

### Install and Add a New Theme

So far, so good. Many will stop there and start updating the content. There is one more thing we need to do to make this site more appealing, and that is to add a better looking theme. Once again, you can do this all without touching WordPress or your Pantheon Dashboard.

WordPress has a plethora of free and paid themes you can install to customize your site. We've chosen one from the [WordPress.org Themes Repository](https://WordPress.org/themes/) named [Pinboard](https://WordPress.org/themes/pinboard).
**Note:** There is no need to download the theme first, WP-CLI will pull it for us from WordPress.org.

Position your Pantheon Dashboard window where you can see it while working in the terminal. To install and activate the new theme on your site, use the following command:


```nohighlight
$ terminus wp <site>.<env> -- theme install pinboard --activate
```

Watch your Dashboard. It recognizes your uncommitted changes.

![Screenshot of the pantheon dashboard showing uncommitted changes](/source/docs/assets/images/dashboard/pantheon-dashboard-uncommitted-changes.png)

We can commit the changes to your site's repo with Terminus. First, make sure that you position your browser so that you can see it while in your terminal. As soon as you issue the command, you'll see everything update in the browser.

```nohighlight
$ terminus env:commit --message "Install wp-cfm plugin" dev
```

Terminus connects to Pantheon's API, which makes real-time updates to any Dashboard you have open. What you do in Terminus is immediately represented in your Dashboard, so it is always up to date.

Now that we've committed our changes, go back to your test site in the browser and refresh it to see what you've created.

### Theming Best Practices

No WordPress site is ready for development without a child theme. Let's create one:

```nohighlight
$ terminus wp <site>.<env> -- scaffold child-theme --parent-theme=pinboard --theme-name=cli-test-theme
```
Next, we'll commit it.

```nohighlight
$ terminus $ terminus env:commit --message "Create cli-test-theme child of pinboard theme" dev
```

Now you're ready to edit the cli-test theme, allowing for upstream theme improvements in the pinboard theme to happen without interfering with the functionality of your site.

![Screenshot of the final website created following the steps in this guide](/source/docs/assets/images/pantheon-final-command-line-test-site.png)


## Importing Content from a WXR File

With the `wp import` command, you can import content from another WordPress site that you've exported to a WXR file. It is important to note that the command runs on the app server, so place the file somewhere in the wp-content directory before running the command. You cannot import a file from your local machine. For more information, see the documentation for `wp import` on the [wp-cli import documentation page](http://wp-cli.org/commands/media/import/).


## The Power of Terminus and WP-CLI

If you're a developer who lives in the command line, you now see the power of Terminus and WP-CLI. This guide has just scratched the surface of what can be done. Terminus provides the power to manage most aspects of your Pantheon sites, while tools like WP-CLI (and drush for Drupal) give you the power to manage the inner workings of your WordPress powered site. Now you're ready to take the sandbox site we've setup and explore on your own to see what else is possible.
