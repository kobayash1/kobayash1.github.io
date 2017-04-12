---
layout: post
title: "Playing the Freshest Kpop in MPD"
description: "Linux is great"
headline: "special thanks to stackoverflow"
category: blog
comments: true
share: true
tags: [blog, linux, terminal, bash, mpd, kpop]
---
So I recently bought a bunch of new k-pop tracks.  Most of them are from 2017, but a few are some old tracks I have only recently become able to buy via melOn.  Collecting k-pop over the past year and half, I have some tracks I purchased a while ago mixed in with the new ones whenever I create a shuffled playlist of my entire k-pop collection.  This leads to a familiar issue where I skip songs I've heard many times until I reach one that I haven't listened to as much.  My {% raw %}updatekpop{% endraw %} script gets the job done when I want to listen to my near 400 track k-pop library:

     {% raw %}
      $ cat Scripts/updatekpop 
            #!/bin/bash
            mpc clear &&
            mpc update --wait 
            mpc ls K-Pop/ | mpc add &&
            mpc shuffle &&
            mpc rm kpop &&
            mpc save kpop
     {% endraw %}

The issue to solve then becomes to create a playlist in mpd that consists of my newest tracks.  Simply sorting by year, I can easily get the 2017 tracks into a neat playlist, but it won't include my also newly purchased songs from before then.  Instead, I would need to find a way to sort the songs in my library by date modified.

      {% raw %}
       $ find ~/Music/K-Pop/ -type f -printf "%T+\t%p\n" | sort | tail -5
             2017-04-08+12:27:02.3826496130  /home/kobayashi/Music/K-Pop/Hyuna/01 - U & Me.mp3
             2017-04-08+12:30:07.1611789570  /home/kobayashi/Music/K-Pop/IMFACT/01 - 텐션업 Tension Up.mp3
             2017-04-08+12:36:09.9316199820  /home/kobayashi/Music/K-Pop/Dreamcatcher/드림캐쳐-02-GOOD NIGHT.mp3
             2017-04-10+11:47:52.6592279990  /home/kobayashi/Music/K-Pop/T-ARA/티아라-01-Lovey-Dovey.mp3
             2017-04-10+11:59:56.7149935390  /home/kobayashi/Music/K-Pop/SHINee/샤이니 (SHINee)-01-Lucifer.mp3
      {% endraw %}

Perfect!  We find all files in the {% raw %}~/Music/K-Pop/{% endraw %} folder, then output the date modified before the full filepath.  Then that output is sorted alphanumerically via the {% raw %}sort{% endraw %} function.  Then {% raw %}tail{% endraw %} takes the 'x' last entries (in this case, 5)!  Now, we just need to take only the filepaths of the songs and put them into a playlist file for mpd to load then play...

      {% raw %}
       $ find ~/Music/K-Pop/ -type f -printf "%T+\t%p\n" | sort | tail -5 | cut -c 32- > ~/.config/mpd/playlists/newkpop.m3u
       $ cat ~/.config/mpd/playlists/newkpop.m3u
             /home/kobayashi/Music/K-Pop/Hyuna/01 - U & Me.mp3
             /home/kobayashi/Music/K-Pop/IMFACT/01 - 텐션업 Tension Up.mp3
             /home/kobayashi/Music/K-Pop/Dreamcatcher/드림캐쳐-02-GOOD NIGHT.mp3
             /home/kobayashi/Music/K-Pop/T-ARA/티아라-01-Lovey-Dovey.mp3
             /home/kobayashi/Music/K-Pop/SHINee/샤이니 (SHINee)-01-Lucifer.mp3
      {% endraw %}

Awesome.  The {% raw %}cut -c 32-{% endraw %} function will cut out the first 32 characters in each line, leaving only the filepaths.  The filepaths are then piped into a playlist file in the folder mpd is configured to search in for playlists.

Now, we need to make this into foolproof script that will accept an integer value to create a playlist of 'x' songs.  If no number (or any value, for that matter) is specified, it defaults to 10.  Finally, if a non-integer value is passed, it errors out, echoing that it needs an integer value:

      {% raw %}
       #!/bin/bash
	if [ -n "$1" ]; then
       	 re='^[0-9]+$'
       	 if ! [[ $1 =~ $re ]] ; then
                echo "error: Value specified is not an integer" >&2; exit 1
                fi
        echo "Creating shuffled playlist of '$1' newest kpop tracks"
        newkpopmpd $1
        else echo "No number of recent tracks specified, creating shuffled playlist of 10 newest kpop tracks"
        newkpopmpd 10
        fi
      {% endraw %}

This function, called 'newkpop', as you can see calls the 'newkpopmpd' script:

      {% raw %}
	#!/bin/bash
	mpc stop
	find ~/Music/K-Pop/ -type f -printf "%T+\t%p\n" | sort | tail -$1 | cut -c 32- > ~/.config/mpd/playlists/newkpop.m3u
	mpc clear
	mpc load newkpop
	mpc shuffle
      {% endraw%}

<figure>
     <a href="{{ site.url }}/images/2017/newkpop.png"><img src="{{ site.url }}/images/2017/newkpop.png"></a>
     <figcaption>Suzy is hot.</figcaption>
</figure>

Amazing!  The first call on {% raw %}newkpop{% endraw %} created a playlist of 10 songs.  The second call specified 30 songs.  I started playback, then called the function a third time with a non-integer.  It errored out just like it should and did not disrupt current playback.
