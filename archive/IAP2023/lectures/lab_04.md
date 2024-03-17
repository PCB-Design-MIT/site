---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Lab 04 — Layout!
description: Begin the layout of your Bluetooth Speaker PCB!

# Micro navigation
micro_nav: true

# Page navigation
page_nav:
    prev:
        content: All Lectures
        url: '../../lectures'
    next:
        content: Lecture 02
        url: '#'
---

# Lab Overview

In this lab you'll be starting the layout of your PCB. The goal is to use the content we presented in [lecture 3](../lecture_03.md) to properly place and orient components on the board, and connect them with traces, vias, pours, planes and other connectivity components. Your goal will be to get familiar with the layout process.

<div class="callout callout--success">
In this lab we will link a whole bunch of tutorials and point out specific things to consider for the layout of the speaker, but for the most part you'll be on your own. We want you to try layout instead of copying ours. I'll link to a few boards that you should look at that have examples of good layout on them, and some tutorials on layout considerations for audio systems.
</div>

<div class="callout callout--danger">
Note you will be using YOUR SCHEMATIC, so please make sure you run it by one of the instructors in case you have doubts about anything. 
</div>

<div class="callout callout--danger">
We WILL be Design Reviewing your Layout! So do the best you can for now! Google examples of good PCB layout! Be inquisitive! And don't be afraid to try things!
</div>

The easiest way to learn is just to DO.

# General Tips for this Specific Layout

## DAC Layout Considerations

The PCM5102APWR is a DAC or Digital-Analog Converter. It takes digital audio signals from the ESP32 and converts them to analog audio for the speaker driver chip (the TPA3116D2). 

<div class="callout callout--danger">
Try to keep this chip as chip as close to the ESP as possible but also try to minimize the length of the audio traces coming out of the DAC and going into the Amp.
</div>

## USB + Programming Header

The CP2104-F03-GM is a USB-Serial Adapter that allows us to communicate with the ESP32. 

<div class="callout callout--danger">
Try to keep the CP2104-F03-GM USB-Serial Adapter, as well as the programming header close to the ESP32. But note the actual USB plug also goes to charging the battery, so you may want to keep that near the battery charging circuit as well.
</div>

## 0-Ohm resistors + the Audio Jack

Note resistors R39 and R40 are 0-Ohm resistors. These resistors are use to selectively connect and disconnect the 35RAPC3BH3 Audio Jack for debugging purposes. While we won't use the audio jack after the rest of the speaker is working, we will use it for initially testing the amplifier.

<div class="callout callout--danger">
We want to keep these as close to the audio jack as possible BUT we also want to be able to take off and put on these resistors easily. A 0-ohm resistor is like a little switch we can populate to turn a part of the circuit on, but we don't want to damage any other part of the circuit when we try to put it on or take it off. Consider how much space you need to remove this resistor.
</div>

## Considerations for Trace Sizing

<div class="callout callout--danger">
Note that the signal stuff you probably don't need to consider trace width. But for power converters and audio, you may need to consider bigger traces. How do you size a trace? How would you go about finding the current that a trace needs to take? Could the datasheet of the component tell you? 
</div>

## Power Considerations

<div class="callout callout--danger">
There's a large switching converter on the board, what things will get upset if we put a switching converter near them? What things won't? Can I place my ESP near the converter? Can I place the audio amplifier?
</div>

## Mechanical Considerations

Note there are some components like the battery holders and the audio jack that go through the board. Make sure you leave space for this. Where do you want the battery holders? On the top or on the bottom? 

# ESP 32 Layout Guidelines

Note the ESP32 has a large metal case around the outside that acts as a faraday cage to block noise, as such it's placement near power converters is not as much of a problem as other microcontrollers. However, it's a Bluetooth-enabled and connected device which means it has an antenna, and that requires additional layout considerations.

This is a [really good guide on guidelines for laying out ESP32 chips](https://www.espressif.com/sites/default/files/documentation/esp-wroom-02_pcb_design_and_module_placement_guide_0.pdf) from Espressif.

<div class="callout callout--danger">
If you read nothing else, read the above PDF because it has guidelines without which your layout WILL NOT WORK.
</div>

Here's a [longer-format guideline](https://www.espressif.com/sites/default/files/documentation/esp32_hardware_design_guidelines_en.pdf) on the Espressif ESP32 Layout Guidelines.

# Audio Engineering Tips

Here's a [fun tutorial from Hackster](https://www.hackster.io/arshmahmughal77/better-quality-of-sound-with-class-d-audio-amplifier-pcb-1816db) on layout of an Audio Amplifier.

Check out this video from texas instruments on laying out Audio systems.

<iframe width="560" height="315" src="https://www.youtube.com/embed/2JdJ5uoxuU8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Check out [this Application Note from TI](https://www.ti.com/lit/an/snaa057c/snaa057c.pdf?ts=1674059307220) on power supply layout for audio amplifiers (specifically the TPA3116D2 we are using).

Check out [this guide on managing EMI](https://www.ti.com/lit/an/snaa050a/snaa050a.pdf?ts=1674084541833&ref_url=https%253A%252F%252Fwww.ti.com%252Fproduct%252FTPA3116D2) also for the TPA3116D2 from TI.

More details on the amplifier can be found [here](https://www.ti.com/product/TPA3116D2#tech-docs).

And here are [some layout techniques for a DIFFERENT TI Class-D amplifier](https://www.ti.com/lit/an/slaa896/slaa896.pdf?ts=1674084736972&ref_url=https%253A%252F%252Fwww.google.com%252F).

# List of Relevant Altium Tutorials

## Component Placement

[Rotating and Positioning Components](https://www.altium.com/documentation/altium-designer/simple-pcb-component-placement)

[Advanced Placement](https://www.altium.com/documentation/altium-designer/advanced-pcb-component-placement-tools)

## Layers

[Using Vias](https://www.altium.com/documentation/altium-designer/pcb-via)

[Layer Stack Manager](https://www.altium.com/documentation/altium-designer/defining-layer-stack)

[Creating Full Power + Ground Planes](https://resources.altium.com/p/creating-ground-plane-your-pcb-design)

## Routing, Traces, Pours

[Routing + Connectivity, Traces, + Trace Width](https://www.altium.com/documentation/altium-designer/interactive-routing-pcb?version=18.1)

[Copper Pours](https://www.altium.com/documentation/altium-designer/pcb-polygon-pour?version=18.1)

## Mechanical

An easy way to change the board shape is to go to "View-->Board Planning Mode" or by hitting the "1" key as a shortcut. That will let you modify the board shape if you go to "Design-->Redfined Board Shape."

[Mechanical Layers in Alitum](https://www.altium.com/documentation/altium-designer/working-with-mechanical-layers)

[Mounting Holes in Alitum](https://my.altium.com/altium-designer/getting-started/creating-pcb-mounting-holes)

[Dimensioning Mechanical Components](https://www.altium.com/documentation/altium-designer/pcb-linear-dimension?version=21)

## Why is there a big red box on my layout and why do my components turn green when I drag my components outside it?

![](Sync_Rooms.png)

Note that you can also just delete the rooms as well, but we reccomend you lay out your components in rooms first. 

[Tutorial on Altium Rooms](https://www.altium.com/documentation/altium-designer/working-with-rooms)

## My Pads are GREEN

Green is bad in Altium. 

Yes, I know, it's strange. 

So for many smaller chip footprints Altium gets mad because it thinks YOU have drawn traces too close together when in reality it's fine. Most of the time Altium just sets very conservative limits to the pad clearances.

Go to Design --> Rules 

Then Clearance Checks and follow ALL the drop downs until you get here:

![](cl.png)

Then select "Ignore Pad-to-Pad Clearences within a Footprint," hit Apply and then OK.

# Additional Resources to Look @

Look at the [last few lecture slides from lecture 3](https://pcb.mit.edu/static/slides/lecture_03.pdf) to see some examples of good and bad layout.

A [fun instructables on PCB Layout for microcontrollers](https://www.instructables.com/Designing-a-Microcontroller-Development-Board/) from beginning to end.