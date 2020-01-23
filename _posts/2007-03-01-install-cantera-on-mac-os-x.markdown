---
title: Install Cantera on Mac OS X
layout: post
author: jcdoll
tags:
- howto
- macosx
- science
---

[Cantera](http://www.cantera.org) is a suite of object-oriented software tools for problems involving chemical kinetics, thermodynamics, and/or transport processes. It can be used from MATLAB, Python, C++, or Fortran. I used it for ME 140 (Combustion Engineering) at UC Berkeley in Fall 2005 to calculate things like adiabatic flame temperatures and equilibrium states for homework problems.

There isn't any documentation for installing Cantera on Mac OS X, and although it should be straight forward I encountered some problems and I want to try to fill that documentation void. Cantera is incredibly powerful software and the creator (Dave Goodwin) is helpful and active on the official [newsgroup](http://groups.yahoo.com/group/cantera/).

#### My Settings

My system specifications are:
  * Mac OS X 10.4.2
  * XCode 2.0 developer tools (current version as of 9/13/05)
  * Matlab 7 (R14)
  * Cantera installed using the 1.6.0 binary release

#### The Symptoms

I could not run the Cantera examples from Python or Matlab and was encountering the following types of errors:

```
>> run_examples
EQUIL a chemical equilibrium example.

This example computes the adiabatic flame temperature and
equilibrium composition for a methane/air mixture as a function of
equivalence ratio.

Traceback (most recent call last):
File "./.cttmp1125719147.pyw", line 1, in ?
from ctml_writer import *
ImportError: No module named ctml_writer
???

Cantera Error!

Procedure: ct2ctml
Error: could not convert input file to CTML.
Command line was:
sleep 1; python ./.cttmp1125719147.pyw &> ct2ctml.log

Error in ==> XML_Node.XML_Node at 11
x.id = ctmethods(10,15,0,src); % newxml(name)

Error in ==> Solution.Solution at 30
doc = XML_Node('doc',src);

Error in ==> IdealGasMix at 39
s = Solution(a);

Error in ==> equil at 12
gas = IdealGasMix('gri30.cti');

Error in ==> run_examples at 3
equil(0);
```

and from within Python

```
-----:/Applications/Cantera/demos/python/flames -----$ python flame1.py
Traceback (most recent call last):
File "flame1.py", line 7, in ?
from Cantera import *
ImportError: No module named Cantera
```

#### The Fix

The error messages indicate that Python is not aware of the Cantera module. After some thought and googling I decided that I should check what directories Python is searching for its modules. The PYTHONPATH environment variable can be printed by importing sys and printing sys.path.

```
-----:/Applications/Cantera/demos/python/flames -----$ python
Python 2.3.5 (#1, Mar 20 2005, 20:38:20)
[GCC 3.3 20030304 (Apple Computer, Inc. build 1809)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> print sys.path
['', '/System/Library/Frameworks/Python.framework/Versions/2.3/lib/python23.zip',
'/System/Library/Frameworks/Python.framework/Versions/2.3/lib/python2.3',
'/System/Library/Frameworks/Python.framework/Versions/2.3/lib/python2.3/plat-darwin',
'/System/Library/Frameworks/Python.framework/Versions/2.3/lib/python2.3/plat-mac',
'/System/Library/Frameworks/Python.framework/Versions/2.3/lib/python2.3/plat-mac/lib-scriptpackages',
'/System/Library/Frameworks/Python.framework/Versions/2.3/lib/python2.3/lib-tk',
'/System/Library/Frameworks/Python.framework/Versions/2.3/lib/python2.3/lib-dynload',
'/System/Library/Frameworks/Python.framework/Versions/2.3/lib/python2.3/site-packages',
'/System/Library/Frameworks/Python.framework/Versions/2.3/Extras/lib/python']
```

As you can see above, the /Libary/Python/2.3/ (or, optionally I think, the /Applications/Cantera/bin/) directory are not being searched for modules, and it happens that the Cantera files are located in those directories. In order to search additional directories you can create .pth files in the current search path to add additional directories to the search path. There is a file called Extras.pth in the /Library/Python/2.3/site-packages/ directory which will do just this task and it's a simple task to add our new directories.

Open the Extras.pth file from the command line:

```
emacs /Library/Python/2.3/site-packages/Extras.pth
```

Edit it so that it looks like this:

```
/System/Library/Frameworks/Python.framework/Versions/2.3/Extras/lib/python
/Library/Python/2.3/
```

Now you're all set!
