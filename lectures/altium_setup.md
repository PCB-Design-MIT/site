---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: ECAD Setup
description: We focus a lot on the fundamentals of design and the concepts behind PCBs, which are tool-independent. But we need to choose a design tool to implement things in, and we've chosen Altium and KiCAD for that. Let's get it setup.

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

We'll be using Altium and KiCAD for the course. You can choose whichever one you like the most, staff will be familiar with both. Altium has two parts: Altium Designer, and Altium 365. The former is the software you'll actually use to design boards, and the latter is a workspace for storing, sharing, and keeping track of designs. KiCAD doesn't have this workspace, so you'll need to create a Github repository in addition. This will make more sense as we set it up, for now just buckle in and run through the steps below :) And of course email us at [yaypcbs@mit.edu](mailto:yaypcbs@mit.edu) if you run into anything strange!

# Altium Setup

## Register for an Altium Account

Run over [here](https://www.altium.com/education/student-licenses) and sign up for an Altium account __with your MIT email/kerberos__. This will give you an educational license, which is free as long as you've got an .edu email. Make sure to use your kerberos for this, this makes it way easier for us to link class registration to your Altium accounts. 

Please do this by midnight on January 8th. We'll add ya'll to our Altium workspace on the morning of the 9th, which you should be able to access [here](https://mit-pcb.365.altium.com/). We'll be spending a lot of time here, so we've also linked to it from the front page of the course website!

## Install Altium Designer

The second step is to install Altium Designer. Your steps will vary a little depending on what operating system you're on.

### Windows

Go ahead and download the installer from [the site](https://www.altium.com/products/downloads), you'll have to login to make the links active. Installation is pretty simple, you'll have to sign in with your freshly-minted Altium account, and default settings are fine for us. That's about it.

If you've already got Altium installed, great! Just make sure to update to Altium Designer 23 so that we don't run into any version-specific bugs. If updating to the latest verison presents any problems for you, feel free to just use the virtual machines as described under the section below.

### macOS 

In the words of Altium: try a [VM](https://ist.mit.edu/vmware/fusion). Otherwise, just get a Windows machine. 

In the words of Winnie: Or, use [software that cares to support your platform](https://www.kicad.org/). 

### Everything Else 

In the words of Altium: just get a Windows machine. 

In the words of Winnie: Or, use [software that cares to support your platform](https://www.kicad.org/). 

# KiCAD

## Install KiCAD

Incredibly straight forward. Go to https://www.kicad.org/download/ and download and install for your platform. Should work for Windows/Mac/Linux/Whatever the kids use these days. We recommend you use the stable release. The defaults for the installer and such probably are good, unless you have specific needs. 

## Setting up Github Repositories [Needed for Assignment 02, not for lab 01]

You'll want to go to [Github](https://github.com/) and setup an account. MIT or non-MIT email works, though we recommend you use a non-MIT email since that will ensure you have your files after you leave the $institute^{TM}$. Once you have that setup, let's get a Github client setup. If you have no idea what it means to use a Github repository, go ahead and download [Github Desktop](https://desktop.github.com/). It's going to work well enough for us, and simplifies a lot of the setup. If you actually know how to use another Git tool like Git command line, you're free to do that too. 

Once that's setup, go to [Github](https://github.com/) and create a new Repository. We don't care if it's public or private, but if it's private, please add us so we can see your work. To add people, go into your repo(sitory) -> Settings -> Collaborators and add:

- Winnie Szeto (github.com/wszeto9, wszeto9[at]gmail[dot]com)

When setting up your repo, be sure to change the "Git Ignore" setting to "KiCAD". This ensures that all the caches aren't being backed up to Github. You'll also want to make sure that the repository you made is downloaded locally. 

That's all for now! You should be set for lab tomorrow :)


