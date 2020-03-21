---
title: "Making a Self Served Blog with Jekyll"
category: posts
date: 2020-03-20
excerpt: "An end-to-end tutorial (and journey)"
---

# Welcome to Jekyll!
Ah, "Welcome to Jekyll!"

This header greeted me every excruciating time I ran `jekyll new` to start over again. Coming from a place of total unfamiliarity with Jekyll, I must have seen this page a couple dozen times during troubleshooting.

To spare you the same trouble, I wrote up this quick guide that captures everything I learned during the creation of the very blog you see before you now!

# Jekyll

[Jekyll](https://jekyllrb.com/) is a static site generator which allows one to quickly generate a site using Markdown, Liquid, HTML & CSS. The way I look at it is: you use markdown to create pages and templatized components of your website quikcly and easily, and these markdown files are redeployed as HTML when you `serve` or `build` your site.[^1]

From my research, it seemed to the be leading standard for this type of deployment, however, there are some other options. [StaticGen](https://www.staticgen.com/) provides a list and ranking system for all of the known static site generators available.

One huge heads up before you get started: getting Jekyll to work on Windows is a little iffy, even just for developing your site, and you probably aren't going to be self-hosting your site from a windows server. [Jekyll's documentation provides some workarounds](https://jekyllrb.com/docs/installation/windows/), but I would highly recommend looking elsewhere if you're going to rely on Windows for hosting or developing your Jekyll site.

# A Word on Github Pages
When I first got started with Jekyll, I used github pages to set up a site in under an hour, which was a great way to learn the absolute basics and become familiar with the files and folders of a Jekyll site. Eventually, I realized I wanted to self-host, or host my static site from a server over which I have full control. It takes more effort, but there's so much more to learn when you "DIY" like this. 

This guide will strictly focus on self-hosting, however, I highly suggest this [Jekyll on Github Pages](http://jmcglone.com/guides/github-pages/) guide to get started on a site you plan on hosting strictly with Github Pages.

## Setting up

For this stack, I opted to use a basic EC2 instance on AWS to take advantage of their Free Tier offering, which should be plenty for a static site like this. We'll be using nginx as our webserver in Ubuntu Server 18.04 to host the site. Jekyll will be used to generate the site itself as we build it, which also requires Ruby. And maybe this is TMI, but my development environment is Ubuntu Desktop and I use VSCode which is utterly fantastic.[^2]

### Self-Serving

Going the self-serve route, we'll need to decide how we'll be hosting our completed site. Like I mentioned, I [set up an EC2 instance in AWS](https://docs.aws.amazon.com/efs/latest/ug/gs-step-one-create-ec2-resources.html) running Ubuntu server 18.04 to act as the server for my live site. This is the server that visitors will connect to in search of our sweet blog posts.

## Setting up Jekyll

We'll need to follow the next few steps in both our development environment *and* our server, as both will need to have Jekyll installed to do their jobs!

1. To get started, let's make sure our respective Ubuntu environments are up to date:
    ```bash
    sudo apt update && sudo apt-get update && sudo apt-get upgrade
    ```
2. Install Ruby stuff

    ```bash 
    sudo apt-get install ruby-full build-essential zlib1g-dev
    ```

3. Installing Ruby Gems as a root user isn't recommended, so the following commands will set up a gem installation directory of your user account by adding environment variables to your ~/.bashrc file. You can just copy and paste these one by one into your terminal if you're not setting up as `root`
    
    ```bash
    echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc`
    echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc`
    echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc`
    source ~/.bashrc`
    ```

4. Now just install Jekyll.
    
    ```bash
    gem install jekyll bundler
    ```
5. Check that you've successfully installed jekyll. This should return Jekyll 4.0.0 or something like it.

    ```bash
    jekyll -v
    ```
Now you should be all set to start working on your site!

## Your First Site
Jekyll is cool because it can give us a functional site in a matter of minutes, which we can use as a starting point to build out a site of which we can be really proud! 

Now, we should be working in our dev environment for a little while, because we want to get our site working and ready for prime time before we expose it to the outside world, lest we look like charlatans in the eyes of our colleagues.

Okay, let's get started!

1. Navigate to an area you want your sites to live while we work on them. 

2. Create a new jekyll site. The string you specify as your_site will be the name of the folder created by Jekyll.

    ```bash
    jekyll new ./your_site
    ```
3. Navigate into this folder and look around, you should see the basic "scaffolding" for a site, which is actually a 100% functional example site!

    ```bash
    cd ./your_site
    ls -l

    404.html
    about.markdown
    _config.yml
    Gemfile
    Gemfile.lock
    index.markdown
    _posts
    ```

4. We'll get into these files a little later, for now let's get our site up and running. First, let's use the following command to "serve" our site in a dynamic way. Make sure you're in the directory of your_site and run 

    ```bash
    jekyll serve
    ```
    This should lock up your terminal while Jekyll dynamically serves your site. To view it, open a web browser on the same system and navigate to the following address in your address bar:
    ```bash
    http://127.0.0.1:4000
    ```
5. You'll see the example site show in your browser. Click around and soak in your success, you've earned it champ.

![Your First Site](/assets/images/your_first_site.png)

## Make It Yours
Okay, so your first site isn't a monumental achievement in aesthetics. It's cool, we all have to start somewhere.

I know, my first thought was also "Great, this looks like !#%@".

Don't worry, we'll tackle the aesthetics shortly, however let's start by fixing some of the titles and names, unless "Your Awesome Title" is what you're sticking with...


## Leveling up

[^1]: When you run `jekyll serve` in the directory of your jekyll site, it starts a service on port 4000 that you can use to look at your site as you configure and customize it. This cool feature dynamically updates changes to the site as you save them. `jekyll build` renders and publishes your HTML files to the _site directory, which is where your webserver points to show your site to visitors.

[^2]: A nod back to the limitations of Windows, my workstation is running W10, so I virtualize an Ubuntu Desktop environment for most of my development work, which is my suggested route if you're a Windows user and don't know how best to compensate.