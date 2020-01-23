---
title: Connecting to Stanford's VPN Network with Linux
layout: post
author: jcdoll
tags:
- blog
---

**Update Nov 12, 2009:** An even easier VPN solution is to go to sussl.stanford.edu. Since updating to Ubuntu 9.10 my vpnc connection hasn't been working, and I have started using sussl instead.

This is a random tidbit that I just figured out. On MacOSX and Windows you typically need to install the special Cisco VPN client to get things to work. This was always a little annoying, and they don't provide an up to date client for x64 Linux which plays nicely, from what I've gathered. This worked under Ubuntu 8.10 (Intrepid Ibex) for x64, your mileage may vary. I saw the instructions from Wei Cai's [group wiki](http://micro.stanford.edu/wiki/How_to_install_and_configure_the_Cisco_VPN_client_on_a_Linux_computer) and figured that there must be a simpler way, which motivated this.

So here's what I did...
* Install vpnc as described [here](https://wiki.ubuntu.com/VPN)
* Download the Stanford VPN settings file from [here](https://www.stanford.edu/services/vpn/downloads.html).
* From the network settings toolbar, choose VPN Settings > Configure.
* Import the VPN settings file you just downloaded.

Now here's the fun part; they give you the encoded group password but you need the decoded password. But there's a solution described [here](http://www.debuntu.org/how-to-connect-to-a-cisco-vpn-using-vpnc); luckily Cisco has some security problems so that you can decode the group password. Open the VPN configuration file that you downloaded and copy the value for enc_GroupPwd. Paste it into the magic decoder ring [here](http://www.unix-ag.uni-kl.de/~massar/bin/cisco-decode) (hint: the decoded password will be a sentence with the last letter of the last word truncated). Paste the password into the VPN configuration box for the group password. Make sure that you didn't accidentally include a trailing space.

Enter your username and password into the other two appropriate boxes.

Save and try connecting; hopefully things will work and you'll be able to connect to Stanford from off campus. Note that these instructions are a completely valid way of connecting to campus and just save you the hassle of trying to install or build the official Cisco VPN client and give you a tightly integrated VPN solution.
