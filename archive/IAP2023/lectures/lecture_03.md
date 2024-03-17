---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Lecture 03 - Layout!
description: as a concept really,

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
        content: Lecture 02
        url: '../lecture_02'
    next:
        content: Lecture 04
        url: '../lecture_04'
---

# A brief overview of Layout

Layout is the FUN part of PCB design! And, after circuit DESIGN, it's 90% of the actual work of making a PCB. I like to call layout "Introduction to Urban Planning for the Distinguished Electrical Engineer," mostly because that's EXACTLY what it is.

I actually believe this SO strongly I'm going to [link the UN Habitat Urban Planning Guidelines here](https://drive.google.com/file/d/1V-q8P66GEc7KPKx-xTfWt8QwH2_byPAH/view?usp=drive_link), and challenge you to find the parallels. Additionally, because it's a great example of how knowledge in the ARTs can help knowledge in the SCIENCEs and vice-versa.

## Your PCB is a City of Tiny Electrons

Think of your PCB as a city. The electrons are the people and they have places to go, things to do. The components are buildings where they go do their business, and the traces are the roads that take them there. Bear with me as I stick with this analogy for a brief amount of time to explain some basic concepts.

- Cities have zones for different types of land use, industrial districts, residential districts, the idea being to keep activities in the areas they need to be in without affecting life in other areas. PCBs are the same, we seperate the board area based on the types of use. Power goes in one spot, digital signals goes in another, and nalog has it's own space as well so there's no interference or overlap. (We'll talk about this more below).
- The best cities have short and direct roads with relevant buildings placed near each other. People are lazy, so are electrons, and most don't like walking far distances to get where they need to go. People can get distracted and meander off-path, electrons can pick up interference that won't let them do their job as effectively. Therefor our job as PCB designers will be to place relevant components as close together as possible to minize the lenght of the traces in-between them.

I'll save you from more analogies for now, but you are likely starting to get the idea. I encourage you all to channel your inner artist while doing layout. A good layout is function, but also BEAUTIFUL. It puts the "Art" in the Art + Science of PCB design.

# Location of Components

The **FIRST STEP** once you start your layout is placing and orienting all your components.

<div class="callout callout--danger">
In general you want to place your components on the board before routing any traces, traces are hard to move so you want all components in their FINAL positions before you draw any traces.
</div>

We'll start at a high level and work our way down.

## Seperation of Power and Signal

<div class="callout callout--danger">
Seperation of power and signal is one of the most important concepts in PCB design, it has implications for signal integrity, and is one of the most significant ways engineers try to reduce overall system noise.
</div>

Let's talk power breifly. There's many types of power converstion but primarily we have LDOs, and switching converters. Switching converters are more popular because they're more efficient.

Here's a [guide on the differences between them](https://resources.altium.com/p/using-ldo-vs-switching-regulator-your-pcb).

Here's a [good guide from TI on how switching regulators work.](https://drive.google.com/file/d/1W8xwsvt-NFQj7knwTwtVT6ma03AO7J8O/view?usp=drive_link)

<iframe width="560" height="315" src="https://www.youtube.com/embed/rfChSvb8FX0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

The main point here, if you look at switching converters, is producing power is NOISY. You have this transistor banging a voltage back and forth, and if we take a fourier transform of this signal, we'll notice frequencies being emitted equal to the switching frequency and many other higher frequencies super-imposed on each other. The NOISE in general is something we don't want to interfere with the other electronics on the board right?

- If we have a digital circuit, noise being thrown off the power converter can affect our digital circuit causing errors in communication and voltage values if the digital circuit is too close to the power electronics.
- Digital being close to the power electronics is STILL BETTER than analog being close to the power electronics. Noise in the power rail near an analog circuit is usually devastating to the analog signals which are very susceptible to any kind of environmental noise and electromagnetic interference (EMI).

An AMAZING BOOK on EMI is [Henry Ott's Electromagnetic Comaptability Engineering](https://drive.google.com/file/d/12jCbfX23SYodxIgUaihjDQ7uPrS7U0BY/view?usp=drive_link). Well worth a read.

![](sep.png)

These images above are of a BMS and of an MPPT, see how the power stuff is on one side of the board, and the digital control circuitry is on the other half. **NOTE that even if the digital control circuitry is controlling the power electronics, we still produce a level of seperation.**

<div class="callout callout--danger">
Based on this, we get the idea of "Seperation of Power and Signal" where we designate areas of the board where "power systems" will live, areas of the board where "digital" electronics will live, and areas where "analog systems" will live. We keep these away from each other where the analog is usually the farthest from the power.
</div>

And yes, this is all so operation of a circuit in ONE part of the board does not affect whatever a different circuit is doing in another part of the board.

Kind like how **you don't want a landfill next to your house,** that would be pretty upsetting. **The same way a bangin' transistor next to your bangin' stereo amplifier will produce some bangin' noise.**

## Orientation + Placement of Components

So that's how we seperate parts of the circuit, how do we place components with respect to each other? And we start with the concepts of inductance loops.

### Inductance Loops

Inductance loops start with Faraday's law of Induction. So here's a video on that.

<iframe width="560" height="315" src="https://www.youtube.com/embed/LDOa7UdfcMQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Based on the law of induction, we know that magnetic field travelling through a loop of wire causes a voltage. Now we use this to our advantage in motors, but we don't want this on our PCBs, [unless we're designing a wireless charger PCB](https://www.digikey.com/en/products/detail/tdk-corporation/WRM483265-10F5-12V-G/10484695?utm_adgroup=Wireless%20Charging%20Coils&utm_source=google&utm_medium=cpc&utm_campaign=Shopping_Product_Inductors%2C%20Coils%2C%20Chokes_NEW&utm_term=&utm_content=Wireless%20Charging%20Coils&gclid=Cj0KCQiAlKmeBhCkARIsAHy7WVupswmVzOTKHJAiNPVbbXzhVAHjdQO5kDG6H5V5BFke9k_db6nXiDMaAviaEALw_wcB).

![](inductance_loop.png)

Magnetic fields resulting from EMI can come from many places in the world. Here are some examples

- Animal from the Muppets playing the drums in [bohemian rhapsody](https://www.youtube.com/watch?v=tgbNymZ7vqY)
- A nearby power converter
- You're beloved Xfinity Home Internet

Basically, we want to minimize the existance of these inductance loops on a PCB, which means minimizing the distance a trace travels, and how "circular" that trace is.

<div class="callout callout--danger">
We minimize the loops by placing and orienting components intelligently on a PCB.
</div>

This is actually an [AMAZING TUTORIAL on Inductance loops form Hackaday.](https://hackaday.com/2018/09/28/inductance-in-pcb-layout-the-good-the-bad-and-the-fugly/)

### So now, how do we place and orient components?

I usually begin with the schematic. Let's take a look at this buck converter.

![](buck.png)

Take a look at the components connected to the central chip of the above buck converter. I'm going to start by taking all the components that surround the chip in the schematic and putting them in a little group. Then I'll orient the components to minize the trace length between them.

<div class="callout callout--danger">
I do this for all the parts of the schematic and then I start moving the groups to places on the board that seem resonable while accounting for seperation of power, signal, and analog.
</div>

Let's look at a different example of a PCB that reads the voltage form a torque sensor and publishes it on a CAN-Bus network.

[A simple intro to what CAN Bus is.](https://www.csselectronics.com/pages/can-bus-simple-intro-tutorial)

![](ts.png)

Here's the board in question with some of it's good features labeled. I want to zoom in on the power area and the signal area to look at how we oriented components and drew traces.

![](analog.png)

Notice here how we oriented the resistors in this network to connect them with traces that were as short as possible, and the side of resistors that went to ground faces away and uses a VIA to go to ground. We placed the signal path we care about, the signals going through the resistor amplification network, as close as possible so we could draw the traces.

You'll notice there are some capacitors on the bottom of the board, this is in general a bad idea, and we'll get to why later.

![](power.png)

You can kinda see the same thing in the power converter, all the components are very close together and they are oriented so the trace lenght can be minimized. We tried to minimize loops in the circuit as well, in a few places we could have done a better job.

Our layouts aren't perfect, we're still learning too!

# Bypass (concept + location)

![](bypass.png)

So you'll also notice on some of our designs we have these weird capacitors. Many of them, connected to chips. What are these? These are called Bypass, or Decoupling capacitors. Remember how we said that switching converters flick on and off really fast to convert voltages? And there can be some noise in the output? Chips don't like this, especially audio chips and microcontrollers so these capacitors filter it out.

You can watch the lecture video for an intuitive explination of bypass capacitors using very hungry sharks, but we'll explore some technical resources here:

[Bypass + Decoupling Capacitors](https://components101.com/articles/decoupling-capacitor-vs-bypass-capacitors-working-and-applications)

[Basics of Decoupling Capacitor Sizing](https://resources.altium.com/p/what-size-decoupling-capacitor-should-i-use-my-digital-ics)

[Sizing Bypass Caps](http://www.learningaboutelectronics.com/Articles/Bypass-capacitor-calculator.php)

[TI Guide to High Speed Communication Bypass](https://drive.google.com/file/d/1Y-lPiZ3IWTPrCSyFOFSvpBZ790rJ0_h5/view?usp=drive_link)

[Micron Bypass](https://drive.google.com/file/d/1Z6TZKYR_Pf_Ep2NpNiXLp1XO7fD4rpy2/view?usp=drive_link)

Some general rules:

- Large capacitors deal with lower frequencies, smaller capacitors deal with higher frequencies.
- Electrolytic capacitors have large parasitic inductances and resistances which only allow them to work at low frequencies.
- We usually use multiple capacitors in parallel to sink a large range of frequencies so only the DC component of voltage comes through.

## Location of Bypass Capacitor + Traces to Bypass Capacitors

![](bypass2.png)

Here's a [great tutorial on where to place bypass caps](https://www.protoexpress.com/blog/decoupling-capacitor-placement-guidelines-pcb-design/). And I've listed some general rules below.

- In general, you want bypass as close to the chip as possible.
- Multiple bypass capacitors are usually placed next to each other.
- Many chips have multiple power pins, we want bypass caps for each of the pins and we want the positive side of the capacitor as close to EACH of the VDD pins as possible.
- Some designs place bypass caps on the BOTTOM of the board UNDER the chip. This is not a great practice as it introduces inductance loops that go through the board. There is some debate on this though.

[Another GREAT PDF from Analog Devices, this one is on Bypass + Decoupling.](https://drive.google.com/file/d/1P8pGBPy3BDr_Xnjm46JIoyFQednPesm7/view?usp=drive_link)

# Connections

## Trace Sizing, Trace Spacing

There's three main things to consider when laying down traces, weight, size, and spacing. The first two are coupled, the third is independent.

These are super important to make sure your PCB works! The width and weight of a trace determine its current capacity. The spacing determines its ability to stand-off voltages without arcing between traces.

## Considering Current

### Trace Weight

Trace "weight" is a funny term we use for trace "thickness." That's the thickness of the copper layer on the PCB, how deep does the copper go? The "height" if you will when we start to calculate a trace's cross-sectional area.

"Oz" refers to 1.35 mil thickness of copper and it comes from the thickness of a layer required to lay down X ounces of copper on a 1sq-ft plate. This unit is likely used because it's helpful in calculating manufacturing cost.

[More info on the oz ratings of copper](https://www.pcbuniverse.com/pcbu-tech-tips.php?a=4)

[Good blog post on copper weight](https://www.z-zero.com/blog-post/actual-copper-thicknesses-as-opposed-to-what-youve-assumed/)

### Trace Width

[This is a great tutorial on PCB trace width calculation](https://www.mclpcb.com/blog/pcb-trace-width-vs-current-table/) from Milennium Circuits Limited (MCL).

Another [good table for trace widths](https://www.pcbcart.com/article/content/copper-trace-and-capacity-relationship.html).

Here's a [GOOD CALCULATOR](https://www.digikey.com/en/resources/conversion-calculators/conversion-calculator-pcb-trace-width) from digikey on trace widths for both INTERNAL and EXTERNAL layers.

### Trace Location

Note that trace width and weight are fundementally changing the resistance of a trace, which changes the power dissipated as heat. The better the trace can dissipate heat into the environment, the smaller it can be for a given current. So the layer of the board matters as well. We put high-current traces on the outer layers of PCBs to enable them to dissipate heat better.

<div class="callout callout--danger">
Note that all trace sizing for current is based on an accepted temperature rise. Depending on the resistance of the trace and the trace's ability to dissipate heat, current running through the trace will produce a certain rise in the temperature of that trace. It's up to us to determine what is GOOD or BAD for a given board. Sometimes temperature rises so much that it explodes :) !
</div>

In super high current applications we'll use [copper POURS instead of traces](https://www.altium.com/documentation/altium-designer/pcb-polygon-pour?version=18.1).

## Considering Voltage

PCB trace SPACING is based on VOLTAGE differences between two adjacent traces. You can read all about it [here](https://www.smps.us/pcbtracespacing.html). That's a really good tutorial on trace spacing.

Sometimes for really high voltage applications, [they'll put SLOTS in the PCB](https://www.raypcb.com/pcb-slot/).

<div class="callout">
Note above with the torque sense CAN board, you can see some of the traces near the power converter are much thicker than others. This is to help deal with the higher current areas of the board.
</div>

# A List of Many Many Layout Considerations

Our goal with this page is to give you things to google. Layout is a massive topic that takes practice to get good at. The only way to get good is by looking at examples of what IS good, and trying it yourself.

The following is a list of a whole bunch of other considerations during layout and design we've linked hear for your interest.

<div class="callout callout--success">
We LEARN BY DOING!!!!
</div>

## Examples of Good Layouts that are Open Source

[ODrive Motor Controller V3.1 - Altium](https://github.com/odriverobotics/ODriveHardware)

[Ben Katz Motor Controller](https://github.com/bgkatz/3phase_integrated)

[Ben Katz SPINE Communications Board](https://github.com/bgkatz/SPIne)

## Signal Integrity + Protection

### Impedance Matching

[Impedance Matching - EE Power](https://eepower.com/technical-articles/understanding-impedance-matching/#)

[More Impedance Matching](https://en.wikipedia.org/wiki/Impedance_matching)

[PCB Trace Impedance Matching - TI](https://training.ti.com/ti-precision-labs-pcb-trace-impedance-matching)

[TI - Even More Impedance Matching](https://drive.google.com/file/d/1XdFvH53Q_tHM1ZT5algW4vfVtaU4kA5j/view?usp=drive_link)

### Differential Pairs

[Differential Pairs](https://resources.altium.com/p/what-are-differential-pairs-and-differential-signals)

[Diff Pairs on LinkedIn](https://www.linkedin.com/pulse/differential-pairs-what-advantages-design-ritesh-kanjee/)

### Debounce

[Switching Bouncing + How to Prevent it](https://circuitdigest.com/electronic-circuits/what-is-switch-bouncing-and-how-to-prevent-it-using-debounce-circuit)

[Debounce Circuits](https://www.circuitbasics.com/switch-debouncing/)

### ESD

[ESD Protection - STM](https://www.st.com/en/protections-and-emi-filters/esd-protection.html)

[The 'Complete Guide' to ESD Protection](https://uk.rs-online.com/web/generalDisplay.html?id=ideas-and-advice/esd-protection-guide)

[System-Level ESD Protection - TI](https://drive.google.com/file/d/1wiBflOMdSXtGn2S07lr9yx3vFOF4iZCg/view?usp=drive_link)

### EMI

[TI - PCB Guidelines for Reduced EMI](https://uk.rs-online.com/web/generalDisplay.html?id=ideas-and-advice/esd-protection-guide)

[MCL - EMI and PCBs](https://www.mclpcb.com/blog/pcb-electromagnetic-issues/)

## Debugging + Manufacturing Resources

[MCL - Test Points](https://www.mclpcb.com/blog/test-points-pcb/)

[MCL - what affects PCB cost?](https://www.mclpcb.com/blog/technologies-affect-pcb-cost/)

## Mechanical + Thermals

[Thermal Design for PCBs](https://www.pcbcart.com/article/content/principles-of-thermal-design-for-PCB.html)

[Thermal Via Management in Altium](https://resources.altium.com/p/management-of-thermal-vias)

[TI - A Guide to Board Layout for the Best Thermals](https://drive.google.com/file/d/1WVD2JGjV-IDS68yk9H711gzqNZEaHIy5/view?usp=drive_link)