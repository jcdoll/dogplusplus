---
title: Calculate Damping Coefficients in Matlab
layout: post
author: jcdoll
categories:
- howto
- science
---

**Update (2020):** This code and analysis is undergrad level and should be taken with a grain of salt.

When working with experimental data you'll often run into damped oscillations. Most of the time you can eyeball the data and see that the disturbance is gone before it affects what you are really interested in. Occasionally, though, you'll need to actually calculate the damping coefficient and determine whether the system is overdamped or not.

The first time that I ran into this problem was in an automatic controls class where I needed to calculate the damping coefficient for an elastic band in order to use it elsewhere in our equations. At that time I only needed to calculate the coefficients for a few different configurations so I did the analysis in Excel. Essentially what you do is use the solver plug-in to minimize the sum of the error squared between your theoretical data and the experimental data at the peaks of the wave. It works pretty well and here's an [ example] file to see what I mean.

The next time I ran into this problem I was working on a camshaft lab where we were analyzing the valve spring behavior for a range of springs, shims and engine speeds. We were generating a crap load of data, something like a few million data points in all. Using Excel wasn't very practical in this case, so I wrote a nice automated Matlab script to do it for me.

We're assuming that the amplitude envelope is described by a decaying exponential function. The problem is that y=a*exp(sigma*x) isn't particularly linear. To get around this problem we take the log of both sides to get log(y) = log(a) + sigma*x. On a quick side note, log means the natural logarithm (ln) and b will be negative because we are dealing with decay.

Now that we have a linear equation we can easily do a least-squares error fit in order to calculate the constants a and sigma. We'll optimize the equation Y = A + BX where Y = log(y), A = log(a), B = sigma and X = x. After some linear algebra you can show that p = (A'*A)^-1*A*Y where A = [1 X1; 1 X2 ... 1 Xn], Y = [Y1; Y2 ... Yn] and p = [a;sigma]. In Matlab you can just use p = A\Y. You can calculate a and b from A and B and then plot away. But what is the physical meaning of sigma and what does it tell you about the damping for the oscillator?

The idea is that sigma = chi*omega_n where chi is the damping coefficient and omega_n is the natural frequency. The damped natural frequency is equal to sqrt(omega_n^2-sigma^2). There are three cases then:

  * Underdamped: omega_n^2 - sigma^2 > 0
  * Critically damped: omega_n^2 - sigma^2 = 0
  * Overdamped: omega_n^2 - sigma^2 < 0

Here's some example code that generates sample data and calculates the damping parameters.

```
clf
clear all

% This is just example data
xData = linspace(0,20*pi,1000);
yData = cos(2*xData).*exp(-.05*xData);

% If we are only interested in a certain data range
rangeLow  = 0;
rangeHigh = 10000;

xCoords = [];
yCoords = [];

% Find the x,y coordinates for the oscillation peaks
yData    = yData(:);
upOrDown = sign(diff(yData));
maxFlags = [upOrDown(1)<0 ; diff(upOrDown)<0 ; upOrDown(end)>0];
maxIndices   = find(maxFlags);

for ii = 1:length(maxIndices)
    if (maxIndices(ii) > rangeLow) && (maxIndices(ii) < rangeHigh)
        xCoords = [xCoords xData(maxIndices(ii))];
        yCoords = [yCoords yData(maxIndices(ii))];
    end
end

% Calculate the optimal values of a and b
A = ones(length(xCoords),2);
Y = ones(length(xCoords),1);

% We take the log of the actual data (yCoords)
for ii = 1:length(xCoords)
    A(ii,2) = xCoords(ii);
    Y(ii,1) = log(yCoords(ii));
end

% After finding A and B, we know that a = exp(A) and b = B
p = A\Y;
a = exp(p(1));
b = p(2);

% We plug them into their function and plot away. Note that b < 0
curvePlot = a*exp(b*xData);

figure(1)
plot(xData,yData,'-b',xCoords,yCoords,'or',xData,curvePlot,'--g');
legend('Data','Points','Curve');
```



The problem with this code, however, is that if your data has noise in it then it will have trouble finding the actual oscillator peaks. Currently the code considers a peak to a point where the first derivative is positive before and negative after a point. We can quickly improve on this code by incorporating a tolerance band and iterating through the data to find maxima. The tolerance band specifies how far on either side of a point to check in order to verify that the current point is the maximum value in that range. Iterating is a bit slower than the 'find' approach, but it'll do.

The following code does a lot more than the previous code. It was used in analyzing the damping of valve spring oscillations at high engine speeds, so it:

  * Loads the spring force data for a specified engine speed. Three springs and three shim sizes were studied.
  * Shifts the force data so that the average force during oscillations is zero. This is necessary accurately report the magnitude of oscillation, which is handy for diagnosing dynamic coil binding.
  * Calculates the damping factor.
  * Calculates the theoretical natural frequency from known masses and spring constants.
  * Calculates the theoretical damped natural frequency, which can later be compared to a fast Fourier transform (FFT) of the system to verify the numbers.

```
clf
clear all

engineSpeed = 4800;

temp = load([num2str(engineSpeed) '_Ford_Damped_NoShim.m']);
AllData(:,1) = temp(:,2);
temp = load([num2str(engineSpeed) '_Ford_Damped_Shim034.m']);
AllData(:,2) = temp(:,2);
temp = load([num2str(engineSpeed) '_Ford_Damped_Shim064.m']);
AllData(:,3) = temp(:,2);

temp = load([num2str(engineSpeed) '_CASS_Damped_NoShim.m']);
AllData(:,4) = temp(:,2);
temp = load([num2str(engineSpeed) '_CASS_Damped_Shim034.m']);
AllData(:,5) = temp(:,2);
temp = load([num2str(engineSpeed) '_CASS_Damped_Shim064.m']);
AllData(:,6) = temp(:,2);

temp = load([num2str(engineSpeed) '_CADS_NoShim.m']);
AllData(:,7) = temp(:,2);
temp = load([num2str(engineSpeed) '_CADS_Shim034.m']);
AllData(:,8) = temp(:,2);
temp = load([num2str(engineSpeed) '_CADS_Shim064.m']);
AllData(:,9) = temp(:,2);

for ii =  1:9
    xData = linspace(0,720,2048)/engineSpeed/360*60;
    yData = AllData(:,ii);
    yData = yData - mean(yData(100:500));

    % If we are only interested in a certain data range
    rangeLow  = 101;
    rangeHigh = 1200;
    checkRange = 100;

    maxIndices = [];

    for jj = 1:length(yData)
        if (jj > rangeLow) & (jj < rangeHigh)
            for kk = -checkRange:checkRange
                if (yData(jj) >= yData(jj+kk))
                    if kk == checkRange
                        maxIndices = [maxIndices jj];
                    end
                else
                    break
                end
            end
        end
    end

    xCoords = xData(maxIndices);
    yCoords = yData(maxIndices);

    % Calculate the optimal values of a and b
    A = ones(length(xCoords),2);
    Y = ones(length(xCoords),1);

    % We take the log of the actual data (yCoords)
    for jj = 1:length(xCoords)
        A(jj,2) = xCoords(jj);
        Y(jj,1) = log(yCoords(jj));
    end

    % After finding A and B, we know that a = exp(A) and b = B
    p = A\Y;
    a(ii) = exp(p(1));
    sigma(ii) = p(2);

    % We plug them into their function and plot away. Note that b < 0
    curvePlot = a(ii)*exp(sigma(ii)*xData);
    figure(ii)
    plot(xData,yData,'-b',xCoords,yCoords,'or',xData,curvePlot,'--g');
    title('CADS (4800 RPM)','FontSize',20);
    xlabel('Time (s)','FontSize',20);
    ylabel('Spring Force (N)','FontSize',20);
    legend('Experimental Data','Analyzed Points','Fitted Curve');
    pause
    print -r300 -djpeg80 SpringOscillations
end

% Calculate the natural frequencies:
mretainer = 28e-3; %kg
mkeepers  = 4.3e-3;
mrocker   = 116.5e-3;
mvalve    = 96.4e-3;

msys = mretainer + mkeepers + mrocker + mvalve;

mspring(1) = 61.6e-3; % Ford
mspring(2) = mspring(1);
mspring(3) = mspring(1);
mspring(4) = 75.4e-3; % CASS
mspring(5) = mspring(4);
mspring(6) = mspring(4);
mspring(7) = 70.2e-3; % CADS
mspring(8) = mspring(7);
mspring(9) = mspring(7);

meff = mspring*.33 + msys;

k(1) = 28.3e3; % Damped Ford (N/m)
k(2) = k(1);
k(3) = k(1);
k(4) = 50.4e3; % Damped CASS
k(5) = k(4);
k(6) = k(4);
k(7) = 31.4e3; % Damped CADS
k(8) = k(7);
k(9) = k(7);

w_natural = sqrt(k./meff)
w_damped = sqrt(w_natural.^2-sigma.^2)
```
