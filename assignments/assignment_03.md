---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Assignment 3 - Footprint and Part Number Assignment
description: In this assignment, we're going to be telling the ECAD software exactly what each symbol in the schematic represents. A symbol is just a symbol, the software may not know exactly how the part looks like on a PCB. That's why we need to do footprint assignment. We also need to add part numbers to each part to ensure that they can be ordered from Digikey. 

author:
    title: Footprint Assignment guide
    title_url: '../footprint_assignment'
    description: A guide on how to assign footprints to a symbol

# Micro navigation
micro_nav: true


---

# Suggested Reading

- Chapter 2 of <u>Designing Electronics that Work</u> covers component selection. We also have an [in-depth component selection guide](../../lectures/ComponentSelection) to help you choose components!

- Chapter 4 of <u>Designing Electronics that Work</u> covers schematic capture. We also have a summary of things to check down below to make sure that we can catch as many mistakes as possible. 

# Part Number Entry

We want to make sure that an exported BOM (Bill of Materials) from your ECAD software will be able to be uploaded to Digikey. We have a few resources to make this easier for you, but we'll still expect you to do most of the work. 

## Resistors

Please label resistors in the following format. Examples are given: 

|Float(Value)|Multiplier|space|Ignored Text|What to name it|
|-|-|-|-|-|
|5.1|K||1%|5.1K 1%|
|0.1|M||[text]|0.1M [text]|
|100|K|||100K|

The multipliers are:

|Letter|Multiplier|
|-|-|
|m|0.001|
|R|1|
|K|1000|
|M|1000000|

Float just means it's a number with a decimal. Generally, you'll want the float(value) to be larger than 1 and smaller than 1000. We don't care about where the decimal is in the tool, but it's easier to read if you follow that format. 


We'll be running KiCAD BOMs through [Winnie's Resistor Naming tool](https://github.com/wszeto9/KiCAD_Lib/blob/main/ResistorDigikeyPNSearch.py) . You can feel free to use it to grab Digikey PNs for parts after assigning them footprints (SMD 0603 resistors most likely). Be aware that this can generate part numbers that may not exist. For this reason, we recommend you upload your BOM to Digikey to ensure that it all goes through fine. 

## Capacitors

This guide refers to multilayer ceramic capacitors (MLCCs) ONLY. These are the small surface mount ones. If you're using an electrolytic capacitor (the big cylindrical ones), name it whatever you want and add the appropriate Digikey part number to the BOM. 

Please label MLCCs in the following format. Examples are given: 

|Value|Multiplier|space|Voltage Rating|space|Other specs|What to name it|
|-|-|-|-|-|-|-|
|0.1|uF||16V||X7R|0.1uF 16V X7R|
|100|nF||25V|||100nF 25V|
|1|uF||6.3V||Y5V|1uF 6.3V Y5V| 

### Capacitance value

For capacitance values, try to stick to a power of 10. For instance, 

- 1nF
- 10nF
- 100nF
- 1uF

It reduces the permutations we have to deal with. For specific things like compensation networks on buck converters we understand, but otherwise ***please don't make us order 100 different capacitors per person***. 

### Multipliers
The possible multipliers are:

- uF (with a u as in you, not $\mu$ as in mu)
- nF
- pF

If you're doing something outside of this, please send a link for where you're getting a 1mF surface mount capacitor or a spec of dust with 1 fF (femto-farad) of capacitance. 

There's arguments for whether it should be called 0.1uF or 100nF. We don't care, our code can (eventually) process them.

### Voltage Rating
Please stick to these voltage ratings in this class:

- 6.3V
- 16V
- 25V
- 50V
- 100V only if you can justify it over a 50V rating

Again, we want to reduce combinations of all possible capacitances + voltage ratings + dielectric ratings of parts we need to order. We'll always get you a part of equal voltage rating or higher, and we'll decide voltage ratings of ordered parts when we order them. For dielectrics, we won't pay attention to temperature ratings. 

### Dielectrics

For Class I vs Class II capacitors, (C0G/NP0 vs X5R/X7R) we'll accept reasonable requests for filters. We'll let you know in advance whether we accept your request or not. Otherwise it will be whatever we choose. We will ***NOT*** be getting Y5V capacitors, that's all you'll need to know for most applications. 

### Package Size

Be sure to check that the capacitance you want can be achieved easily in the size you put down for the footprint. For instance, it's not possible to get a 1uF 100V capacitor in an 0603 package. We'd like you to stick to 0603 packages as much as possible, and only go bigger if you can't find a 0603 package with that specification you want. 

### Digikey Part Numbering

There's lots of variables to keep track of! For this reason we won't need you to input a specific digikey part. We'll search for them for you and order you MLCCs that either meet or exceed the specified voltage rating. 

# Footprint Assignment

See our [Footprint Assignment Guide](../footprint_assignment) for how to assign footprints. 

# PCB Room Layout

This is a lot simpler than you may think. Move around the rooms around as you wish until you're satisfied with how it's laid out. You don't need to start orientating components and such yet, we're looking for a very high-level look at how you want to organize your board. You should ***probably*** sketch a 100mm x 100mm rectangle as your board size at this point, so you know what you need to fit it into. We'd like a screenshot of your board organization in the software, and we also want you to annotate on top about how it's organized. Here's an example:

![Room Layout](./RoomLayout.png)