---
title: Asus and DVD drive firmware
layout: post
author: jcdoll
tags:
- how to
---

My [new laptop]({% post_url 2009-02-25-the-times-they-are-a-changin %}) has been great for the past seven months or so. Ubuntu is speedy and reliable, and by buying a random Asus laptop (X83VM-X1) I'm free of the [upgrade](http://www.apple.com/macosx/) [cycle](http://www.apple.com/macbook/). No need to lust over the latest computer when I don't even know how mine fits into their product lineup. The only problem is that I bought a random laptop from Asus. My DVD drive has been quirky to say the least and has been mostly unusable. I was unlucky enough to get a drive with an older firmware (SR04) than the one provided on newer machines (SR07). I tried contacting Asus but they refused to send me the new firmware. I had given up on using the drive, not that it was a big problem. That is, until I came across comment #26 on this [Launchpad thread](https://bugs.launchpad.net/ubuntu/+source/dvd+rw-tools/+bug/307477). This lead me to the guy's [blog](http://solarisnevada.blogspot.com/2009/08/gsa-t50l-issues.html), where he linked to a copy of the firmware that I needed. You can apply the patch in Windows to a GSA-T50L disc drive with SR04 on it, and I can confirm that it won't make your computer explode or anything.
