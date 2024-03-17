---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Lecture 05 - Population + Debugging!
description: Description

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
        content: Lecture 04
        url: '../lecture_04'
---

![](debugging.jpg)

# History of 'Debugging'

The term 'Debugging' actually referes to actual bugs that used to sit inside computers when they were purely electro-mechanical systems and interfere with operation. The term orginated when Admiral Grace Hopper found herself pulling a moth out of the Harvard Mark II computer in the 1940s.

![](bug_book.jpeg)
![](bug_book2.jpeg)
*These images are of the original bug, credited to the Smithsonian — National Museum of American History*

But in some sense, PCBs resolved problems like these. Bugs used to get on the wires of the old wire-wrapped, or point-to-point computers that we saw in Lecture 2 notes, and mess with signals. With no external wires to interfear with, your PCBs are safe should a bug land on them. But we still have other problems.

Debugging is really only necessary because we're never perfectly certain what's going to happen with our PCB. All models are wrong. And especially when dealing with real components, our "100H inductor" might not be exactly 100H, or it might saturate differently than the datasheet says it does.

OR, and more commonly, we might just forget to model something in design, or we just didn't know it had to be modeled (engineering is a science after all). All of these can cause issues in boards that we didn't forsee.

# Tools for Debugging

## Multimeter

<iframe width="560" height="315" src="https://www.youtube.com/embed/ts0EVc9vXcs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/lo8MWr3NuuM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Multimeters are the first tool you should pick up when having a problem with a circuit. They can give you incredible amounts of information very quickly and help you rule out possible issues fast. Here are a few types of checks multimeters can run.

### Voltage

Voltage is a super useful test where the multimeter tells you the voltage difference between the point on the circuit where you place the **red** probe, and the point on the circuit where you place the **black** probe.

<div class="callout">
Voltage tests can tell you things like if your components are getting power correctly, or if the output of an analog sensor (such as a current sensor) is as expected. For more complex things, it's better to use an oscilloscope or logic analyzer if possible.
</div>

### Continuity/ Resistance/ Diode Checks

Continuity tests are honestly one of the most useful checks while debugging. I use them to most just to sanity check that all parts of the circuit are connected. Sometimes one pin of an STM32 didn't reflow properly, or you need to check if a switch or relay or MOSFET is turning on and off correctly, and a continuity check is super useful for this.

<div class="callout callout--danger">
Do NOT run these tests while the circuit in ON. Remove all power sources from the circuit such as batteries and power supplies. Unless you're probing across a relay or contactor.
</div>

### Current

Current tests are not as useful while debugging but super useful when characterizing systems. Be careful with these since most multimeters can only do about 10A before they explode.

## Oscilloscope

<iframe width="560" height="315" src="https://www.youtube.com/embed/u4zyptPLlJI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/t_UTV_9dpTc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Oscilloscopes are super powerful tools for debugging. They can capture extremely fast analog or digital signals in high resolution. We use them very often in debugging because often we know what we think the signal SHOULD look like but we need to check if the circuit is actually producing that waveform. If you're just checking a signal that's supposed to be a DC voltage, generall you use a multimeter. But if you want to see a sine wave, a pwm signal, or something more complicated like the input and output of a filter you use a scope.

<div class="callout callout--danger">
You can even use a scope to probe how well your bypass capacitors are filtering your power rail. Put one scope probe right before the bypass caps on your circuit, and one right after and compare the signals!
</div>

<div class="callout">
Scopes have a lot of other cool features, some can take the difference between two signals, add or multiply signals. Some can even take fourier transforms! Some have in-built logic analyzers as well.
</div>

## Logic Analyzer

<iframe width="560" height="315" src="https://www.youtube.com/embed/cC8m9CoRXKw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Not really trying to do product placement here but the Saleae Logic Analyzer (and other logic analyzers) are amazing tools for debugging. Logic analyzers are perfect for debugging communication systems and other digital signals. Logic Analyzers can be used on communication systems like I2C, SPI, and UART to read and decode signals. You can actually see the values of the bits being sent over the wires using a logic analyzer.

<div class="callout callout--danger">
This can debug pretty much any communication system. Except for CAN Bus, we reccomend a PEAK CAN for that. Other differential communication systems also may need special tools to read.
</div>

## Power Supply

Now you may not think a PSU is a debugging tool, but it's more useful than you might think. Often times when we are getting bad signals in a circuit, or if parts aren't turning on, it's useful to be able to apply a voltage to the circuit to test components or to make sure the right voltage is being supplied. For example, if a circuit runs on a battery and you don't know the battery's voltage, or if you're reading from an analog sensor and you're afraid the output might be wrong. To test the circuit you could inject a correct, known voltage to test the rest of the sensor and conclude the sensor or the battery are faulty.

Note, the [MIT Solar Car Battery Characterization Document](https://www.adim.io/_files/ugd/067a81_00780811c8bb441c8df4d58ae436911a.pdf) shows the use of a power supply to fake a current signal to test a Battery Management System.

[MIT Solar Car Battery Characterization Document](https://www.adim.io/_files/ugd/067a81_00780811c8bb441c8df4d58ae436911a.pdf)

# Strategy for Debugging

Note that there's no real, set strategy for debugging. Good debugging requires an understanding of the laws of physics, how circuits work, and sometimes even down to the component level. The more you've debugged circuits the better you get both at debugging, but also design. The best design principals for electrical systems are usually extracted from debugging boards. **Everytime someone tells you "you should do this," in electrical design. Try to imagine the situation that caused someone to realize that, reason it out.**

But, we can give you a couple tools to guide you in how to begin debugging a circuit. These things apply for anything you notice "isn't working."

- check continuity, turn off the board, and make sure the things you expect to be connected to each other are connected. Let's say a communication signal is not being sent, or something isn't getting power. Probe from the LEG of the chip to the LEG of the other chip, NOT from pad to pad. Testing pad to pad will not rule out a bad solder joint which is often the case. A lot of issues come just from bad solder joints.
- check voltages, make sure everything is getting power, make sure the analog signals (maybe signals coming out of sensors) are correct.
- then you can break out things like a scope or a logic analzer, use these to probe signals to make sure they are what you expect them to be.

DEBUGGING IS PURLEY ABOUT PROBING. PROBE PROBE PROBE YOUR CIRCUIT. Each time you probe something you learn something, even if you think something is fine, probe it. It's one less thing to worry about. The greatest barrier in debugging is remembering to pick up the multimeter. Just make sure it's on the correct mode XD.

## Interrogating a Circuit Example - Diodes

**Another good thing to do is try to ask "why" certain things are in a circuit when you look at other people's designs.** For example, on many circuits with relays, you'll see a diode connected to the relay activation contacts like this.

![](flyback.png)

*Image from [Stack Exhange](https://electronics.stackexchange.com/questions/204332/bi-directional-flyback-diode-for-relay-spike-protection)*

Remember, everything that is in a circuit was placed there deliberately. Just taking the diode out would be irresponsbile and dangerous engineering. So let's ask why the diode is there? This is a great video that visually shows that the diode does something.

<iframe width="560" height="315" src="https://www.youtube.com/embed/5kjtiY9gxGM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Note that when the relay is switched off, the diode lights up briefly. So why? What's it doing? See [Flyback Protection](https://en.wikipedia.org/wiki/Flyback_diode). Also look at [how a realy works](https://www.explainthatstuff.com/howrelayswork.html).

<div class="callout">
Basically, an inductor produces a voltage proportional to the change in current across it $V = L \frac{di}{dt}$. If we look at how a relay works we see that it's basically an electromagnet. And one side of the electromagnet is a coil of wire, which is an inductor. When we turn off voltage to the relay, the voltage of the inductor spikes, and if there isn't something to dissipate that voltage the relay can explode. (summary of the article)
</div>

Sooooo, let's say your PCBs relay isn't turning on anymore but it worked the first time. You might say "but hey, it worked before! Why wouldn't it work now!" Then you look at your circuit and you notice you forgot the flyback protection diode. Does this explain why your relay only worked once? Is there a test you can run to see if the relay is blown?

<div class="callout callout--success">
(Hint, continuity test across the input to the relay, it should conduct because an inductor is just a wire).
</div>

# Case Study

But the best way to learn debugging by far, is to see it happen. So we'll show you below in the case study. The best way to learn is by doing. In lecture itself we will present the case study of the Solar Car BMS.

<div class="callout">
For the 2021 car Nimbus, there was a phase where when we turned the car on, the battery would throw an overcurrent error. Watch the lecture to see us systematically figure out why and how!
</div>

# This is an [AMAZING RESOURCE on Debugging Written by an AMAZING 6.101 TA](https://drive.google.com/file/d/1FUPEqvmuqaIFKmHTXmBfa5qj5Ma4Eikx/view?usp=drive_link)

# Population Order for Debugging

Population is putting the components on the PCB itself. You learned the act of population in [Lab 03](../lab_03) but somtimes you can save yourself debugging time in the future if you approach population in a more systematic way.

- don't just start shoving all the components on the PCB mindlessly
- first start by seperating the problem, you know everything needs power, start with the power converters and populate all of those
  - then check the pads to make sure the power is populated correctly
  - this is to avoid you blowing components out if your power converters are WRONG or need adjustment
- after that start with the most critical module, either the microcontroller or something else
- think about if you have connectors on this board and what they are made of
  - if you have connectors like JST connectors, and they are made of plastic, you want to populate those last or you'll melt the plastic (even XT90s/XT60s will get mad at you)
  - think about HOW you'll populate things, the plastic connectors should be done with a soldering iron, small passives might be done best with a hotplate, and larger stuff with a re-flow oven or a hot air gun
- try to minimize the heat you put on the board, that sounds counter intuitive, but too much heat can damage the board, be slow, deliberate, and check as you go
  - for example, if you populate a 555 timer on your board, check the output after powering it on

DO NOT JUST PUT ALL THE COMPONENTS ON THE BOARD AT ONCE AND HOPE IT'LL WORK, THIS IS NOT A GOOD STRATEGY BECAUSE IF SOMETHING GOES WRONG, YOU WON'T KNOW WHAT IS WRONG.

We'll do an example of this as part of the later case-study

## Population Strategy Example

Let's take a look at the PCB below, the same Torque-Sense CAN board from the layout lecture!

![](ts.png)

Let's list the parts of the circuit
- There's a power connector
- There's a buck converter on the board that takes a 20V input and steps it down to 5V
- There's a linear regulator that takes the 5V and steps it down to 3.3V
- There's a torque sensor connector
- There's a resistor divider network that takes the -10-->+10V signal from the torque sensor and converts it to a 0-3.3V analog signal
- There's a CAN Bus connector
- There's a circuit that drives the CAN Bus communication system
- There's a microcontroller, programing header, and oscillator that reads from the resistor divider network and sends it out to the CAN circuit

### Order of Population + Testing

So for a board like this, this is the strategy I would take.

1. Populate all the components for the 5V converter EXCEPT for the screw terminal power connector. Connect alligator clips to the power input and test the output of the 5V converter. (I always populate connectors last because they're easy to damange, and once you populate them the board no longer lies flat on the table).
2. Populate the 3.3V LDO and power the board with the alligator clips. Test the output of the 5V converter AND the 3.3V LDO.
3. Populate the components for the resistor divider network except for the connector. Connect a power supply with alligator clips to the input of the resistor divider network and vary the voltage between -10V --> 10V and make sure you get the correct 0-->3.3V analog signal output going to the microcontroller.
4. Populate the microcontroller and the CAN Bus network. Populate the programming header, program the microcontroller with "Hello World!" and check it works.
5. Populate the CAN circuit.
6. Populate the rest of the connectors.

# References

[Debugging + Harvard Mark II](https://en.wikipedia.org/wiki/Debugging)

[Circuit Debugging Techniques](https://resources.pcb.cadence.com/blog/2022-two-critical-circuit-debugging-techniques)