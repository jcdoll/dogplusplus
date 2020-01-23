---
title: Git on MacOSX
layout: post
author: jcdoll
tags:
- git
- how to
- research
---

I've been playing with Git lately and wanted to share some instructions and gotchas. I've installed Git on 10.5 and 10.3 (which needs a few extras installed) but haven't tried them on 10.4 yet, let me know how it goes. From my experience, version control is way underutilized (at least in non-computer engineering) and would reduce the amount of duplicated effort, improve the ability to collaborate, and keep detailed history of important text files (like say your thesis if you're using LaTeX, which you should be). Other version control systems I've used are subversion and perforce, but I like git because it's super fast, it's pretty easy to manage remote repositories over ssh, and it's hot right now.

#### **Initial Setup**
As usual, you need to have the Developer tools installed.

Edit  ~/.bash_login (for just you) or /etc/profile (for all users on your computer)to add /usr/local/bin + sbin to your $PATH variable, e.g. add the following line:

```
export PATH="/usr/local/bin:/usr/local/sbin:$PATH"
```

#### Build and Install Git

```
curl -O http://kernel.org/pub/software/scm/git/git-1.5.4.4.tar.gz
tar -xzvf git-1.5.4.4.tar.gz
cd git-1.5.4.4
./configure --prefix=/usr/local
make all
sudo make install
cd ..
```

If you get a compile error about **po/de.msg make[1]: *** [po/de.msg] Error 127** then you should just be able to run

```
export NO_MSGFMT=1
make all
```

assuming that you only need to install the English interface.

If you get an error like referring to **expat.h** then you will need to build expat using the following commands

```
curl -O http://surfnet.dl.sourceforge.net/sourceforge/expat/expat-2.0.1.tar.gz
tar xzvf expat-2.0.1.tar.gz
cd expat-2.0.1
./configure
make
make check
sudo make install
cd ..
source /etc/profile
```

To make sure that git is installed and working, trying running **git** from the command line.

#### Setting up a remote repository

In my case, I was interested in setting up our lab server (OSX 10.3) to properly host git repositories. First, install git on the server. Next, add the following to  /etc/profile, which is a handy script to make repository creation easy. The script is great and is from [here](http://autopragmatic.com/2008/01/26/hosting-a-git-repository-on-dreamhost/).

```
newgit()
{
if [ -z $1 ]; then
echo "usage: $FUNCNAME project-name.git"
else
gitdir="/Library/WebServer/Documents/git/$1"
mkdir $gitdir
pushd $gitdir
git --bare init
git --bare update-server-info
chmod a+x hooks/post-update
touch git-daemon-export-ok
popd
fi
}
```

After creating the folder /Library/WebServer/Documents/git you can just run **newgit repository.git** to make an accessible repository.

Here's an important step: make sure that /etc/profile properly loads the /usr/local/bin directory (see above). When you push/pull/clone data from your server, things will not go smoothly and will get errors like **git-receive-pack: command not found** or **git-upload-pack: command not found**.

#### Installing gitweb

Git includes a handy .cgi program for viewing your repositories through a browser. It's quick to install assuming you have apache setup with mod_perl installed and it's setup to serve .cgi files. Checkout the gitweb directory in the git source folder after you built git earlier (you didn't delete it yet, right?)

At this point you can use git normally on your remote host. Here are some hastily written examples:

#### Creating a repository from a directory of existing files

```
cd PROJECT
git init
git add .
git commit -m "first commit"
```

#### Creating a new repository

```
mkdir PROJECT
cd PROJECT
git init
(create files, write code)
git add .
git commit -m "first commit"
```

#### Putting your code on your server

```
ssh USERNAME@YOURSERVER
newgit PROJECT.git
git push USERNAME@YOURSERVER:/Library/WebServer/Documents/git/PROJECT.git master
```

#### Pulling code from your server

```
git clone USERNAME@YOURSERVER:/Library/WebServer/Documents/git/PROJECT.git
```

Here are a few additional references to help get started ([1](http://www.kernel.org/pub/software/scm/git/docs/tutorial.html), [2](http://www.dekorte.com/blog/blog.cgi?do=item&id=2539), [3](http://git.or.cz/course/svn.html), [4](http://www.simplisticcomplexity.com/2008/03/04/git-it/), and [5](http://wincent.com/knowledge-base/Installing_Git_1.5.2.4_on_Mac_OS_X_Leopard)).
