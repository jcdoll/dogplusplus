---
title: Mirroring LayoutEditor
layout: post
author: jcdoll
tags:
- integrated circuits
- l-edit
- layouteditor
- lithography
- mask design
- mems
---

**Update (2020):** I migrated this site from a dedicated host to Github and misplaced these files in the process. For new projects I'd highly recommend using [KLayout](http://www.klayout.de) rather than LayoutEditor. I primarily used its excellent Ruby-based DRC engine, but in general KLayout is well designed and supported.

I've [previously written]({% post_url 2008-02-18-building-layouteditor-on-leopard %}) about [LayoutEditor](http://www.layouteditor.net/), a cross-platform application for designing lithographic masks with an emphasis on MEMS. There are other, commercial programs like [L-Edit](http://www.tannereda.com/l-edit-pro) that are available at Stanford but I like LayoutEditor because of its macro support. The icing on the cake of course is that LayoutEditor is free. Or at least was until recently. In late 2008 the creator of the project decided to take it commercial to make ends meet. That's great, and I'm sure the software has improved greatly since then because of the decision. And it didn't affect any of the pre-commercial users because the free versions were still available on SourceForge.

But sometime in mid-2009 all of the free versions (at least post-2007) were scrubbed from the SourceForge [files page](http://sourceforge.net/projects/layout/files/). This was a bit of a problem, because I was pretty sure at the time that I would need another fabrication run (and reticle layout) to finish my PhD. And I'm sure that I'm not the only student who would be more than happy to use old but free software if it were available.

But the free version of LayoutEditor was still available, there just weren't any direct links to it. The URL format on the [new downloads page](http://www.layouteditor.net/download.php5) is easy to guess, so when I noticed that the SourceForge files were gone I started poking around. When I found the 20080925 version (the latest and greatest free version) I downloaded copies for a few different platforms for insurance. A neutered free version was available on the updated LayoutEditor site, but it didn't have my beloved macro feature amongst many other essential features.

I hadn't thought about it again until I came across the files while cleaning up some backups the other day. I poked around on the site again and all of the files I had previously downloaded were gone. So even if someone was desperate enough, there was no trace left of what was once free software. I want to emphasize that there are probably great features in the post-commercial versions of the application and you should support it if you can, but that doesn't change the fact that the software was once free (and sporadically open source). ~~Which is why I decided to free versions of the software here.~~
