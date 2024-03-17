---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Lab 06 — Population and Assembly on your PCB!
description: Puttin' it all together, quite literally. It's your PCB's turn :)

# Micro navigation
micro_nav: true

# Page navigation
page_nav:
    prev:
        content: Schedule
        url: '../../schedule'
---

<div class="callout callout--danger">
Since this is your own PCB, you're the one that has the best idea of how it works! You'll be responsible for getting it working before the Friday student showcase. We don't expect you to write code that is finalized and polished, but you should demonstrate that your circuits work. For instance, blinking all the lights on the MBTA line rather than fetching the data from the Internet.
</div>

# Safetyin'

You'll be soldering a bit in this lab, this time with hot plate and solder paste instead of soldering irons and solder in wire-form. The regular rules still apply towards:

- __Hot things:__ The hot plate heat the boards up by being a hot plate which is a little more forgiving than a super hot soldering iron, but it'll still burn you. Keep an eye on where your hands are, and don't touch the metal surfaces. The wood may be a bit warm, so be sure to test the temperature before you stick your hand anywhere.
- __Sharp things:__ The tweezers are sharp, don't stab yourself with them. Be gentle with them too - it doesn't take a ton of force to keep a part in your tweezers, and you'll send it flying if you sqeeze too hard.
- __Tiny things:__  For the reason above. We'll keep our safety glasses on to keep any stray flying bits out of your eyes. Grab a set from us if you don't have normal glasses.

# Stencilin'

Your stencils come taped to an wood MDF board and to keep things clean and portable, we want you to do your stenciling on this wood board.

![Wood board step 1](/labs/lab_06/StencilStep1.jpg)

<div class="callout callout--danger">
Handle the stencil carefully and do NOT bend or flex it too much. We need it to remain as flat as possible to ensure a good pasting.
</div>

Go ahead and remove the tape and plastic covers from the stencil.

![Wood board step 2](/labs/lab_06/StencilStep2.jpg)

Using secure a board on the wood panel using four other boards. Be sure to secure the four outer boards rigidly with tape, do not apply any tape or adhesive to the center board. The center board is the board we will paste.  

![Wood board step 3](/labs/lab_06/StencilStep3.jpg)

Go ahead and align your stencil with the pads of the center board.

![Wood board step 4](/labs/lab_06/StencilStep4.jpg)

Be sure to take a close look at all pads and ensure they are centered within the stencil holes.

![Wood board step 5](/labs/lab_06/StencilStep5.jpg)

Put a single piece of tape across the top of the stencil and the wood board. This way, after you apply paste, the stencil opens upwards like a book.

At this point, you can begin pasting. Remember, apply a single line of solder paste on one side and, using a credit card at 45 degrees, slide the solder paste across the stencil in one go.

Avoid multiple swipes of paste across the stencil, these steel stencils are much thicker than the polymide stencils we used for the demo board and will allow more paste to be left on the board pads.

![Wood board step 6](/labs/lab_06/StencilStep6.jpg)

Once you finish pasting, you can open up your stencil like a book (this will make it easy to paste other boards if you need to). You can now remove one of the outer taped boards to free your center pasted boards.

![Wood board step 7](/labs/lab_06/StencilStep7.jpg)

Here's a video that shows you how to do the stenciling:

<video width="600" height="400" controls>
    <source src="https://pcb.mit.edu/static/videos/paste_demo.mp4" type="video/mp4">
</video>

Feel free to have a quick look at your board under the microscope once you've got paste on. You can see the little solder balls under there, and how crisp your stenciling job was.

Once you finish all our stencilling for the day, please be sure to clean off the excess paste using alcohol.  

# Populatin'

Once you've finished stenciling your board (yay!) we'll go ahead and start populating the board. For KiCAD, we suggest getting the [Interactive BOM](https://github.com/openscopeproject/InteractiveHtmlBom) plugin. We used this tool to generate the BOM y'all worked with last week for the demo PCB assembly. On Altium, you can go to Altium 365 and open your project. On the left, select the _Assembly Assistant_ and pray to the Altium Gods that you didn't use up all 10 of your free trials.

For assembly order, that's up to you, but we suggest placing smaller components before placing larger components. And of course, you should leave through-hole components to be soldered last.

<div class="callout callout--danger">
Be super careful to get the orientation right on your ICs, diodes, and LEDs as you put them in. The marking on the part should match the orientation marker on the part - usually it's a little dot or line. Feel free to ask us if you're unsure which way to put it in - preferably <em>before</em> you place it on the board and solder it in.
</div>

Go ahead and pop everything onto your board that is within reach. Towards the last 15-30 minutes of lab, we'll be asking if you need to go home at 12:00. If you do need to leave immediately, we're going to suggest that you reflow your board so the components don't come lose. The last thing we want is for you to lose progress.

# Reflowin'

<div class="callout callout--danger">
Board check: before moving on this step, have an LA check your board.
</div>

- Turn on the hot plate if it's not on already. We're going to set it to $270 ^ \circ C$ since that's what our paste wants in order to reflow. Wait for it to reach at least $260 ^ \circ C$. If we put it on too early, it could boil off the flux before the board is hot enough, and we'd be left with bad solder joints.
- Slap it on the hot plate
- The hot plate makes your board hot. Wow such hot
- Once it's been a minute or two you'll see the solder paste change color as the goo holding the parts in evaporates off, leaving the solder balls behind. This means the paste's heating up, keep going until you see it properly melt and reflow.
- Once you're done, keep the station on so others don't need to wait to heat it up. Pull the board off onto the piece of wood. We don't want to bump the parts off while the solder's still molten. Because that would be like, super sad. And we're in this for happy vibes 💖

If your electrolytic caps or inductors don't get all the way there, that's totally okay! We can hit those with a soldering iron afterwards :)

# Stashin'

You're probably not going to get through the whole thing in one go. Your boards are going to be safer chillin in lab for a few days then they would be floating around in your backpack, so we'll ask you to leave your boards in lab rather than take then with you. We'll show you where to stash them. Be sure to stick your name on your board before you stash it.

# Debuggin'

This depends on your project! We had the bringup plan so you aren't completely on your own here :)

# Programmin'

## Powering up stuff

USB-C is smart... a little too smart. Back in the olden days, USB-A (the thing that requires an average of 3 attempts to insert, thanks Intel) just sent 5V. _Technically_ they had some current limit and some negotiation stuff, but let's be honest-- most devices didn't follow that spec. USB-C on the other hand, is a little more picky. It does some handshaking and other data exchange before power is sent. This means we need to configure our device to accept 5V from a power source. Our device is a power "sink" since it receives power in and converts it into heat and other stuff. This is on the right side of the diagram. 

The CC lines form a resistor divider! On the side that the CC is connected, the CC voltage is some voltage between 5V and GND. On the side the the CC is not connected, the pull-up on the source (the thing providing the power) pulls the unconnected CC line high to 5V. This allows for the detection of the orientation of the USB-C port. USB-C has some fancy features if you need say Displayport that needs orientation identification but we don't need that. All we need is for both Rd resistors to be 5.1K to get 5V. If you plugged in your USB-C cord and you don't get 5V, this would be the first thing to check. Note that USB-C to USB-A plugs forgo this handshaking and simply send 5V. If your CC lines don't work properly, an A to C cable may still provide power. 

![](CC.jpg)

Image credit: [Silicon Labs](https://community.silabs.com/s/article/what-s-the-role-of-cc-pin-in-type-c-solution?language=en_US)

## Sendin' Data

Recall that GPIO 0 (BOOT) is a bootstrapping pin. This means that if IOO0 is low when the microcontroller turns on, it goes into "wait for a computer to send it programming data" mode. If we want to go into this mode, we have to hold down the boot button (bringing it low) and keep it held while we press and let go of the reset button. This turns the microcontroller on with BOOT low, allowing it to go into download mode. 

If your device is soldered and connected right, it should show up on your computer as a USB - Serial device. If that shows up, great! You can send it a program on Arduino and be on your way :) If not, you might want to check the D+ and D- pins. Here are the settings, and your COM port might be different. Specify
- ESP32S3 Dev module
- USB CDC on boot
- 8MB Flash
- PSRAM disabled
- MSC on boot and DFU on boot disabled

Once you finish programming, you may have to hit the reset button to get it to run your program. 


![](ProgrammingSettings.png)