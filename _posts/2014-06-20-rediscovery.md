---
layout: post
title: "Rediscovery"
description: "Fond memories"
category: blog
headline: "But crap, piano is hard"
comments: true
share: true
tags: [blog, fitness, music]
---
The final off day from work of this week brings not much to write about.  A few necessary errands, cooking, and catching up on Linux gaming news and [Jupiter Broadcasting](http://jupiterbroadcasting.com) content composed an otherwise sparse agenda for today.  The highlight of today came from my "Workout A" session this afternoon.  I gloriously failed the barbell curls and hyperextensions and fell only one repetition short of completing the bent over rows.  Requiring to reduce the weight for the hyperextensions for Monday is a tough pill to swallow.  Hopefully I can pass the other two, lest I have to reduce their resistances as well.

----

## Music

Spending a good amount of the day at my desktop, I had a chance to reacquaint myself with my aging .mp3 collection.  Stale, and without many tracks dating from even last year, my music selection could benefit from some fresh sounds.  That said, I had a nice rediscovery of sorts with music from *Bartender*.  Ootake Kaoruko's piano playing is so graceful and smooth in these tracks.  Temptation to start up Synthesia and plug in my keyboard once again manifest for the duration of the tracks I heard.  Simply passing the 

    {% raw %}
      music
    {% endraw %}

command in my terminal calls a script I wrote to play my entire music collection in random order.

    {% raw %}
      mplayer -playlist <(find ~/Music -type f -iregex ".*\(adx\|flac\|mp3\|ogg\|wav\)" | sort -R)
    {% endraw %}

Today, however, the script played 3 *Bartender* tracks consecutively.  One day I *will* learn how to play "Moscow Mule."
