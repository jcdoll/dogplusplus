---
title: This American Life
layout: post
author: jcdoll
tags:
- blog
- code
- howto
---

**Update (2020):** This is no longer a thing and apps have taken over the world. Listen to every TAL episode at your leisure using your favorite podcast app.

**Update (2009):** I verified that the script still works and it goes up to episode #389 now.

If you enjoy listening to [This American Life](http://www.thislife.org/) on NPR and don't find their weekly free podcast enough, here's a little script to download all of their old episodes. It probably isn't condoned, but I figure that if I had listened to the show from the start and taped them off of the radio they wouldn't have minded, so trying to charge listeners to dip into past shows seems pretty arbitrary. I'd rather just donate to NPR as a whole. I adapted some code from [here](http://www.dirtygreek.org/journal/journalId/2006) so that it works on any system with python installed . Here are step-by-step instructions for MacOSX/Linux/whatever:

  1. Copy and paste the code below into a file called '''download.py'''. Save it in your home directory.
  2. Start up the terminal (/Applications/Utilities/Terminal.app), type '''python download.py''' and hit enter.
  3. All of the shows (up to #342) will be downloaded to your home folder. Shows #5 and #8 don't seem to exist.

```
#!/usr/bin/env python
from urllib import urlretrieve
import urllib2
for i in range(1,5) + range(6,8) + range(9,389):
urllib2.urlopen("http://audio.thisamericanlife.org/jomamashouse/ismymamashouse/%d.mp3" % i)
urlretrieve("http://audio.thisamericanlife.org/jomamashouse/ismymamashouse/%d.mp3" % i, "%d.mp3" % i)
print "Succesfully downloaded %d.mp3" % i
```
