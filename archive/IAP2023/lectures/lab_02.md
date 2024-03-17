---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Lab 02 — Schematic Capture + Design Case Study, the Solar Car BMS
description: Full credits for design go to Jaeyong Jung, Francis Wang, Quang Kieu, + Cece Chu, special thanks for letting us use this in the class!

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

<div class="callout callout--danger">
NOTE: this is the pre-lab required reading for lab 2. Please read all of it and if you have questions shoot us an email or post on the course Piazza.
</div>

![](schematic.jpg)

# Battery Management Systems

[PDF Files of Schematic + Layout for the SEVT Headboard](https://drive.google.com/file/d/1KLIEmnSQEz9ZXyKeI9kjRNrVwL-6MiZv/view?usp=drive_link)

A battery management system (BMS) is a critical component of all electric vehicles. It is responsible for making sure the battery doesn't explode. To fully understand a BMS, we need to understand battery cell chemistry, we'll go into that briefly, we'll go into the functions of a BMS briefly, and then we'll get to the main part of the case study.

The purpose of this case study is to show you the schematic of a complex system. A battery management system has many parts, and requires some level of structure for it to be readable. So we're going to look @ the schematic in detail, and learn from it to see what clean schematics look like for complex systems. We'll also see some critical design principles for battery protection systems along the way.

[Here's the slides for a presentation on the overview of the solar car battery](https://docs.google.com/presentation/d/1Kz0ML4x0C5iZSwR3hdiNHJDAZImNCSLtzxcJBjrCptE/edit#slide=id.ga4e51f29ad_1_0)

## A Brief Brush-up on Batteries

![](babbling.gif)

Babbling Bumbling Band of Baboons, Babbling Bumbling Band of Baboons... Ok so there's no baboons, I'm sorry. The alliteration is, however, quite pleasing. And it does give me an excuse to include a gif of Professor McGonagall.

This is A [GREAT RESOURCE](https://batteryuniversity.com/articles) for batteries.

Also, briefly, here's a note on different [Lithium battery configurations.](https://www.power-sonic.com/blog/lithium-battery-configurations/)

Finally, here's a [list of battery types](https://en.wikipedia.org/wiki/List_of_battery_types), we will focus on Lithium batteries as these are the most common battery type in electric vehicles today because of their superior energy density (electrical energy stored per kilogram of battery weight). We will have a [more detailed resource on batteries](../lecture_y) later, but for now, here are the essentials.

### Packs, Modules, Cells

![](cmp.jpg)

As you likely know, electric vehicles have large batteries that store energy. The battery itself is also referred to as a "battery pack" which is the entire unit that gets installed in the car. The "pack" is made of modules, which are collections of "cells." A Lithium battery cell looks like this.

![](cell.webp)

The cells are wired in series (voltage adds), and parallel (capacity adds) to create modules. Modules are wired in series and parallel to create packs. [Here are some fun videos on that if you're interested](https://www.batterysystems.net/knowledge-base/series-vs-parallel/).

Batteries use chemicals to store energy, this is a Lithium Ion battery cell which means it uses Lithium (Li+) ions to store that energy. We won't delve into battery chemistry here but if you're interested, [here's a link](https://letstalkscience.ca/educational-resources/stem-in-context/how-does-a-lithium-ion-battery-work). These batteries are rechargeable.

### Model of a Lithium Ion Battery Cell + Voltages

The most basic lithium battery cell looks like the following, electrically.

<img src="batt_model.png" width="30%"/> <img src="discharge_curve.gif" width="60%"/>

A battery has two parts, an ideal voltage source (this is called the chemical voltage of the battery, this is related to the actual energy stored in the battery), and an impedance (or resistance, because ALL things have resistance). A Lithium battery cell usually has a max chemical voltage (or charge voltage) of 4.2V/cell, and a minimum of 3.0V/cell. If you charge the cell above 4.2V/cell it explodes. If you discharge it below 3.0V/cell it will be permanently damaged. The chemical voltage of a battery cannot be directly read because of the battery's internal resistance. The voltage we read if we take a multimeter to the terminals of a battery is called the terminal voltage. To get the true chemical voltage, we need to adjust for the impedance.

$V_{terminal} = V_{chemical} - i_{cell} R_{cell}$

From this, if we measure the voltage of a battery when it isn't connected to anything, we can assume that $i_{cell} = 0$ and $V_{terminal} = V_{chemical}$.

A fully charged battery starts @ 4.2 V/cell, and then the voltage drops as it discharges according to a discharge curve (like the one above).

<div class="callout callout--danger">
Note that a battery cell will not limit itself to these voltages. If the battery is at 4.2V/cell and you keep charging it, or the battery is at 3.0V/cell and you keep discharging it you will damage the cell. It has no internal protections. This is one of the many jobs of the BMS, to ensure the cells stay in this voltage range.
</div>

### Ratings of a Lithium Battery

Lithium battery cells also generally have current ratings in the form of "continuous" and "burst" current ratings. These limits are thermal like we explained in [Lab 01](../lab_01/#calculating-trace-widths--spacing). A BMS's job is also to protect a battery from exceeding these ratings.

Batteries also have thermal ratings, if a cell gets too hot either from current draw or environmental factors (it's just hot outside), you can damage the cell. Therefor BMS systems also need to consider thermals.

## BMS Functions Summary

So, in short, the job of a BMS is to do the following.

Fundamentally,

- Monitor the voltages of each cell
- Monitor the currents of each cell
- Protect against over-charging the cells
- Protect against over-discharging the cells
- Protect against drawing too much current from the cells
- Protect against the battery getting too hot

Additionally,

- Report the status of the battery pack to higher level control systems in the vehicle
- Manage the cooling system, charging system, and other subsystems of the battery

#  the Solar Car Headboard

With this in mind, we can go ahead and analyze the schematic of the solar car BMS.

## Schematic Organization

Once again, here is the [PDF Files of Schematic + Layout for the SEVT Headboard](https://drive.google.com/file/d/1KLIEmnSQEz9ZXyKeI9kjRNrVwL-6MiZv/view?usp=drive_link) linked for convenience. You can refer to this, we'll include screenshots as well.

The headboard schematic is what we refer to as a [Mutli-Sheet schematic](https://www.altium.com/documentation/altium-designer/multi-sheet-multi-channel-design). Basically, this means the circuit is complex and it's been separated into different sheets for readability, and also to minimize confusion. Often in design we talk about "separating the problem into smaller problems," and Multi-Sheet is a way to do that. For example, we can design the entire logic circuit without thinking about how it'll be powered, and then design the power separately. It's also commonly used in industry to divide tasks.

### Breaking down the Problem

![](multisheet.png)

This schematic is separated into 4 sub-schematics all of which are connected to each other. This highest-level schematic defines the connections between all of them.

- High Power System + Measurement (HV_Power.SchDoc)
- Microcontroller, Logic, + Communications (AT90CAN.SchDoc)
- Connectors (IO.SchDoc)
- Low Power System (Power.SchDoc)

Now we're going to analyze the sub-schematics. I'll spend the most time on the high-power system, the lower-level schematics we will briefly discuss to show how the schematics were organized.

### High Power System + Measurement (HV_Power.SchDoc)

![](hv_power.png)

This is the most complicated of all the schematics in the set, so don't stress if you don't understand it immediately. First, let's think about what this system has to do. The headboard is a PCB that goes IN BETWEEN the modules of lithium cells and the rest of the car to protect it in case of emergency. The only way we can really "protect" the batteries is by completely isolating them from the rest of the system. So, if we detect the battery voltage going above 4.2V/cell, or we detect a current that isn't safe, the battery gets disconnected from the rest of the car.

Note that this is for a solar car, which has a battery, a solar array, and a motor. So we're looking for three ports, one to connect the battery, one to connect the solar array, and one to connect the motor.

<img src="pack.png" width="50%"/>

This is the connector for the battery modules to be connected to the headboard. Note there's a piece of hardware protection included here, a fuse. If the entire battery management system fails to isolate the battery, eventually the fuse will blow protecting the battery. This is an example of redundancy in electrical design.

<img src="array.png" width="50%"/>

This is the connector where the solar array (and the charger that's connected to the array) connects to the headboard.

<img src="motor.png" width="50%"/>

This is the connector where the motor controller connects to the headboard. Notice how the motor connector has two diodes on it and the array and battery connectors don't. These are TVS diodes, they're another form of protection, and I'll talk in more detail about them below.

In real life, this looks like the photo below, that might help you build a little intuition.

![](batt.JPG)

We also said this circuit needs to be able to provide the functionality to disconnect the input (the connector that goes to the battery modules) from the outputs (the solar array, and the motor controller).

<div class="callout">
The solar array is considered an "output" in this case just as a distinction. Yes, power will flow in from the solar array to charge the battery.
</div>

The way we isolate circuits is by [using large relays also called contactors](https://drive.google.com/file/d/167bf2Oga6XQfRxsUvz10BrjtJP2OItEU/view?usp=drive_link). A relay is an electrically-controlled, PHYSICAL SWITCH, which is different from a MOSFET or transistor. A piece of metal physically moves away from another piece of metal when a relay turns off producing a large air gap which means better isolation. MOSFETs don't do this.

A battery has two wires, a positive, and a negative. To FULLY isolate the battery we need to disconnect BOTH the positive and the negative. We want isolation to be as if we were unplugging the battery completely from the rest of the car.

![](hb_annotated.png)

This was implemented on this headboard using three contactors. A Motor (+), an Array(+), and a Battery (-). And based on the way the schematic is drawn (where the contactors were placed) we can turn off and on the Motor and the Array separately from each other, but we only need one ground contactor to fully isolate the battery. If an issue is detected anywhere all three contactors open. If we just want the array to turn on, Array (+) turns on, and Ground turns on.

<div class="callout callout--success">
Note, the point here is, the way you draw the schematic can save you on the number of components you need to use AND determine the functionality you get. For example, if we only used two contactors on the Batt+ and Batt- line we could get full isolation, but we couldn't turn the array and motor off individually. Similarly, if we used four contactors, two on Motor+/Motor-, and two on Array+/Array-, then we could get the same functionality but we'd be using an extra contactor. Contactors take up a lot of space and cost money!
</div>

<div class="callout">
If you're wondering how the solar array charges the battery, that's a good question. The solar panels are hooked to a power converter that is basically a "smart battery charger." Therefor, all we need to do is hook the output of this converter directly up to the battery modules to charge from the solar array. That's why the circuit can be this simple. If the power converter wasn't there, we'd need to implement that power converter on the board (if we plugged solar cells directly into the battery). This system is called a Maximum Peak Power Point Tracker or an MPPT. Google it if you're interested.
</div>

#### Pre-Charge

![](precharge.png)

Now you may have noticed these two circuits in the bottom of the schematic. These are called pre-charge circuits and it's very common in battery systems, especially high voltage systems.

This is a [GREAT guide from TI on why precharge circuits are necessary in HV systems](https://drive.google.com/file/d/1MU8QTyj0ERECmJMW62hZ59FNra-V9n0l/view?usp=drive_link).

<div class="callout">
The long and short of it is, motor controllers, power electronics, and similar systems generally have large capacitors on them for filtration as well as protection of circuit components. But, if these capacitors start empty, and you connect a large battery to them, you'll see a HUGE spike of current when you first connect them because the capacitor needs to charge. This is bad for the electronics, but it can also wrongly trigger the overcurrent protection on the BMS. In short, the car will never turn on. So a pre-charge circuit charges these capacitors up slowly before connecting the main battery directly.
</div>

But I'm showing you this circuit for a different reason. I'm showing you this because it's a good example of how to organize a schematic cleanly. A pre-charge circuit is generally placed in parallel with the contactor that turns on the high-side of the output port. For example, the K3 contactor turns on the Motor+, the motor precharge would be in parallel with K3 because precharge bypasses K3 and then turns on K3. It charges the motor, and then connects it to the battery.

Knowing this, I could have just as easily made this my schematic, right?

![](seperation.png)

But this looks properly awful, and it's very hard to see if things are connected correctly.

<div class="callout callout--success">
So in this schematic, the creators separated the circuit off to the side, and used net labels to connect it. This is a good use of separation and net labels in a schematic sheet.
</div>

#### A Bad Schematic Thing

There's a bad thing in this schematic.

![](bad.png)

This is bad. Can you see why?

<div class="callout callout--success">
In a schematic, if two wires cross and you see a BLUE DOT on the crossing location. That means the wires are connected.
</div>

In this case the wires are crossing, but we don't WANT them to connect. The circuit you see here is a current sensor. If Batt- was connected to Battery_GND then we would be shorting out the current sensor and it wouldn't be able to measure current, current sensors need to be in SERIES with the circuit. [How Current Sensors Work](https://www.tek.com/en/how-to-measure-current)

<div class="callout callout--danger">
In Altium, if you cross two wires they will automatically connect. If you don't want things to connect you shouldn't even give Altium the CHANCE to make a mistake. DO NOT CROSS WIRES THAT YOU DON'T WANT TO CONNECT TO EACH OTHER. In large schematics, mistakes like this are easy to miss. We got lucky, you may not!
</div>

#### Some Extra Easter Eggs

If this is already overwhelming, feel free to skip this section. This section just contains some extra "good design" principles for circuits shown in this schematic for those of you interested.

##### Why are there like, 6 Grounds?

Yes, why are there like 6 grounds?

Honestly, a lot of it is just to make it immediately clear on the schematic what is what. But it does bring up an important point.

![](grounds.png)

Note in this schematic that the power ground is different from the grounds going to things like current sensors, AND that the ground above the ground contactor is COMMON_GND and the one below is BATTERY_GND. Why does this matter?

<div class="callout callout--danger">
Even though the contactor is a switch that's connecting a ground to another ground, and when its closed those grounds will be the same, this is an important consideration. When there are sensors, microcontrollers, switches, in your circuit, it's important to know what grounds they are referencing. For example, the circuitry that turns ON the ground contactor, needs to reference BATTERY_GND, if it referenced COMMON_GND then it could only turn on the contactor when the contactor is on, which doesn't work. Similarly, the sensors and the microcontroller must also share the same ground because to read accurate data from the sensor they need to have the same reference.
</div>

##### TVS Diodes

<img src="tvs.png" width="50%"/>

Like mentioned earlier, these diodes on the motor are called TVS diodes or Transient-Voltage-Suppression. Essentially, if they see a voltage surge too high, they short and protect any circuitry in front or behind them.

For those of you familiar with electric vehicles, you're likely familiar with regenerative braking. But what happens if you regeneratively brake faster than the battery can sink the current? Remember the capacitors from the precharge section? Well they accept any energy that doesn't go into the battery, they start to charge and their voltage begins to rise. If the voltage goes too high, things can explode. TVS diodes prevent them from overcharging.

##### Another Funky Diode?

Note that the array contactor has a diode in parallel with it.

![](flyback.png)

This diode is called a flyback diode, and a good explination of it is contained in [Lecture 05.](../lecture_05/#interrogating-a-circuit-example---diodes)

But some of you smart cookies might be looking at K3 and K5 (the motor and the ground contactor) and saying "well they don't have flyback diodes."

That's because these contactors are larger and have in-built flyback diodes.

### The Rest of the Schematics

<div class="callout callout--danger">
In an effort to not throw too much at you, I won't explain the rest of the schematics in the level of detail I did the first one. But here's some things to think about.
</div>

Look through the rest of the schematics, skimming is fine. Try to ask yourself the following questions.

- How did the designers minimize the number of connections between different sheets in the schematic? Are there connections between schematics you think are unnecessary? Could they have minimized more while keeping the simplicity?
- How did the designers separate the components to make design easier to THINK about? Are there components where if moved to another sheet could've actually been harder to design if you think about too many things at once?
- Do all the net labels have a reason? Are any extraneous and could have been omitted from an electrical standpoint?

# Lab Assignment

<div class="callout callout--danger">
This is a message for those of you who scrolled past all the text above straight to the lab assignment.

U thoughttttt we wouldn't find out did ya?
</div>

<div class="callout callout--success">
Now it's time to start the schematic of your bluetooth speakers! Use what you learned from above to help with your schematic capture!
</div>

Use the following [PDF Schematic](https://drive.google.com/file/d/15hAap-ClR8PHIWUSimQq3ruf3WlNjQC3/view?usp=drive_link) and the [Bill-of-Materials](BOM.csv) to create a schematic in Altium Designer for the Bluetooth Speaker project. Note that we chose to do a multi-level schematic and chose to separate the schematic in this way. You may choose to make your schematic differently (we even encourage it!) but there's a few things that you might get tripped up on:

##### Components Window vs. Manufacturer Part Search

In the last lab, we asked you to find all of your parts with Manufacturer Part Search. And for good reason! We wanted to show you that your parts physically exist, are purchaseable, have 3D geometry, and have a footprint on the circuit board. We're going to do something a little different here, and it's what you'll see in the real world with projects of moderate complexity or higher.

If you noticed, we had a _lot_ of choices for resistors in lab01. We gave you the JST connector and LED, but there were _thousands_ of choices for the resistor, and a lot of them are functionally equivalent - or equivalent enough for our purposes. Imagine that we were a big company with a bunch of designs floating around, and each needs a 320 Ohm, 1/4W resistor. Unless they agree on some standardization, each board designer will select a different resistor. This will cause a bunch of chaos when it comes time to order parts - fundamentally we should only have to order only one kind of resistor, but we wouldn't know that if all we have is a bunch of random resistor part numbers. And lord knows the chaos that'll happen as those parts go in and out of stock. Not to mention how irregular the CAD is going to look between designs, and what happens if we use a part that has a bad footprint?

We solve all of this by making generic parts. For instance, if I wanted a 240 Ohm resistor in a 1206 package with a 1% tolerance that can dissipate a 1/4W, I could:
- Import a part with these specs, like a [ERJ-8ENF2400V](https://octopart.com/erj-8enf2400v-panasonic-55529380?r=sp) into my schematic from MPS.
- Instead take a generic 1206 resistor, copy it, and tell Altium that it should have a 240 Ohm resistance, a 1% tolerance, and a 1/4W power rating. These generic templates are shared amongst the teams working on their boards. Since everything is copied from a template, the CAD between a 1206 resistor on two different designs will be exactly the same. And, when it comes time to order the boards, all of our parts say _240 Ohm 1/4W 1% 1206 Resistor_ instead of _ERJ-8ENF2400V_. Much easier to read, and it lets us easily choose a different part in case our original one was out of stock, or if we found a better price on another one.

The nice thing about this is that our CAD now better conveys our _design intent_, namely, what we're trying to say with the CAD that we've made. What we want here is a resistor that fufills some specs - what's important is the specs, not that ERJ-8ENF2400V fufills them.

Anyway, what does this mean for you? It means that we've used generics to make every resistor and capacitor that you'll need for the design! We've stashed all of these in Altium 365, and they show up in the 'Components' panel, accessible from the 'Panels' button on the bottom right. We've thrown in some other specialized parts too, including the:
- battery holder
- programming header
- boost converter
- audio amplifier
- testpoints

We'll ask you to grab these from our Altium workspace library instead of the Manufacturer Part Search. Everything else is available on MPS.

##### Multi-sheet Schematics

If you are having trouble with Multi-Sheet designs see the [following Altium tutorial on Multi-sheet](https://www.altium.com/documentation/altium-designer/multi-sheet-multi-channel-design). Note that multi-sheet schematics use PORTS in addition to NET LABELS, and this combination may be a new concept.

Another good tutorial on [Multi-Sheet](https://drive.google.com/file/d/16tZdB46X8dFuilo9EVWy4x1VIbjzr87c/view?usp=drive_link) from the University of Florida.

##### Differential Pairs

Note that this schematic also has differential pairs for the speaker outputs and USB D+/D- lines. Take a look at this [Altium tutorial on Differential Pairs](https://www.altium.com/documentation/altium-designer/interactively-routing-differential-pairs-pcb) to see how to include that in the design.

##### Ports vs Net Labels

Ports are used in Altium in Multi-Sheet schematics to make connections between schematic sheets. A net label exists only within a schematic sheet and from sheet-to-sheet, even same-name nets will not connect across sheets. That's why we use ports which are used to create a connection between sheets.

[Altium Tutorial on Ports vs. Nets](https://www.altium.com/documentation/altium-designer/creating-circuit-connectivity-schematics)

<div class="callout callout--danger">
In Altium, each port on a sheet must have a UNIQUE NAME.
</div>

<div class="callout callout--danger">
Note, however, that power nets are GLOBAL and do not need ports cross sheets but only if they are defined as power nets with the power net symbol. This means you don't need a port to access the 3.3V across sheets.
</div>

![](ports.png)
*Edited image from Altium Tutorials*

<div class="callout callout--danger">
So you don't NEED to do this and you should NOT. Because if you do this then Altium WILL isolate the 3V3 net to the sheet and you WILL need a port to connect across sheets. This is also how you isolate global nets if you don't want them to be global, by adding a port.
</div>

# Tuesday Design Review Assignment

<div class="callout callout--success">
Your final schematic is due Tuesday in lab. We will be design-reviewing your schematic one-on-one. Don't stress too much about it, we just want to make sure you grasp the basic concepts of schematics and we just want to shy you away from any crimes you may have committed along the way.
</div>

<div class="callout callout--danger">
Design reviews will be TUESDAY during lab, you will be soldering practice PCBs and we will pull you aside for design reviews one-by-one :)
</div>

##### Inductor Saturation

[Look @ this Tutorial on Inductor Saturation!](https://www.monolithicpower.com/en/how-to-avoid-inductor-saturation-in-your-power-supply-design)

# References

[Battery University](https://batteryuniversity.com/articles)

[Battery Hookup](https://batteryhookup.com)

[Lithium Battery Cells, Modules, Pack](https://www.acc-emotion.com/stories/battery-cell-module-or-pack-whats-difference-infographics)

[Adafruit, Lithium Batteries](https://learn.adafruit.com/li-ion-and-lipoly-batteries/voltages) --> THIS IS A REALLY GOOD TUTORIAL AS WELL IF YOU'RE CONFUSED