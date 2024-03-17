---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Lab 03 — Soldering + Population
description: yes yes soldering

# Micro navigation
micro_nav: true

# Page navigation
page_nav:
    prev:
        content: All content
        url: '../../'
    next:
        content: Lab 04
        url: ../lab_04'
---
<div class="callout callout--danger">
Before you start the lab, please take a moment and make sure your most updated files, BOM, and room layout are uploaded to Github/Altium 365/Slack. Basically, turn in assignment 03. We'll be checking them really quick during lab and ordering parts soon-ish. 
</div>

This lab's got two parts - some background reading, and the actual lab. Fundamentally we want you to:

- Know what soldering is
- Understand how to solder through hole parts
- Understand how to solder surface mount parts, with a variety of methods
- Understand how to do so safely!
- Do some surface mount soldering for a variety of packages!

We __won't__ be doing any through hole soldering in this lab! This is for a few reasons, namely that:
- We don't think it's that complicated. The ability to solder good through hole connections comes mostly from experience, and we didn't want to devote a 2-hour lab section to just soldering the same thing over and over.
- You'll probably run into through hole soldering on your own at some point, and you'll build that experience naturally. Heck, you might even already know how.
- Most boards these days are surface mount, which is more complicated and requires more nuance. The ability to solder good surface mount connections relies on more prior knowledge, and that's worth communicating in lab.

# Background

We attach parts to circuit boards with a process known as __soldering__, where we melt metal in between parts to connect them together electrically. This is better explained with a .gif than it is words:

![](soldering.gif)

*Image Courtesy of [gifs.com](https://gifs.com/gif/soldering-GvL9VK)*

The metal here is called __solder__, and it melts as it's being fed into the joint, which is being heated with the tip of the soldering iron. As we've seen before, there's a metal ring around the hole (called a _pad_), along with a metal _pin_ that goes through it. We heat both the pin and the pad with the soldering iron, and feed in solder, which melts when it hits the joint. Surface tension takes over here and the molten solder fills the voids, and once it cools, it makes a super solid electrical connection between the pin and pad.

The process of soldering our parts onto the board is called __assembly__ or __population__

## Flux

It's also standard to include a gel-like substance in the solder called __flux__, which is a mild acid that removes the surface oxidation on whatever you're soldering. This means that you solder to the strong metal your parts are made of, not a super thin and flakey outer layer. It vaporizes during soldering, which activates it and makes a little white smoke that floats away. This stuff isn't super good for you to breathe in, so be sure to use a fume extractor.

Flux is usually put inside the solder itself - the solder makes a tiny tube that's filled with flux (kinda like a jelly donut) and this is called _rosin-core_ solder because flux used to be made from rosin. If you need more you can also get some on it's own as a little paste or gel. Flux does leave some gunky brownish residue though, but that usually comes off with a little isopropyl alcohol.

<div class="callout callout--danger">
    Beware of using solder or flux that's meant for plumbing! That stuff has a slightly different acid that's better suited to pipes than electronics, and it can corrode your parts.
</div>

This is super neat, and it's how we've been hooking things up for centuries. However it can be a little dangerous, and it's worth talking about.

## Safety

There's a few hazards in soldering: 

- __Hot Stuff__: The soldering iron is _really_ hot, like moreso than [Donna Summer's 1979 single](https://open.spotify.com/track/2zMJN9JvDlvGP4jB03l1Bz?si=c617b16a0f4948de) [^1]. Leaded solder melts at around 370°F give or take depending on what alloy you get, and lead-free melts at around 420°F. As a result the soldering iron has to be at least that temperature, so be careful to not burn yourself! If you drop the iron just let it fall and get out of the way, resist the urge to catch it.

Also be sure to wear safety glasses - I've got friends of friends who've gotten hot beads of solder flicked into their eyes because they were soldering wires under tension. They're blind now. __Wear safety glasses.__

- __Heavy Metals__: Traditionally solder is a mix of lead and tin, but since lead's toxic, manufactured goods are required to use lead-free solder instead. [^2] The leaded stuff flows a little better and is easier to work with. Lead's super toxic once it gets in your bloodstream, so be __super sure__ to wash your hands after soldering! You don't want to ingest any tiny solder bits. Lead is also pretty soft so you don't have to worry much about stabbing yourself with it and getting some in your blood that way, but just be careful. Holding it in your hands is totally okay though, you don't need gloves or anything as long as you wash 'em afterwards. You have the option of using leaded or lead-free solder, though the lab already has trace amounts of lead, and you'll want to wash your hands after regardless. 

<div class="callout callout--danger">
    It's worth repeating: Always wash your hands thouroughly after soldering! You don't want to ingest any tiny solder bits - they're super toxic if they get in your bloodstream. 
</div>

- __Fumes__: When flux boils it gives off a little whiff of white smoke, and breathing that in for a prolonged period of time isn't super good for you. Fume extractors help with this, so we'll be giving you ones like these:

![](extractor.jpg)

*Image Courtesy of [Valtcan](https://www.valtcan.com/products/valtcan-solder-smoke-fan-fume-extractor)*

## Through Hole

There's a fair bit of technique (and differing opinions) that goes into soldering, so most of what we've got here is a synopsis that we agree with. Through hole is the most straightforward and it's been the bread and butter of the maker community for years, so we'll borrow a lot from SparkFun/Adafruit. 

This pair of videos from SparkFun actually does a really good job of explaining how to do through hole:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/f95i88OSWB4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/t9LOtOBOTb0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

And the written guides from [SparkFun](https://learn.sparkfun.com/tutorials/how-to-solder-through-hole-soldering/all) and [Adafruit](https://learn.adafruit.com/adafruit-guide-excellent-soldering?view=all) are also quite good!

And as an aside, there's this cursed technique that Fischer came up with for getting solder out of holes:

_TODO: video @fischerm_


## Surface Mount

Surface mount is a little trickier, and there's a few ways to do it. There's a few methods for getting the solder on the board, and then a few methods for reflowing it.

We'll talk about those, but it's worth mentioning first that the normal through hole technique still works! Just like through hole, we can heat up the joint, add solder, let it flow into the pads we're trying to connect, and then remove the iron. Unlike through hole, the alignment of the part is up to you! We don't have the pin/hole interface to make sure the part is in the right spot, so we'll have to line it up by eye. 

We'll get a chance to do surface mount with an iron later on in this lab. There's enough variation in the techniques for differing packages that it's worth checking out a few. But for now, let's take a look at the how to make our magic combination of paste and reflow come to life:

### Applying Solder Paste

There's a few ways of getting solder paste onto the board:

#### Syringe

![](syringe.jpg)

*Image Courtesy of [Chipquik](https://www.chipquik.com/store/)*

This is about the easiest way to get solder paste onto a board - just get a syringe full of it that you manually squirt onto the board. This works well for smaller things where you have enough patience to properly portion out the right amount of paste, but this doesn't scale well. Your hands get shaky and cramped after doing this for a while, but it's still super handy.


#### Stencil

![](stencil.jpg)

*Image Courtesy of [PCBWay](https://www.pcbway.com/blog/PCB_Design_Tutorial/A_very_easy_way_to_accurately_align_the_stencil_with_the_PCB_1.html)*

You'll get to use one of these in the next lab to put together a bluetooth speaker, so we'll save most of the talking for then! But these are stainless steel stencils that are used to apply solder paste to the PCB. Just put it on top your board, put some paste down, squeegeeee it through the stencil, lift off the stencil and you're good to go!

There's a few tricks to this of course, and we'll cover those in the next lab!

### Reflowing Solder Paste

#### Hot Air Station

![](hot_air_station.jpg)

*Image Courtesy of [Hakko](https://www.amazon.com/Hakko-FR810B-05-SMD-Rework-Station/dp/B01GESLB1I)*

You'll also get to use one of these next lab! Fundamentally these blow hot air on the boards to heat them up, reflowing the solder and assembling the board. 

The only tricks to this are moving your hot air gun in circles to make sure you get even heating and don't thermally stress the board, which a preheater can also help with if you have one (our stations have them built in). Don't use more air than you have to, otherwise you'll blow little parts away. You should see them snap into place as you refow the board.

#### Reflow Oven

![](reflow_oven.jpg)

*Image Courtesy of [puhuit](http://www.puhuit.com/)*

This is a reflow oven - it's a machine designed to take a circuit board through a set of predefined temperatures over time. It slowly ramps up the temperature so the entire board is at the same temperature, melts the solder, and then cools it back down slowly. The exact temperature/time curve is something you can program, and getting the right one is important! Using one that doesn't get hot enough would prevent the solder from melting, while choosing one that's too hot would melt any plastic bits on your board. Choosing one that ramps too fast could hurt the board itself with uneven heating and thermal stresses.

You can get these for a few hundred dollars, but it's also pretty popular to build your own by hacking a thrift store toaster oven. We've also got one upstairs in EDS.

#### Hot Plate

![](hotplate.jpg)

*Image Courtesy of [Home Science Tools](https://www.homesciencetools.com/product/electric-hotplate/)*

Hot air/reflow oven are the most common and preferred methods, but the hotplate method is worth mentioning. It works by just tossing your boards on the hotplate, turning it on until the solder melts, and then turning off the hotplate. This works in a pinch, but it's pretty crude because it doesn't give you good temperature control, and puts all the heat through the bottom of the board - using an oven would give you a more even heating of the board, which prevents the board from warping or tearing itself apart from thermal stress. 

It's also a little weird using a hotplate for soldering since it could be used for cooking afterwards, and we don't want people ignesting solder. Chemistry hotplates are better about this.

This isn't an exhaustive list! There's other exotic methods for getting the paste to melt - everything from lasers to ultrasonics, but this is most of what you'll see at the small scale. There's also a lot of fancy machines used at production-scale too, everything from solder paste printers to reflow ovens the size of a room. 


## Population

We know how to solder parts on the board now - yay! However this is just a technique for getting the parts on the board, and it doesn't tell us what order parts should go onto the board in! When we're putting parts together on our boards, we get the choice of which parts go on in what order. This isn't a problem for boards that are being mass-produced, they're known to work and parts can go on in whatever order they want. But when we're doing prototype/debugging/bringup work we can save a lot of time by soldering parts on in a smart way. We'll talk about this in lecture.

Before we get to actual lab, have a meme :)

![](oh_no.jpg)

# Lab Time!
For this lab, we'll be learning how to solder surface mount components. We'll start with one of our soldering practice boards and populate the components onboard - we've added a bunch of different footprints to the boards, and we'll learn how to solder them. 

__This is hard, and we don't expect you to be good at this!__ Soldering surface mount is difficult, and if you have solder bridges or your pads fall off, that's okay! We have an inspection scope for you to inspect your handiwork, and we really just want you to build technique. You'll get better with practice, we just want to show you how it's done.

As mentioned before, we won't be doing anything with through hole. If you'd like practice with them, feel free to grab a soldering iron from EDS along with a perfboard and some header pins and go to town.

Grab a practice board and let's put together parts in the following order:

### PLCC-2

For the PLCC-2's, we're going to populate the white row of LEDs. This is the bottom row and there should be 10 LEDs total. 

![PLCC-2](PLCC-2.png)

<iframe width="560" height="316" src="https://www.youtube.com/embed/QYWczcsqY5I" title="The Art and Science of PCB Design (IAP 2024) - PLCC-2 Soldering Demo" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

![PLCC-2 NASA](PLCC_NASA.png)

Image credit: [NASA](https://workmanship.nasa.gov/lib/insp/2%20books/frameset.html)

### SOIC 8

<div class="callout callout--danger">
I'm super sorry - I forgot thermal management and poured the ground pours to have no thermal reliefs for SMT parts. The top 2 pins are much harder to solder since they're connected to 2 gigantic sheets of copper. You can send me hatemail at wszeto [at] mit [dot] edu. On the bright side, this is a great lesson on just how much some ground pours can change thermal performance. 
</div>

Note: This board is a little different from our board, but the idea is the same. 

![SOIC-8](SOIC-8.png)

<div class="callout callout--danger">
This is not mentioned in the video, but be sure to check the part orientation! There's a little dot/indent/marking that shows where pin 1 is. It should face the side of the footprint with a longer line. If you're unsure, ask us and we'd be happy to check. We can flip parts after the fact, but that's more work than checking orientation first. 
</div>


<video width="600" height="400" controls>
    <source src="https://pcb.mit.edu/static/videos/lab_03/soic.mp4" type="video/mp4">
</video>

![SOIC-8 NASA](SOIC_NASA.png)

We know that the SOIC dot can be hard to see. Here it is on the microscope, and the dot is on the bottom right. 

![SOIC DOT](soic_dot.jpg)

Image credit: [NASA](https://workmanship.nasa.gov/lib/insp/2%20books/frameset.html)

### 0805 Passives 

Again, the video is slightly than the demo boards, but the methods are the same. Here's a photo to show what resistance values to use for each location. We're soldering everything with 2 total pads. The 4-number codes are what you'll read on the resistor, and the number above that is the value of the resistor. We also suggest you solder the 100 $\Omega$ resistor after Q4 (SOT23). 

![0805 Passives](PassivesValues.png)

<video width="600" height="400" controls>
    <source src="https://pcb.mit.edu/static/videos/lab_03/0603.mp4" type="video/mp4">
</video>

![0805 Passives from NASA](Passives_NASA.png)

Image credit: [NASA](https://workmanship.nasa.gov/lib/insp/2%20books/frameset.html)

### SOT 23

The video is showing a SOT-223 package. It's a little bigger than the SOT 23 package, but the methods are the same. 

![SOT-23](SOT-23.png)

<video width="600" height="400" controls>
    <source src="https://pcb.mit.edu/static/videos/lab_03/sot.mp4" type="video/mp4">
</video>

Soldering quality checks should be the same as the SOIC-8. 

# Testing!

<div class="callout callout--danger">
If you are sensitive to flashing / bright / changing lights please let us know before we test your board! We don't know what will happen when you power up your board and we want to keep you safe. 
</div>

With the grace of a distinguished calligraphy pen, we crafted labels for the world's most distinguished PCB class. The canvas of choice is an imported tapestry of artisanal adhesive, selected to complement the grandeur of the artistic endeavor. Each stroke, laden with opulence, adorns the possessions of opulence, epitomizing refined elegance.

![Tester](Tester.jpg)


In other words, pay attention to the labels. Here's where to stick the probes. 

![Test Points](TestPoints.png)

Ask an LA to show you the testing station! It's on the table near the stairs where no one is sitting. You'll be using test probes to jump your board to +5V, +36V, GND, and W_SIG. If your board works, you should see something like this:

![Demo](Demo.gif)

If it does, congrats! You're done. If you'd like extra practice, you can also solder the row above (blue). The other stuff we'll be soldering using reflow next week. Totally not just because Winnie messed up the red and green LED footprints... Can't be... 

# Unused Packages

We don't solder these devices in this lab, but you might encounter these later on, so we left them here. 


### TSSOP 14
<video width="600" height="400" controls>
    <source src="https://pcb.mit.edu/static/videos/lab_03/tssop.mp4" type="video/mp4">
</video>

![](tssop.jpg)

### QFP64
Unfortunately, we don't have any QFP64 parts - most parts that use this package are microcontrollers/digital logic, which is pretty much the primary target of the chip shortage. We could get these parts but they were super super expensive, so we decided to put that money into having more students instaed of more chips. The process is super similar to the TSSOP though, except now on four sides of a chip instead of just two.

### QFP24
Also unfortunately we had a mixup in the package between vendors - we used a footprint for a smaller variant of a QFP24 than we thought we ordered, so you're more than welcome to try soldering the QFP, but it won't fit. Here's what our best efforts resulted in:

![](qfp.jpg)

Sorry about this - we put most of our efforts into the bluetooth speaker though, so hang tight for those :)



# Resources
- [SMD packages](https://zaxis.net/smd-package-types-sizes/)
- [more SMD packages](http://www.electronicsandyou.com/blog/smd-surface-mount-electronic-components-for-smt.html)
- [EVEN MOAR SMD packages](https://www.electronics-notes.com/articles/electronic_components/surface-mount-technology-smd-smt/packages.php)
- [passives size chart](https://www.kadvacorp.com/technology/smd-capacitor-sizes/)
- [op amp packages](https://learnabout-electronics.org/Amplifiers/amplifiers65.php)

# Footnotes

[^1]: Produced by Giorgio Moroder strangely enough, who's an absolute legend and also showed up on a few Daft Punk tracks.

[^2]: The regulation that decrees this is called RoHS, and all consumer goods have to be RoHS-compliant in order to be sold. RoHS compliance applies to everything in the product, so not only does the solder have to be lead-free, all the parts and PCBs being soldered together have to be lead-free too. It's pretty common to see RoHS compliance mentioned in datasheets - pretty much every part is RoHS compliant nowadays.