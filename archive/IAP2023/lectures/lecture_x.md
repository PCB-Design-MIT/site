---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Motors + Motor Control - an Overview

# Author box
author:
    title: Electric Motor Control
    title_url: 'https://drive.google.com/drive/folders/17ZYPjCDqWwoHHAgkh1itcahMD45OL6t0?usp=share_link'
    external_url: true
    description: Good resource on motors from Sang Hoon Kim

# Micro navigation
micro_nav: true

# Page navigation
page_nav:
    prev:
        content: All Lectures
        url: '../../lectures'
    next:
        content: Lecture 02
        url: '../lecture_02'
---

# Basics of Motors + Motor Control - what are they, and what do we want to do?

A motor is an electro-mechanical device that takes electrical energy and converts it to mechanical energy, and vice-versa. Motors are super cool! They spin! And understanding how a motor works is critical for certain systems, especially if you want to design a motor controller.

The lecture video itself contains a more step-by-step model of how we can develop a good model for motors and how we can control one. Here we want to take a higher-level approach introducing the field of motors and motor control, and link resources to study.

A GREAT resource on motors + motor control is Sang Hoon Kim's "Electric Motor Control" linked above.

# Types of Motors + Motor Basics

There are many types of motors including axial flux, radial flux, brushed, brushless, induction, IPM, SPM, etc. Here's some resources on that as well as modeling and torque-speed curves.

[Vex Robotics - brushed vs brushless](https://motors.vex.com/brushed-brushless)

[Modeling of Brushed DC motors](https://www.mathworks.com/help/sps/ref/dcmotor.html)

[A good paper on Brushed Motor Modeling](https://drive.google.com/file/d/1yK2YJyAGMZpoVxdyXv__SH19uF0OUzrd/view?usp=drive_link)

[Motor torque-speed curves](http://lancet.mit.edu/motors/motors3.html)

<iframe width="560" height="315" src="https://www.youtube.com/embed/pxtRlKs0pAg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

# Field-Oriented Control

Field-Oriented Control is a technique for controlling brushless motors that allows us to maximize the torque output that we can get from a motor. The lecture video covers field-oriented control for electric motors so see that for details, but here are some great links for motor control.

[Clarke + Park Transforms from TI](https://drive.google.com/file/d/1TJm3IjwkJ36Gmnd_SNLLhFojkMOnO8Z3/view?usp=drive_link)

[James Mevey 2009 Masters Thesis - "A Legtimately Excellent Read" as said by Dan Kouttron, thanks Ben Katz for sharing!](https://drive.google.com/file/d/1dh10l1d56yAShU-4keCYaprz6aKJLl-C/view?usp=drive_link)

[Clarke + Park Transforms](https://www.mathworks.com/solutions/power-electronics-control/clarke-and-park-transforms.html)

[FOC](https://www.mathworks.com/solutions/power-electronics-control/field-oriented-control.html)

[Space Vector Modulation](https://www.mathworks.com/solutions/power-electronics-control/space-vector-modulation.html)

## Field-Weakening Control

Field Weakening Control is a type of motor control under Field-Oriented Control that allows us to artificially weaken the magents to get higher speeds. Field-Weakening produces a magentic field along the d-axis of the park transform to COUNTER the field of the permanent magnets in the rotor allowing the motor to spin faster.

Wild right? Here's a good tutorial on how that works in code.

[Field Weakening Control](https://www.mathworks.com/solutions/power-electronics-control/field-weakening-control.html)

And here's a fun example in real life of mechanical field-weakening!

<iframe width="560" height="315" src="https://www.youtube.com/embed/Q7fC_DmLZ30" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

This is a Variable Field Inductance motor from FreeEnergyLT, thanks for sharing w/ us Chris Evagora! Basically what is happening here is that pulling the stator away from the rotor increases the effective gap distance, which increases the Kv of the motor, which decreases the Kt, makes the motor run more efficiently at speed, but it reduces torque production.

According to Austin Brown "This is a type of field weakening but it's pretty bad because you waste half the motor." "A better version is an [axial flux motor](https://en.wikipedia.org/wiki/Axial_flux_motor) that moves its stator in and out." But most manufacturers have determined no field weakening is good enough "IPMs tend not to field weaken well anyways."

# Switchcraft + SVM

Once the clarke and park transforms generate the iq and id we want, we need to actually apply that to the motor. One technique for that is called Space-Vector-Modulation or (SVM).

[Great FOC Tutorial](https://www.switchcraft.org/learning/2016/12/16/vector-control-for-dummies)

[Great SVM Tutorial](https://www.switchcraft.org/learning/2017/3/15/space-vector-pwm-intro)

[Another Good SVM Tutorial](https://www.pscad.com/webhelp/Master_Library_Models/HVDC_and_FACTS/Space_Vector_Modulation/SVM_Theory.htm)

More stuff on SVM in these videos:

<iframe width="560" height="315" src="https://www.youtube.com/embed/hSXSY4LRizg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/oHEVdXucSJs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

# Parts of a Motor Controller

So now that we kinda understand how a motor spins, let's look at a block diagram of the ideas presented above and see if we can turn this into a PCB design. That is, what does a motor controller need to do? And then, what parts are in a motor controller, and why are they there?

![](FOC.jpg)
*FOC Block-Diagram from MathWorks*

Let's make a short list of things we need to know to control a motor using FOC.
- The phase currents of the A, B, and C phases
- The position of the rotor
- The DC voltage at the input of the motor controller

Other signals such as iq, id, the duty cycles, and etc, are all generated using the math above such as the clarke and park transforms, current controllers, and SVM. The list above contains RAW VALUES we have to MEASURE IN HARDWARE because these are the basic values we base all the math off of, these are the input variables.

In the design of a motor controller, we need all the power electronics required to drive the motor phases of course, but we also need any hardware essential to measuring the key variables we need for performing the math for commutation.

[VESC-6 Schematic PDF](https://drive.google.com/file/d/16O-hxTDGhnvwSedbBEbmxXxsFFhFzCVE/view?usp=drive_link)

Let's look at the VESC-6 schematic above and see if we can tell how each of the variables are measured.

## Measuring Phase Currents

We know from the section on SVM, we use a total of 6 MOSFETs to generate the signal to drive the phase lines of the motor. So if we wanted to see how to measure the phase currents, we could start by looking at the power schematic of VESC-6.

![](current_sense.png)

This is a part of the VESC schematic that shows ONE of the three pairs of MOSFETs driving the phases. The **PH_A** label connects to the phase wire of the motor. But also notice, that in this schematic (compared to the video on SVM above) instead of the phase wire connecting directly to the point in-between the two FETs, it has a resistor in series. This resistor is called a SHUNT resistor and it's usually a resistor of very small value. And a chip called the [AD8418](https://drive.google.com/file/d/124yJBAg5k8feAOqsyjhC7tQGRbTesAqS/view?usp=drive_link) is connected across the resistor.

The AD8418 is what we call a current shunt amplifier. Essentially, it reads the voltage across the shunt resistor and converts that into a signal that can be read by a microcontroller. The voltage across the shunt resistor will be proportional to the current going through the resistor, and because this resistor is in series with PHASE A of the motor, this part of the circuit is reading the phase current of PHASE A of the motor.

We can see that there are three total shunt current measurement circuits, one for each phase. The outputs of which all go to the microcontroller.

![](currentABC.png)

Note we want to use a very small resistor in this case, something that's close to the value of the phase resistance because other wise we'll be just wasting power by sending a whole bunch of current through the shunt resistor. Higher resistances will also make the motor harder to drive and reduce the performance of the motor controller. If the shunt resistor is small, we can neglect it in modeling.

## Measuring Bus Voltage + Phase Voltages

The easiest way to measure voltage is to use a resistor divider circuit with a set of known resistances driven to an ADC pin on a microcontroller. The resistor divier takes the voltage input and reduces it to a voltage proportional to the main voltage trying to be read, but in the range of the microcontroller's allowable inputs. For example, a 3.3V microcontroller has an ADC that can input values from 0-3.3V. Any more than that you'll ruin the microcontroller.

![](voltages.png)

Note these four voltage dividers that measure the three phase voltages as well as the main bus voltage. These voltage measurements allow us to set duty cycles accordingly in the SVM and give us the ability to do some feedback control around the actual output voltage of each phase.

Note that we want these resistances to be as large as possible, again for the same reason we don't want to burn too much power drawing current into a voltage divider. For voltage, the resistor is in PARALLEL, so we want it to be LARGE to draw minimal current. For current measurement the resistor is in SERIES so we want it to be SMALL to dissipate minimal power.

## Measuring Rotor Position

Guide on encoders is coming soon, but basically you generally need an encoder for precise control, low-speed control, or control of large brushless motors. Some small motors, and high speed applications like drones, don't need the sensors to do commutation and they can do Field-Oriented Control sensorless.

See [James Mevey's Master's thesis](https://drive.google.com/file/d/1dh10l1d56yAShU-4keCYaprz6aKJLl-C/view?usp=drive_link) for more on sensorless FOC.

See [Ben Katz's Master's thesis](https://drive.google.com/file/d/1k_Dxy-WPMjK2TlIn_dJqv-ot8wSdtmQC/view?usp=drive_link) for more on general FOC and design of motor controllers.

### Calibration Routines for Brushless Motor Controllers

For calibration routines and why calibration is important, a longer explination is coming soon, but see [the later slides of this presentation](https://drive.google.com/file/d/1j3ViOslegbMaBozjofxeMBwNLz1HG_6U/view?usp=drive_link) for the Biomimetic Robotics Lab @ MIT.

## Selecting Gate Drive Chips + MOSFETs

This section is also coming soon. But generally you have the Texas Instruments DRV series which is the best on the market. Trinamic motion control also makes some good products you can check out that I have not personally used.

Another cool chip to try is the STM series of integrated STM and DRV chips, like the STSPIN32G4.

The [hardware for Ben Katz's motor controller is open source](https://github.com/bgkatz/3phase_integrated) and you should totally check it out. As is the hardware for the [ODrive v3.1](https://github.com/odriverobotics), and the [VESC](https://vesc-project.com/documentation). Note that the Ben Katz controller and the ODrive are tailored to precise control for robotics, VESC is targeted at scooters and vehicles.

# Motor Structure + Design

Motors have a standard set of parameters that determine their performance, we design to these parameters, but they also have many considerations associated with them. Here are some.

[General selection of poles and slots in motor design](https://things-in-motion.blogspot.com/2019/01/selecting-best-pole-and-slot.html)

[Power + Transmission Design MIT 2.75 - look @ the middle section about motors](https://meddevdesign.mit.edu/wp-content/uploads/simple-file-list/FUNdaMentals-Chapters/FUNdaMENTALs-Topic-7.pdf)


Also from Austin Brown, our resident Motors Guru "Axial flux motors are strange. A lot of times they look really good on paper but suck in real life. It seems like there are 100 startups all touting their "physics breaking" axial flux technology but as before none of the major auto manufacturers are doing it, probably for a reason. Axial flux motors are really annoying because on a big motor there is literally a ton of force trying to squish the motor together, and this means the motor has to be very heavily constructed. A lot of axial flux motors are double-sided for this reason, but unfortunately this makes them electrically very difficult to drive because of low inductance. It seems in general they are used for niche applications very well (wind turbines, hub motors, maybe some other weird ones) but other than that just the normal IPM plus gearbox reigns supreme."

So maybe things like the [quark motor from koenigsegg](https://www.roadandtrack.com/news/a38940998/koenigsegg-quark-electric-motor/) won't be replacing the motors of all EVs in the future.

![Motor from a BMW i3 - Austin Brown](bmwi3.PNG)

Also from Austin "The above image is a BMW I3 motor. The most efficient motors seem to be of this style: basically reluctance motors with a little bit of permanent magnet thrown in. Using less permanent magnet hurts your torque density, but makes the motor more efficient at high speeds because there is less magnet to fight"

MORE from Austin

"Outrunner vs Inrunner: this just means whether the stator is on the inside or the outside, in general it seems that inrunners are more efficient. Outrunners have better torque density though. Inrunners are also a lot easier to cool. If waterproofing is anywhere on your radar, you need an inrunner (or an outrunner in a can which is more or less and inrunner) so generally inrunners are used for everything outside the hobby market where waterproofing is necessary"

Inrunner is more efficient because --> "I don't have a good answer to why inrunners are generally more efficient but it just seems like over all the motors I've seen they are better, but its the same fundamental physics (steel, magnet, and copper) so hypothetically they should be comparable" --> that's what I thought too

IPM vs SPM --> "Regarging IPM vs SPM: SPMs are generally more efficient below base speed, but field weaken extremely inefficiently. So, for a fixed speed application (like an electric plane), people generally go with SPMs (and make the Kv exactly right for that application), whereas for something with a varying speed like a car you go with an IPM"

"The base speed of a motor is the nameplate speed at the rated voltage and full load. load varies from no load to full load over the stated speed range. Example: 1% of base speed is 17.5 RPM." --> [source](https://acim.nidec.com/drives/kbelectronics/-/media/kbelectronics/documents/technical-notes/general_performance_specs.ashx?la=en)

[Motor Poles + Slots Design from Richard Parsons](https://docs.google.com/spreadsheets/d/1K11hwqNGIjCnSvQtaQrRqfzwzPwThnfMw5cCNv4pqCI/edit#gid=352296252)

Here's a GREAT video on the Tesla Motor design!

<iframe width="560" height="315" src="https://www.youtube.com/embed/esUb7Zy5Oio" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

# Dynamometers + Motor Testing

So you might be wondering with all of these different types of motors, how do we actually determine the performance of motors? Generally we use something called a dynamometer that measures both the mechanical power output of a motor given an electrical power input, as well as the efficiency of the motor. There's a great guide on Dynamometers linked below!

[Dynamometers for Dummies - Adi M.](https://evt.mit.edu/textbooks/Dynos_for_Dummies.pdf)

# Other Resources + Projects to Look @ !

[Great Video on Magents 1 (thanks Quang!)](https://www.youtube.com/watch?v=1TKSfAkWWN0)

[Great Video on Magents 2 (thanks Quang!)](https://www.youtube.com/watch?v=hFAOXdXZ5TM)

[STM Cube FOC SDK](https://www.st.com/en/embedded-software/x-cube-mcsdk.html?icmp=tt6644_gl_bn_mar2018#sw-tools-scroll)

[VESC Docs](https://vesc-project.com/documentation) note that VESC motor controllers are better for control of electric vehicles including scooters, they aren't so good for robotics control.

[ODrive v3.1 + Earlier](https://github.com/odriverobotics) ODrives are amazing robotics motor controllers, they became closed source after v3.1 but before then are a super awesone resource.

[Ben Katz](https://github.com/bgkatz) Ben has some amazing resources on motors and motor control and he designed the MIT Mini Cheetah, his motor controller and motor controller firmware are used in many dynamic robots around the world today.

[Licence to Fab](https://licence-to-fab.github.io) is an MIT team that will have more resources on motors and motor control coming out soon.

