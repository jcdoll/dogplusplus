---
title: Downloading Stanford Lectures
layout: post
author: jcdoll
tags:
- blog
---

Many Stanford classes are posted online as part of SCPD. Occasionally I get the hankering to learn something new and think that it would be great to download all of the videos from the past quarter and watch them at my leisure long after they've been removed from the SCPD website. Unfortunately, SCPD doesn't go out of its way to make it quick or easy to download the videos.

Luckily, someone named Jon [went ahead](http://hawflakes.unoc.net/?tag=scpd) and wrote two scripts that are very useful. The first is a python script that uses mencoder to download the videos if you pass it a list of links. Getting the video URLs by hand is tedious though, so Jon wrote a Greasemonkey script that grabs all of the links for you.

Unfortunately, Stanford updated its SCPD page format at some point since the script was first published, so when I tried using it the other day it kept on throwing errors. I fixed the Greasemonkey script and posted the changes back as a comment to the original blog.
