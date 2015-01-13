---
layout: post
title: "How the Terminal Saved the Day"
description: "Brute force site design changes"
headline: "grep da gawd"
category: blog
comments: true
share: true
tags: [blog, linux, terminal]
---
So, there's that button up in the top left corner that, when clicked, expands the menu.  Currently it says ">."  It used to say "F."  I'm assuming the "F" stood for the last name of the person who created the current jekyll theme I'm using.  It was annoying me that it said "F."  So I resolved myself to change it.  However, there was a problem.  I didn't know how to do it.

This site is composed of a bunch of files.  These files structure how the final product you see is built.  I know almost nothing about those files.  The best I can do is read the files in a text editor, and intuit the purpose of those files from their filenames.  I don't know what a .css file is.  I've opened up some of those JavaScript files.  Those are not human-readable.

So I loaded up the site on Chromium, and clicked on "View Source" in the Tools menu.  Here, I had no idea what to look for.  Instead, I right clicked the button at the top left and selected "Inspect Element" from the context menu.  I got a couple of cryptic hints toward where I could find out how to customize it.  Ultimately, however, those threads didn't lead me to the payoff I wanted.

I used 'grep' in the terminal while inside the repository on the various strings I found in the source on the website, but never found exactly the thing I needed.  I even tried capturing the color of the button when the pointer hovers over it, and grep the hex code in the repo, but to no avail.  The last thing I tried was sort of my hail mary.  Knowing the one hint the button itself gave me, I threw up a prayer on the shell:

     {% raw %}
      grep -r 'F' *
     {% endraw %}

It returned over 600 lines, most of them garbage.  But shortly into the results, I found an intriguing line:

     {% raw %}
      assets/css/style.css:    content:"F"
     {% endraw %}

Sure enough, I edited the style.css file in vim, searched for that exact string, changed the "F" to ">," and the rest is history.

Oh, and I also figured out how to make post titles displayed on the home page no longer in all uppercase.  I'll call it a pleasant side effect of wallowing in the web page source and .css files for hours.
