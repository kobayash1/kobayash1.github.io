---
layout: post
title: "Fixing TT-RSS"
description: "Regaining access to my new content"
category: blog
comments: true
headline: "How come I didn't find this earlier?"
share: true
tags: [blog, linux, fitness]
---
Months ago, I updated my version of Apache from 2.2 to 2.4 with a pacman upgrade.  The update changed some settings in my PHP installation that ultimately broke my RSS reader, TinyTinyRSS.  I couldn't find the solution to my problem with minimal effort searching on Google.  So I decided to let it sit and try to repair it later.  Fast forward to today.  I find myself bored at my desktop without new content to read or watch.  Another search on Google finds my answer.

During the upgrade, access levels on the database were reset for all users.  I had to log in to mysql in a terminal and increase the access level for my RSS reader account.  Here is how I did it:

     {% raw %}
       mysql -u <sqluser> -p <dbname>
       Password: <sqlpasswd>
       update ttrss_users set access_level=10 where login = '<ttuser>';
       exit
     {% endraw %}

Now I can access all my subscribed content again from one place!

----

## Fitness

Workout A was on the agenda for today.  I gloriously failed the cable crunches 3x10 for the first time yet, at 145 lbs. of resistance.  I will definitely have to reduce the weight on that until my core develops more strength.  I continue to make gains in squats, however, setting a new personal record of 85 pounds 5x5.  Friday at 90 lbs. will be interesting.
