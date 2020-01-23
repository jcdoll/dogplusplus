---
title: Run Comsol Simulations from Matlab
layout: post
author: jcdoll
tags:
- comsol
- matlab
---

**Update (2014):** This post is slightly outdated. Since I wrote this post back in 2004, Comsol integrated parameter looping to their solver (it was called Femlab back then). Many use cases that would have previously required Matlab integration can be handled entirely within Comsol now. The particular case that is shown could not (because the permittivity is calculated from a helper function). Unfortunately I do not have the time to answer specific questions or basic FEA issues, but hope that this post gets you pointed in the right direction for simulation automation.

Comsol is extremely powerful because it's well integrated with Matlab. However, the ability to build and run simulations in Matlab isn't well documented.

Here are a few cases where you would want to build a simulation in Matlab:
  * You want to simulate a variety of geometries to optimize a system
  * You are generating tons of data and you would like to save the results for later analysis and plot generation
  * Your simulation is so complicated that the file is becoming tens or hundreds of megabytes in size

#### Instructions

Create a new simulation in Comsol or open an existing one. Now, choose Save As... from the File menu. Before you do anything else, look at the options that you have for saving it. One of them is to save it as a .m file. Do that and then open up the existing file in a text editor. You'll see something like this:

```
flclear fem
clear vrsn
vrsn.name = 'FEMLAB 3.1';
vrsn.ext = '';
vrsn.major = 0;
vrsn.build = 157;
vrsn.rcs = '$Name: $';
vrsn.date = '$Date: 2004/11/12 07:39:54 $';
fem.version = vrsn;
...
```

Take a few minutes to dig through the file and guess what it's doing. It's important to realize that this is a normal .m file so you have access to all of Matlab's libraries and commands.

So how do you run these things? The next time that you start up Comsol be sure to use the Comsol with Matlab option and then just open the file in Matlab like you would any other script. This last step gave me plenty of grief and it's surprising that Comsol doesn't more clearly communicate the massive power that you gain by working directly with the text file (edit: at least in 2004 when I was working on this, I've heard rumors that things are a lot better documented now).

My usual procedure for using this feature is create a new simulation in Comsol using the GUI with the exact options, geometry, boundary conditions, etc. that I want. Once it is done, run the simulation and then save it as a .m file. This approach is useful because Comsol will generate all of the hard part for you and you can just go back and add your for loops, etc.

Here's an example file that I used for simulating the local electromagnetic field enhancement for a gold nanocrescent at UC Berkeley.

{% raw  %}
```
% Joseph Doll
% OpticalOneLayer.m
% FEMLAB 3.1 simulation to calculate the maximum local electric field
% enhancement for a gold or silver nanocrescent of arbitrary ID, OD,
% and opening width immersed in a low absorption dielectric medium of
% arbitrary dielectric constant.
function [simResults, lambda] = NanocrescentSim(OD, IDRatio, ...
  DRatio, lambda, eps_o)

flclear fem
clear vrsn
vrsn.name = 'FEMLAB 3.1';
vrsn.ext = '';
vrsn.major = 0;
vrsn.build = 157;
vrsn.rcs = '$Name: $';
vrsn.date = '$Date: 2004/11/12 07:39:54 $';
fem.version = vrsn;

% -------------------------------------------------------------
% -------------- Start Geometry Initialization ----------------
% -------------------------------------------------------------

% Fix outside diameter
% Specify inside diameter as a proportion of OD
% Specify delta as a proportion of ID

indexCounter = 1;
theta = 0;

for outsideRadius = OD/2
  for insideRadius = outsideRadius.*IDRatio
    for delta = (insideRadius*2).*DRatio

    sprintf('OD = %d, ID = %d, OW = %d', ...
      2*outsideRadius, 2*insideRadius, delta)

    % Calculate the horizontal displacement of each ellipse based
    % upon the dimensions noted above (radii and opening widths)
    outerCircleDx = 0;
    outerCircleDy = 0;

    innerCircleDx = -((outsideRadius - insideRadius) + ...
      (insideRadius*(1-cos(asin(delta/(2*insideRadius))))) - ...
      (outsideRadius*(1-cos(asin(delta/(2*outsideRadius))))));
    innerCircleDy = 0;

    % Define the ellipses based upon the calculated dimensions
    g1=ellip2(outsideRadius,outsideRadius,'base','center','pos', ...
      [outerCircleDx,outerCircleDy],'rot','0');
    g5=ellip2(insideRadius,insideRadius,'base','center','pos', ...
      [innerCircleDx,innerCircleDy],'rot','0');

    % Define the environment bound rectangle
    bound_rect = rect2('4e-6','2e-6','base','center', ...
      'pos',{'0','0'},'rot','0');

    % Boolean operations to create the "sharp structures"
    sharp_nc = geomcomp({g1,g5},'ns',{'g1','g5'},'sf', ...
      'g1-g5','edge','none');

    % Fillets
    round_nc = fillet(sharp_nc,'radii',2E-9,'point',[1 2]);

    % Rotate the finished nanocresent
    nanocrescent = rotate(round_nc, theta ,[0,0]);

    clear s
    s.objs = {nanocrescent, bound_rect};
    s.name={'NC','BR'};
    s.tags={'nanocrescent','boundrect'};

    fem.draw=struct('s',s);
    fem.geom=geomcsg(fem);

    % ---------------------------------------------------------
    % ---------------- Start Simulation Loop ------------------
    % ---------------------------------------------------------

    % Vector of the max electric field at each wavelength
    emax = [];

    % Calculate the dielectric constant values
    % Requires helper function OpticalData.m
    eps_i = OpticalData( lambda(1), ...
      lambda(length(lambda)),length(lambda),1);

    for ii = 1:length(lambda)
      fem.const={'eps_i',eps_i(ii),'eps_o',eps_o};

      clear appl
      appl.mode.class = 'InPlaneWaves';
      appl.module = 'CEM';
      appl.assignsuffix = '_wh';

      % Solve via input wavelength
      clear prop
      prop.field='TM';
      prop.inputvar='lambda';
      appl.prop = prop;

      % Set the boundary conditions
      clear bnd
      bnd.H0 = {{0;0;1},{0;0;0},{0;0;0}};
      bnd.type = {'NR','cont','NR'};
      bnd.ind = [1,3,3,3,2,2,2,2,2,2,2,2,2,2];
      appl.bnd = bnd;

      % Set the domain variables
      clear equ
      equ.epsilonr = {'eps_o','eps_i'};
      equ.ind = [1,2];
      appl.equ = equ;

      % Set the current wavelength
      appl.var = {'nu','1e9','lambda0',lambda(ii)};
      fem.appl{1} = appl;
      fem.border = 1;

      fem=multiphysics(fem);

      % Mesh initialize parameters, smaller = finer
      fem.mesh=meshinit(fem, ...
        'hmaxfact',1, ...
        'hcurve',0.1, ...
        'hgrad',1.2, ...
        'hcutoff',0.001);

      fem.xmesh=meshextend(fem);

      fem.sol=femlin(fem, ...
        'solcomp',{'Hz'}, ...
        'outcomp',{'Hz'});

      % posteval returns the electric field at each point
      eii_struct = posteval(fem,'normE_wh','dl',[1]);

      % Take the maximum field value and add it to emax
      eii_max = max(eii_struct.d);
      emax = [emax eii_max];
    end

    simResults{indexCounter} = emax;
    indexCounter = indexCounter + 1;
  end
end
end
```
{% endraw  %}

Using this approach I was able to automate process of finding the peak electric field intensity as a function of design parameters. Combined with another script I was able to run a simulation for about 3 days on end that tested 300+ different geometry combinations. And all I had to do was set it up, click a button, and put a note on the computer to not disturb it. Like I said, pretty awesome.
