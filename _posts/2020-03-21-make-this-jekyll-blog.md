---
title: "Making a Self Served Blog with Jekyll Part: 1"
category: posts
date: 2020-03-20
excerpt: "An end-to-end tutorial (and journey)"
---

# Welcome to Jekyll!
Ah, "Welcome to Jekyll!"

This header greeted me every excruciating time I ran `jekyll new` to start over again. Coming from a place of total unfamiliarity with Jekyll, I must have seen this page a couple dozen times during troubleshooting.

To spare you the same trouble, I wrote up this quick guide that captures everything I learned during the creation of the very blog you see before you now!

## Jekyll

[Jekyll](https://jekyllrb.com/) is a static site generator which allows one to quickly generate a site using Markdown, Liquid, HTML & CSS. The way I look at it is: you use markdown to create pages and templatized components of your website quikcly and easily, and these markdown files are redeployed as HTML when you `serve` or `build` your site.[^1]

From my research, it seemed to the be leading standard for this type of deployment, however, there are some other options. [StaticGen](https://www.staticgen.com/) provides a list and ranking system for all of the known static site generators available.

One huge heads up before you get started: getting Jekyll to work on Windows is a little iffy, even just for developing your site, and you probably aren't going to be self-hosting your site from a windows server. [Jekyll's documentation provides some workarounds](https://jekyllrb.com/docs/installation/windows/), but I would highly recommend looking elsewhere if you're going to rely on Windows for hosting or developing your Jekyll site.

### A Brief Word on Github Pages
When I first got started with Jekyll, I used github pages to set up a site in under an hour, which was a great way to learn the absolute basics and become familiar with the files and folders of a Jekyll site. Eventually, I realized I wanted to self-host, or host my static site from a server over which I have full control. It takes more effort, but there's so much more to learn when you "DIY" like this. 

This guide will strictly focus on self-hosting, however, I highly suggest this [Jekyll on Github Pages](http://jmcglone.com/guides/github-pages/) guide to get started on a site you plan on hosting strictly with Github Pages.

## Setting up

For this stack, I opted to use a basic EC2 instance on AWS to take advantage of their Free Tier offering, which should be plenty for a static site like this. We'll be using nginx as our webserver in Ubuntu Server 18.04 to host the site. Jekyll will be used to generate the site itself as we build it, which also requires Ruby. And maybe this is TMI, but my development environment is Ubuntu Desktop and I use VSCode which is utterly fantastic.[^2]

### Self-Serving

Going the self-serve route, we'll need to decide how we'll be hosting our completed site. Like I mentioned, [I set up an EC2 instance in AWS](https://docs.aws.amazon.com/efs/latest/ug/gs-step-one-create-ec2-resources.html) running Ubuntu server 18.04 to act as the server for my live site. This is the server that visitors will connect to in search of our sweet blog posts.

## Setting up Jekyll

We'll need to follow the steps in this section for both our development environment *and* our production server, as both will need to have Jekyll installed to do their jobs!

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

For the next few sections, we'll work in our dev environment because we want to get our site working and ready for prime time before we expose it to the outside world - lest we look like charlatans in the eyes of our colleagues!

Okay, let's get started.

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

4. We'll get into these files a little later, for now let's get our site up and running. First, let's use the following command to "serve" our site in a dynamic way. Make sure you're in the directory of your_site and run:

    ```bash
    jekyll serve
    ```
    This should lock up your terminal while Jekyll dynamically serves your site. To view it, open a web browser on the same system and navigate to the following address in your address bar:
    ```bash
    http://127.0.0.1:4000
    ```
5. You'll see the example site show in your browser. Click around and soak in your success. You've earned it, champ.

    ![Your First Site](/assets/images/your_first_site.png)

6. Finally, to stop serving the site, send CTRL+C to the terminal.

## Your Second Site
Okay, so your first site wasn't a monumental achievement in aesthetics. It's cool, we all have to start somewhere.

I know, my first thought was also "Great, this looks like !#%@".

Fortunately, we'll tackle the aesthetics shortly, however let's start by fixing some of the titles and names, unless "Your Awesome Title" is what you're sticking with...

*Unfortunately*, the default starter site isn't the best way to learn how to do this. For this next exercise, we'll clone a demo Jekyll site from github

1. Navigate back to where you want your sites to be while we work
2. Clone [this demo site](http://hankquinlan.github.io./) courtesy of Jonathan McGlone [^3]
    ```bash
    git clone https://github.com/hankquinlan/hankquinlan.github.io.git
    ```
3. We can serve up this site the same way we did with our first one. Navigate into the folder and run
    ```bash
    jekyll serve
    ```
4. Navigate to the same `127.0.0.1:4000` address in your browser. This looks sweet, but unless your name is Hank Quinlan, it isn't going to work without some customization.

5. Keep serving your site for the next section.

## Customize - Pages
<img src="{{ "/assets/images/Your_Second_Site_Files.png" | absolute_url }}" hspace="20" align="right">
Let's start poking around in the some of these files and see what kind of alterations we can make. Like I mentioned, I'm using VSCode to modify the files, but you're welcome to use whatever you desire.

The site you cloned and are currently serving should have a directory structure similar to what is shown on the right.
<br>
<br>



Let's start by looking at `index.html`-->
<br>
<br>
1. It's pretty obvious what we want to change here, but first, look at the document's structure. Notice the first four lines are dedicated to some high level information. This is called [Front Matter](https://jekyllrb.com/docs/front-matter/), and you'll need to add and modify this part of the file often to make sure Jekyll does what you want it to.

    Everything in the rest of this file is basic HTML. Go ahead and personalize this file, swapping out Hank-related references to ones more relevant to you.

2. Once you're done, save the file. You're still serving the site right? Go back to your web browser and refresh/reconnect to `127.0.0.1:4000`. Voila! The changes should appear automatically - without you having to tell Jekyll to reload anything. This is why using `jekyll serve` can be so handy!

3. Now, look at the `about` folder and open `index.html` inside. Look familiar? Pay attention to that [Front Matter](https://jekyllrb.com/docs/front-matter/) again. Same as the root `index.html` file we just edited. 

    Notice where it says `layout: default`. This is the part of the front matter that tells Jekyll how to format the page based on layouts you define for your site. This saves developers time and effort, because they can just call the layout they want in a single line, rather than duplicate code everytime they want to reuse a format.

4. Customize the information in here to your liking. Save.
5. Take a look at the `cv` folder and modify the `index.html` file in the same fashion.

All right, looks like most of the page is personalized to us at this point, but there are still some Hank-references to be found...

## Customize - Layouts

The remaining Hank Quinlan references are in the footer where it says `email` and `github.com/hankquinlan`. How do we change these? 

Pay attention to the fact that the header and footer are consistent across all the different pages to which we navigate. This is a major clue that they are part of the Layout we're calling in all the pages: `Layout: Default`. So, we'll need to edit the Layout itself!

1. Go to the `_layouts` directory.

2. Open `default.html`. Here's the material that is dictating what sits in the header and footer of our pages marked with the `layout: default`. 

3. Swap out the values as you like down in the footer. Add, modify, or remove these links to your liking.

    Personally, I added my Bandcamp page:

    ```html
    <li><a href="https://bandcamp.ianterry.com">bandcamp</a></li>
    ```
4. Save and examine your site. You are still serving it, right?

5. Feel free to look at the `post.html` layout as well, though we won't be making any changes here.

## Posts

All right, now let's talk about posts. With Jekyll, there are three big things you need to know:

1. Posts go in the `_posts` directory
2. Posts are written in [Markdown](https://en.wikipedia.org/wiki/Markdown), with file extensions of either `.md` or `.markdown`
3. Posts *must* follow the folling naming convension for their files:
    ```bash
    YYYY-03-21-your-blog-post-name.md
    ```
Let's open the existing post in this folder.

1. Notice that this time, the Front Matter reads `layout: posts`. This tells Jekyll to use the posts.html layout in the layout folder instead of the default.html layout.
2. The Front Matter also contains a `title:`, which not only displays at the top of the post, but also in the tab of your web browser.
3. Finally, it's got a line for `date:` as well.

Instead of modifying this one, let's make our own.

1. Create a new post in the `_posts` directory.
2. Name it with the Jekyll post naming convention:

    ```md
    YYYY-MM-DD-My-First-Post.md
    ```

3. Open it up, add in the Front Matter, and then write up a cool blog post.

    ```md
    ---
    layout: posts
    title: This is my cool post
    date: 2020-03-21
    ---

    I've taken over this blog.
    Hank Quinlan has been terminated.
    Prepare for the influx of superior content.
    ```

4. Save and refresh your browser *you are still serving right?*

All right, our blog is totally filled with our own content and Hank Quinlan is ousted. Nice job.

## Aesthetics
Jekyll allows us to use CSS (and SASS) to stylize our site, so if you're familiar with CSS already, you already know how to customize your static page. 

1. Open the directory titled `css` and edit the file called `main.css`
2. We already see that a lot of starter work is in place. We can edit this file to change the background color, font size, etc. 
3. Change this up as you see fit. 
4. Here are some examples of what I did:
    ```css
    #Changed the background-color and color of my text
    body {
        background-color: #1e1e1e;
        color: #aeaeae;
    }
    #Changed my link color
    a {
    text-decoration: none;
    color: #d2d2d2;
    }
    # 
    ```

This is my blog after making those changes:
    ![After making my aesthetic improvements](/assets/images/My_Second_Blog_Customized.PNG)


[^1]: When you run `jekyll serve` in the directory of your jekyll site, it starts a service on port 4000 that you can use to look at your site as you configure and customize it. This cool feature dynamically updates changes to the site as you save them. `jekyll build` renders and publishes your HTML files to the _site directory, which is where your webserver points to show your site to visitors.

[^2]: A nod back to the limitations of Windows, my workstation is running W10, so I virtualize an Ubuntu Desktop environment for most of my development work, which is my suggested route if you're a Windows user and don't know how best to compensate.

[^3]: http://jmcglone.com/