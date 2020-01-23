---
title: Building LayoutEditor on Leopard
layout: post
author: jcdoll
tags:
- blog
- howto
- opensource
---

In order to build tiny things like circuits or MEMS you need to first design them. Typical software is something like L-Edit, which only runs on Windows and costs an arm and a leg. The other day I was looking around for a better option, one that was open source and cross-platform. Luckily I found LayoutEditor, which satisfied both of these requirements and runs great. However, the most recent Mac binary release is about 6 months old, so I've cobbled together some instructions for building the latest source yourself. It's pretty easy so bear with me.

Here are instructions for building and installing the following on MacOSX 10.5:
  * QT
  * Freetype2
  * LayoutEditor (2008-02-05)

Here's what you need before you get started:
  * The latest developer tools, which are not installed by default but should be on your Leopard DVD or can be downloaded from Apple for free.
  * A little patience.

With that said, here's what you do:
  * Open /Applications/Utilities/Terminal.app. In the following instructions there are chunks of text that you will need to copy and paste into the terminal.
  * Download and install QT, a library used by LayoutEditor for cross-platform goodness. You can get a binary release from http://trolltech.com/developer/downloads/qt/mac. Double-click the installer and follow their instructions.
  * Install Freetype2


```
mkdir src
cd src
curl  -O http://download.savannah.gnu.org/releases/freetype/freetype-2.3.5.tar.gz
tar xzf freetype-2.3.5.tar.gz
cd freetype-2.3.5
./configure --prefix=/usr/local
make
sudo make install
sudo ln -s /usr/local/include/freetype2/freetype/ /usr/local/include/freetypecd ..
```

  * Download and build LayoutEditor

```
curl -O http://internap.dl.sourceforge.net/sourceforge/layout/layout-20080205.tar.bz2
tar xf layout-20080205.tar.bz2
cd layout
qmake -spec macx-g++
make
```

  * Install LayoutEditor

```
mv bin/layout.app/ /Applications/
mv macros/ ~/Library/
mkdir ~/Library/Application\ Support/layout/
mv doc/ ~/Library/Application\ Support/layout
mv macros/ ~/Library/Application\ Support/layout
```

If you're curious, we needed to make the freetype symbolic link so that the build process would go smoothly. Then we needed to pass a few arguments to qmake so that it didn't try making a bunch of XCode projects and giving you errors. Finally, we moved the application and the docs/macros folders to their appropriate places.There you have it. You should be able to goto your applications folder and run layout now. Before you get started be sure to read the documentation, especially this [primer](http://layout.sourceforge.net/Introduction_to_LayoutEditor.pdf).I've tested these instructions but if you have any errors or questions please let me know so that they can be updated.

**Update (2/26/08):** If you get a config.log error while building freetype, that means that when you installed the developer tools you unchecked the "duplicate everything in /usr" option, which is key for building unix stuff. Reinstall the developer tools. (Thanks Nahid)

**Update (8/21/08):** Jürgen, the author of LayoutEditor, is now providing a precompiled Universal binary, so these instructions aren't strictly necessary unless you really want to build from source for one reason or another. I just used LayoutEditor to design a reticle using macros, which turned out to be incredibly efficient. Once I work out a few of the kinks in the overall process I'll write up some instructions to share.
