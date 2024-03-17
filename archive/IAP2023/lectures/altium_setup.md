---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Altium Setup
description: We focus a lot on the fundamentals of design and the concepts behind PCBs, which are tool-independent. But we need to choose a design tool to implement things in, and we've chosen Altium for that. Let's get it setup.

# Author box
# author:
#     title: Fundamentals of Design
#     title_url: 'https://meddevdesign.mit.edu/fundamentals-of-design/'
#     external_url: true
#     description: Good resource on design from Professor Alex Slocum

# Micro navigation
micro_nav: true

# Page navigation
page_nav:
    prev:
        content: Lectures
        url: '../../lectures'
---

We'll be using Altium for the course, which has two parts: Altium Designer, and Altium 365. The former is the software you'll actually use to design boards, and the latter is a workspace for storing, sharing, and keeping track of designs. This will make more sense as we set it up, for now just buckle in and run through the steps below :) And of course email us at [yaypcbs@mit.edu](mailto:yaypcbs@mit.edu) if you run into anything strange!

## Register for an Altium Account

Run over [here](https://www.altium.com/education/student-licenses) and sign up for an Altium account __with your MIT email/kerberos__. This will give you an educational license, which is free as long as you've got an .edu email. Make sure to use your kerberos for this, this makes it way easier for us to link class registration to your Altium accounts. 

Please do this by midnight on January 9th. We'll add ya'll to our Altium workspace on the morning of the 10th, which you should be able to access [here](https://mit-pcb.365.altium.com/). We'll be spending a lot of time here, so we've also linked to it from the front page of the course website!

## Install Altium Designer

The second step is to install Altium Designer. This is normally a Windows/x86-only program, but we've taken some steps to make it work for everyone! As a result your steps will vary a little depending on what operating system you're on.

### Windows

Go ahead and download the installer from [the site](https://www.altium.com/products/downloads), you'll have to login to make the links active. Installation is pretty simple, you'll have to sign in with your freshly-minted Altium account, and default settings are fine for us. That's about it.

If you've already got Altium installed, great! Just make sure to update to Altium Designer 23 so that we don't run into any version-specific bugs. If updating to the latest verison presents any problems for you, feel free to just use the virtual machines as described under the section below.

### Linux / macOS / Android / iOS / Chromebooks 

The folks at IS&T have been kind enough to spin up some Windows virtual machines for us, which we've installed Altium Designer onto for you. The instructions on [their wiki](http://kb.mit.edu/confluence/display/istcontrib/Horizon+Virtual+Desktops+and+Computer+Labs+Landing+Page) are the best, and they'll walk you through installing the VMWare Horizon Client to connect to the VM. You'll need to be on MITnet, either through MIT Wifi (MIT Secure/EECS Labs/RLE) or through our VPN. Instructions for the latter can be found [here](https://ist.mit.edu/vpn).

They also make the client as an app for Android/iOS, so as dumb as it sounds, you could do PCB design on your phone or tablet! If you try this lemme know how it goes, I'm super curious. Altium on an iPad doesn't sound half bad, and might even be cool if you're using Samsung DeX or something on one of their phones.

Once you've got it go ahead and connect to [vdi.mit.edu](vdi.mit.edu), and login with your kerberos. You'll need to include the @mit.edu in your username. You should see a VM called PCB-FAB, go ahead and click that and you should be in.

Go ahead and launch Altium and login with your account, and you should be all set! 

### Everything Else (BSD, Plan 9, Solaris, etc)

If for whatever reason the above steps don't work for you, fear not! You can also access the VMs through a browser tab instead of the client, under the same condition that you're on MITnet.

Instead of using the client, head over to [https://vdi.mit.edu/portal/](https://vdi.mit.edu/portal/) and fire up an HTML client, and then launch the PCB-FAB VM and sign in with your Altium account, just like above.

This does rely on some amount of browser sorcery that I don't fully understand, so if you're on some wierd system that this doesn't work on, let us know! We've got some backup options that probably invovle just throwing the client on some of the linux machines upstairs.

