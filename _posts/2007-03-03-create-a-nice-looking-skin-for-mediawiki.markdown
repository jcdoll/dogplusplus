---
title: Create a Nice Looking Skin for Mediawiki
layout: post
author: jcdoll
tags:
- howto
---

The default installation of MediaWiki is extremely functional but not too easy on the eyes. There are some nice [examples](http://meta.wikimedia.org/wiki/Gallery_of_user_styles) of skins online. Particularly good looking examples are the [Hula](http://www.hula-project.org/Hula_Project) and [Beagle](http://beagle-project.org/Main_Page) projects. The Beagle guys are really nice and [released](http://beagle-project.org/Talk:Main_Page) their skin, which I used as an example for this website's previous incarnation. There were a couple of bugs on the Beagle skin that I fixed and simplified for this site.

In order to customize the skin you will need to walk through the code. Here's a brief description of the files.

  * Beagle.php: Specifies where everything fits together. If you want to move the navigation bar, eliminate the searchbar or enable Google Analytics this is the place to do it.

  * main.css: Specifies everything from text style, size and color to the look of the navigation bar.

  * logo.png: The Dogully tree logo up at the top of your screen

  * title_bg.png: The orange gradient that sits behind the logo. Specified in main.css
