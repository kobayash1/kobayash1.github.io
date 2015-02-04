---
layout: post
title: "Printing Demons"
description: "Computer troubleshooting"
category: blog
comments: true
headline: "Damn printer"
share: true
tags: [blog, linux]
---
A day off work and with just a couple of errands to run, I used a bit of my free time to try and troubleshoot a couple of computer problems.  The first and really only pressing matter was that of my printer.  For quite some time now, I hadn't been able to print files from my computer.  CUPS (Common Unix Printing System) would send the job to the printer and tell me everything was successful.  However, the printer wouldn't show any kind of activity at all.  After searching for a possible solution, I consulted the Arch wiki.  I re-installed all the necessary packages the wiki article on CUPS said I needed, and it turned out that I was missing the ghostscript package.  I don't know how it got uninstalled...

<pre><code>$ sudo pacman -S cups ghostscript gsfonts</code></pre>

With the printer restored to proper function, I turned my attention to my brother's problem.  For the past few weeks he has been pestering me to install *Diablo III* to the computer and get it up and running.  *Diablo III* is a computer game made for the Windows platform.  I don't remember exactly when I switched from Windows to Linux.  It has to have been at least 3 to 4 years by now.  I don't ever plan on returning to the Windows OS on my desktop either.  So I installed WINE.

No luck in Arch.  Hoping I would have better luck in Ubuntu, I downloaded the latest 14.10 image and put that on my USB stick.

<pre><code>$ dd bs=4M if=~/Downloads/ubuntu-14.10-desktop-amd64.iso of=/dev/sdc && sync</code></pre>

After wiping my old hard drive clean with a fresh install of Ubuntu 14.10, I installed WINE and all of its dependencies through apt-get.  After fighting with the Battle.net launcher some more, I ultimately failed again.  On Ubuntu I was able to update the launcher program, however it crashes once the user logs in.  Oh well.  I guess my brother will have to buy Windows 7 or something.
