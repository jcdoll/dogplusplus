---
title: Pick Wiki Software
layout: post
author: jcdoll
tags:
- howto
- mediawiki
---

I currently use [Mediawiki](http://www.mediawiki.org) on a few different servers:my [research group](http://microsystems.stanford.edu), a [class](http://memsed.stanford.edu/E240) I TA'ed for and it's [related](http://memsed.stanford.edu/E341) [classes](http://memsed.stanford.edu/E342), and another research [website](http://biomechanics.stanford.edu) or [two](http://mc.stanford.edu). So I'm a big fan of it for certain sites. A wiki allows people to share data and allows people to put together decent looking pages by using the site skins. On the class site, it makes it easy for the other staff to post information and files without digging into any files and provides constant backups.

#### Other Options (Twiki and Drupal)

A few days ago I decided to look for wiki software capable of WYSIWYG editing. Mediawiki has a few [options](http://meta.wikimedia.org/wiki/WYSIWYG_editor) but nothing that is easy for end users. The one that really stood was [TWiki](http://www.twiki.org), which had a WYSIWYG editor built-in and has hundreds of plug-ins. It uses a file system for data storage rather than a database (e.g. MySQL) like MediaWiki, which is nice for smaller installations. It also page-by-page permissions built-in, while MediaWiki requires a plug-in. On the other hand, the installation was much less straightforward and the massive number of options and required setup can be overwhelming, even for someone relatively experienced.

This was all great, and I started looking into transitioning our MediaWiki data over to it (there are plug-ins for that too). That's when I realized that TWiki has horrible syntax. And I mean really, really horrible; it is whitespace sensitive. Then there are headers. Man, I don't even remember what TWiki used, something like "---+" and then more pluses or something. How am I going to explain that to people? And the WYSIWYG editor is only marginally better than the MediaWiki ones, in other words not usable.

#### Drupal

I switched this website from Mediawiki to Drupal for about a year and then switched to Wordpress because a blog engine just made more sense. Here's what I like about Drupal:
  * Many, many more modules for Drupal than Mediawiki.
  * A better organized development community; the software gets better faster.
  * Modules for integration with [Gallery](http://gallery.menalto.com) and anything else you could imagine.
  * Support for Mediawiki markup (the bright spot on Mediawiki).
  * Support for blogs/stream of thought updates.

In the end Drupal was too clunky to maintain for a simple website, although it would be good for more complex content management. Also I found it a bit slow, but that's just me.
