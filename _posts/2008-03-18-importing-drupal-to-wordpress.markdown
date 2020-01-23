---
title: Importing Drupal to Wordpress
layout: post
author: jcdoll
tags:
- blog
- drupal
- howto
- wordpress
---

Drupal is great software but is a bit much for a simple website, and it's small user base relative to Wordpress makes it difficult to find high quality plug-ins. The final straw was that Drupal is really slow on my current host (Dreamhost) because it makes tons of database queries and Dreamhost keeps my database on a separate server to keep costs down.

I decided to switch to Wordpress last night and managed to transfer all of the data in a couple hours of work. The key was this [sql script](http://www.darcynorman.net/2007/05/15/how-to-migrate-from-drupal-5-to-wordpress-2/) to transfer everything, which is a little bit old and required installing Wordpress 2.0, importing, and then upgrading to the latest version because they changed the table format in 2.2 or so. Now I'm just working on cleaning up some old posts that didn't transfer cleanly (no good wiki syntax in Wordpress, I probably should have switched it awhile ago though) and am happy so far.
