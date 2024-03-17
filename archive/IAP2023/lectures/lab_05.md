---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Lab 05 — Population and Assembly!
description: Puttin' it all together, quite literally.

# Micro navigation
micro_nav: true

# Page navigation
page_nav:
    prev:
        content: All Lectures
        url: '../../lectures'
---

<div class="callout callout--danger">
There's a lot going on in this lab, and it's got a lot of moving parts in order to make it work. Read the whole thing before you start.
</div>

You'll be doing this lab with your lab partner. This is done to speed things up, one person will be obtaining parts while the other is placing them. There's a lot of parts and doing either task can get old fast - make sure to trade off between yourselves.

# Safetyin'

You'll be soldering a bit in this lab, this time with reflow stations and solder paste instead of soldering irons and solder in wire-form. The regular rules still apply towards:
- __Hot things:__ The reflow stations heat the boards up by blowing hot air on them which is a little more forgiving than a super hot soldering iron, but it'll still burn you. Keep an eye on where you're pointing your reflow gun, and make that isn't towards you or your lab partner.
- __Heavy metals:__ The solder paste we're giving you is leaded, meaning that we've got a bunch of microscopic solder balls. We don't want you to ingest this, so be sure to wash your hands afterwards. We'll also be giving you gloves to wear while pasting your boards - feel free to keep them on or grab new ones during population if you'd like.
- __Sharp things:__ The tweezers are sharp, don't stab yourself with them. Be gentle with them too - it doesn't take a ton of force to keep a part in your tweezers, and you'll send it flying if you sqeeze too hard.
- __Tiny things:__  For the reason above. We'll keep our safety glasses on to keep any stray flying bits out of your eyes. Grab a set from EDS.

# Stencilin'

We've set up some stations around lab with a setup that looks about like this. Here's a video that shows you how to do the stenciling:

<video width="600" height="400" controls>
    <source src="https://pcb.mit.edu/static/videos/paste_demo.mp4" type="video/mp4">
</video>

We've also put a little piece of kapton over some parts of the stencil that we don't want to put solder paste through. Be careful to not peel these off with your spudger!

Feel free to have a quick look at your board under the microscope once you've got paste on. You can see the little solder balls under there, and how crisp your stenciling job was.

![](paste.jpg)

![](always_has_been.jpg)

# Populatin'

Once you've finished stenciling your board (yay!) we'll go ahead and start populating the board. Conveniently, Altium's got an assembly feature that helps us with this. Go ahead and open the staff reference design in Altium [here](https://mit-pcb.365.altium.com/designs/8B77B4A2-3FB0-46D1-A7AF-E692737A4C77?location=[1,90.38,47.49,37.49]&dwfPageId=&layers=&variant=[No+Variations]&activeView=SCH&activeDocumentId=PKSEBWBJ-1#design).


Peep the _Assembly_ tab. Go ahead and hit _Start Assembly_, and we'll get something that looks like this:

![](altium_assembly.png)

Here Altium's got all the parts that need to go on the board on the left. It'll tell you where they're supposed to go, and keep track of what you've placed on the board. Go ahead and click _Done_ once you've got that part on board. It does seem to reset your progress if you refresh the page, so just be careful!

On paper, you just keep placing things until everything's on, but there's a little more nuance to this, just like we talked about in [lecture 05](../lecture_05). We're going to practice what we preach with regards to component population. We'll put on all the passives + power circuity first, and then once we're sure that works, we'll go ahead and solder the rest of the big chips. We've placed thin kapton tape on the stencils so that no solder paste gets applied to those pads. You'll use the techniques from lab03 to solder those parts when the time comes.

Before we jump in though, a few things to note:

- __Battery Holder (B1, B2)__ - these will be the very last thing you populate on your board! These are plastic parts that go on the back half of the board, and will get melted into a big puddle of gooey sadness if we have them on when we reflow the board. Plus they'll prevent the board from lying flat on your bench, which makes populating parts really hard.

- __Speaker Connectors (J2, J3)__ - don't populate them just yet! Same reason as above.

- __Audio Jack (J4)__ - don't populate it! It's meant as a failsafe if your amplifier doesn't work, so we can wire it up to a normal speaker with a 3.5mm audio jack. But we're going to spread good vibes and hope that you don't need it quite yet.

- __Programming Header (J5)__ - don't populate it either! We're leaving it off for the same logic as the audio jack - it's meant as a backup for USB/UART bridge, and we're going to keep spreading good vibes by leaving it off.

- __Amplifier Config Resistors (R29, R30, R31)__ - don't populate these, they're used for setting some configuration options on the amplifier but we don't want to enable those particlar bits. We might choose to populate them during debugging if we need to, but for now just leave them off the board.

- __Testpoints__ - no need to populate them! They sell little loops that you can solder on there, but that's convenient for an oscilloscope probe (with the little spring-loaded doodad) but it's kinda annoying for multimeter probes. And it costs money. So we've elected to let our horses graze elsewhere 🤠

- __CP2014 (USB/UART Bridge)__ - normally we wouldn't want to populate this chip immediately, but we're going to ask you to here. The reason being that it's going to be really difficult to get the solder paste application right with a syringe, so to increase our odds we'll just have ya do it at the beginning. If this footprint was any less complicated, we'd just do it later.

<div class="callout callout--danger">
Be super careful to get the orientation right on your ICs, diodes, and LEDs as you put them in. The marking on the part should match the orientation marker on the part - usually it's a little dot or line. Feel free to ask us if you're unsure which way to put it in - preferably <em>before</em> you place it on the board and solder it in.
</div>

Go ahead and pop everyting onto your board that you've got the solder paste for - should be everything except for the ESP32, DAC, and Amplifier.

# Reflowin'

The original plan was to show you how to reflow in lab03, but Things™ happened and this'll be our first rodeo. You'll need to bring your populated board to a reflow station at the far end of lab. Go ahead and:

- Set the airflow knob to zero - all the way to the left. This is to make sure we don't accidentally blow away our parts!
- Turn the station on with the big black switch in the back.
- If it isn't already on, enable the the _Hot Air_ and _Preheater_ switches on the front. If they're off, they'll say _OFF_. If they're not, they'll say _H100_, or whatever temperature they're at.
- Go ahead and set the preheater to 150°C, and the hot air to 280°.
- Gently throw ya board on our little barbeque once it's all up to temperature. Make sure the hot air gun is not pointing at the board just yet.
- Once it's been a minute or two and the board is done preheating, go ahead and grab the reflow gun. Pick a spot to start, and hold the gun 3-4 inches away from the board, and move it around in small circles to evenly heat the part. You'll see the solder paste change color as the goo holding the parts in evaporates off, leaving the solder balls behind. This means the paste's heating up, keep going until you see it properly melt and reflow.
- Keep moving around to the other parts of the board, soldering everything you've populated. This part's super satisfying, so enjoy it 😎
- Once you're done, turn the station off. Let everything cool down for a few minutes before taking the board off - we don't want to bump the parts off while the solder's still molten. Because that would be like, super sad. And we're in this for happy vibes 💖

If your electrolytic caps or inductors don't get all the way there, that's totally okay! We can hit those with a soldering iron afterwards :)

# Stashin'

You're probably not going to get through the whole thing in one go. Your boards are going to be safer chillin in lab for a few days then they would be floating around in your backpack, so we'll ask you to leave your boards in lab rather than take then with you. We'll show you where to stash them.

# Debuggin'

## Find Shorts

Once you've got your parts soldered onto the board, go ahead and make sure you don't have any shorts. Grab a multimeter (they're at your lab benches, grab leads from EDS if you need them) and make sure you don't have any shorts between:

- 3V3 and GND
- 12V and GND
- VUSB and GND
- VBAT and GND

If you do, that's okay! Have a look around your board and look for shorts - feel free to use the microscope if you need to. Once you've found them, use some solder wick to clean 'em up and you're on your way.

If you don't, nice work! Go ahead and plug in a USB cable and see if there's 3.3V on the board - use the 3V3 testpoint for this. We won't have 12V yet since we need the ESP to enable the boost converter. Speaking of which, go ahead and solder your ESP32 when you've got your 3.3V voltage rail up.

Soldering the ESP32 is going to require a little nuance. Go ahead and use a syringe to put paste on the bottom 3x3 grid of pins, and then reflow that. Once that's in, go ahead and do the pins around the chip with an iron.

## Flash Firmware

Now let's try to flash your chip with firmware! Originally we had a nice bit of Javascript to do this for us through your web broswer, but as a collosal surprise to _absolutely no one_, Javascript is terrible. Instead we'll just have Fischer come over and program your board.

I'll need a name for your bluetooth speaker - that's what it'll show up as in your Bluetooth settings. Make sure you give me a solid name though - [here's](https://www.babycenter.com/baby-names/most-popular/top-baby-names-2023) some good options.

Once you've got your firmware flashed, make sure you can pair to it from your phone.

<esp-web-install-button
  manifest="/media/firmware/manifest.json"
></esp-web-install-button>

## Solder the DAC

Once you're able to get bluetooth audio onto your board, go ahead and solder the DAC. __Make sure you have the orientation dot aligned correctly.__ Check that the DAC works by scoping the RCH and LCH testpoints on your board with the oscilloscope - you should see a flat line when there's no music, and then some signal when the music's playing. Let us know if you have trouble with this!

## Solder the Power Switch

Ask the staff for a power switch and a pair of wires to solder to it. You'll need to strip the insulation off the ends of the wires, and solder the wires to either side of the switch. The other end will go into the power connector on your board (J5).

<div class="callout callout--warning">
For whatever reason these switches melt really easily with the normal blue irons - so we set the irons on the reflow stations to be a little colder to still melt the solder, but not the switch. Go ahead and use one of those.
</div>

## Check the Boost Converter

At this point, all you're missing is the amplifier. It gets power from the 12V rail, which is generated by the boost converter - and we haven't verified yet. The boost converter's input is from the battery (not USB), which we haven't soldered into the board yet. Therefore, we'll want to test that the boost converter works with a power supply. We'll apply 3.7V to where the battery would go, and then make sure that we see 12V on the 12V rail.

We'll help you with this. Flag us down when you get here - we'll walk you through the testing.

## Solder the Audio Amplifier

Once we know that our amplifier's going to get power, let's go ahead and solder it in using the techniques we learned in [lab03](../lab_03). __Make sure you have the orientation dot aligned correctly.__ The audio amplifier comes in a HTSSOP package - a slight variation on the TSSOP we soldered last week, but we'll use the same technique.

Have a look at your work under the inspection scope once you're done, and make any tweaks if you need to. If you're wondering if your solder job is adequate, ask for our opinion.

Once you're happy with it, go ahead and solder in the screw terminals (J2, J3) and one of the battery holders. Keep the orientation of each part in mind. The screw terminals should have their metal bits facing away from the board, and the battery terminals should have their minus and plus markings match what's on the board.

## Solder the Speaker

Ask the staff for a speaker and a length of speaker wire to solder to it. You'll need to strip the insulation off the end of the wire, and solder the speaker wire to the plus and minus terminals on your speaker. You'll strip the insulation off the other side too, but those will go into the screw terminals on your board.

## Flicking the Switch :)

Go ahead and flag a member of the teaching staff down. We'll rig your setup up on a power supply again, clipping to the battery leads. We'll flick the power switch, knock on wood, and we should get audio out! If this works then we'll give you a set of batteries, and the speaker is yours!

## Heatsink

The audio amp is super effecient, but we're still pushing a good bit of power through the board so it'll get a little warm. To keep it happy we'll throw a heatsink on the back of it, which we'll stick on with _thermal tape_! This is the blue double-sided stuff on the staff table, go ahead and cut a length of it to cover the back of your heatsink. Cover the entire back of the heatsink - we don't want to leave any exposed metal that can short things out. Go ahead and trim the excess tape around the edges, and then you should be set!

__Be sure to give us your names/kerberoses/board number before you leave. Otherwise we don't have a record of you finishing the lab!__