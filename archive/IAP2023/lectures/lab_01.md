---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Lab 01 — Introduction to Altium Designer
description: In this lab, we'll do a very basic design for a PCB just to get familiar with Altium Designer, it's tools, and the interface.

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

<div class="callout callout--warning">
NOTE: this lab is a CRASH COURSE and is really just to get you familiar with what is POSSIBLE in Altium. I'll link to external tutorials when I can but don't worry about understanding every detail of everything! Just think of it as dipping your toe into the North Altium Sea. It's overwhelming at first, but with time and practice you'll learn to sail too.
</div>

![Moana!](moana.jpg)

# Installing Altium Designer

The first step is to [Install Altium Designer](../../lectures/altium_setup/) if you have not done so already. If you are in the class, you should have already done this, and you should already be on the Altium 365 workspace for the class where we will hold a lot of the design resources - let us know ASAP if this is not true.

These instructions work for both MIT students as well as non-MIT affiliates with an @edu email address. [Altium is provided free for students + educators](https://www.altium.com/education/student-licenses).

<div class="callout callout--danger">
MIT Altium Virtual Machines will not work at the moment for non-MIT affiliates.
</div>

# Creating a Folder in the Altium 365 Workspace

Once you're added to the Altium 365 Workspace you should get an email with instructions to add the workspace to your Altium Installation and a link to the online Altium 365 workspace.

Go to the Altium 365 workspace and sign in, click "projects," "New," and "Create Folder."

![](create_folder.png)

Create a folder where the name is your kerberos!

![](folder.png)

Once you [connect to the Altium workspace](https://my.altium.com/altium-365/getting-started/connecting-altium-365) in Altium itself, you should see your folder in the workspace. Create a new project IN that folder or you can create a new project in the folder on the web interface.

Name your project Lab01_kerberos

[Here's another Altium 365 tutorial](https://www.altium.com/documentation/altium-365/managed-projects)


# Introduction

This lab is a super simple, super short lab! It's basically just to get you familiar with the following in Altium Designer.
- project structure
- schematic capture
- layout tools
- component libraries/manufacturer part search

We will be designing a very simple PCB from start to finish, not accounting for ANY parasitics, or following really any "good design principals." Remember, this lab is JUST FOR YOUR FAMILIARITY with the software. This is by no means a "well designed PCB."

The order you will do things in this lab is...
- select your components
- create a schematic
- create a layout of these components
- [define a board shape](https://www.altium.com/documentation/altium-designer/defining-board-shape?version=19.0)

<div class="callout">
Typically, when defining "board shape" we do this in a MECHANICAL LAYER called the BOARD SHAPE LAYER or MECHANICAL LAYER 1 so that when exporting the PCB to be manufactured the manufacturer's software can identify the mechanical layer easily, otherwise they'll probably email you with questions. **This is not necessary for this lab since we aren't manufacturing these, but is the accepted practice so you should do it this way.**
</div>

# Overall Design

In this lab we'll be designing a very simple board with an LED, a resistor, and a [JST header](https://www.digikey.com/en/products/detail/jst-sales-america-inc./B2B-XH-A(LF)(SN)/1651045?utm_adgroup=Rectangular%20Connectors%20-%20Headers%2C%20Male%20Pins&utm_source=google&utm_medium=cpc&utm_campaign=Shopping_Product_Connectors%2C%20Interconnects_NEW&utm_term=&utm_content=Rectangular%20Connectors%20-%20Headers%2C%20Male%20Pins&gclid=Cj0KCQiA45qdBhD-ARIsAOHbVdHVljx1WOLrFpzCbZRUbRvv9fbUMB_mJo-GNihIsqbWNY0-snzPAGcaAvZPEALw_wcB) (here's the [datasheet](https://drive.google.com/file/d/1zLJboW5hgA_aoW00pe7Z8NA9uS-hs0MI/view?usp=drive_link) in case the link is broken). Pretend you need a tiny PCB with an LED on it, maybe you're making a flashlight. But none of the power electronics are actually on the board. You'll use the JST header to send a voltage to the board (connect a power supply), the resistor will limit the current, and the LED will shine.

## How does an LED work?

Brief aside for those who don't know, anyone who does know can skip this section. LEDs shine with a brightness proportional to the current flowing through them. Most LED circuits have what is called a "current limiting resistor" which limits the current from the power source to an appropriate level for the LED. If the resistor is not there, the LED would burn out immediately when hooked up to a voltage source.

LEDs also have a voltage drop across them when they are turned on. This voltage drop is NOT proportional to the current through the LED nor the applied voltage, however is specific to the LED and is based on the internal characteristics of the silicone. We can draw the following circuit diagram for the LED, resistor, and power connector.

![circuit diagram for lab](LED.png)

If we look @ the [datasheet for a typical LED](https://drive.google.com/file/d/1RfvqDrj0AQ-POEH9POQfMgnB-iZPkdQp/view?usp=drive_link) we can see the values of the two parameters we care about, the forward voltage, and forward current per chip. In this case, the forward current needed for the LED to work is 20mA. Let's call this $ I_{fw} $. When on, the LED will have a voltage drop between 1.7-2.6V, let's call this $V_{drop}$.

Now let's assume we'll power the board with four AA batteries (if we're making a flashlight), that's a voltage of 6V if they're in series. Let's call this voltage $V_{in}$. The current can be calculated as follows. The value of the resistor is $R_{limit}$.

$I_{fw} = \frac{V_{in}-V_{drop}}{R_{limit}}$ (Ohm's Law)

That means the voltage across the resistor at MAXIMUM will be 6V-1.7V which is 4.3V, so if we want 20mA of forward current, $R_{limit}$ must be 215 Ohms.

<div class="callout">
Note that we took the lower bound of the typical LED forward voltage range, this is to ensure we don't accidentally under-size the resistor and send too much current through the LED.
</div>

# Component Selection + Manufacturer Part Search

<div class="callout callout--success">
Create a new project by going to "File"-->"New"-->"Project", name it "Lab 01_{your kerberos}". Then add a new schematic to the project by right clicking the project on the left hand pane and selecting "Add New to Project"-->"Schematic." Save the schematic as "Sheet1.SchDoc."
</div>

![Figure 1](lab1_fig1.png)

When designing PCBs, we need to remember that we work with REAL components. We need to be able to find these components, and purchase these components. Therefor PCB design is entirely design for manufacturing (DFM) where component selection based on what is avaliable is critical.

**A Schematic defines the components of a PCB and how they are connected** it also tells Altium what components the design uses. Altium has many in-built parts libraries that automatically pull the pin-out and the footprint of each part into your design. **A Pinout is the definition of what to connect to what pin for a part, the footprint is the physical shape of the pads on the PCB.**

## Altium In-Built Component Library

Altium has a built-in library for very basic components and footprints (such as basic resistors and capacitors). It can be accessed from the "components tab" on the right side of the screen.

<div class="callout callout--warning">
If you don't see the components tab, go to "panels" in the lower right corner.
</div>

<div class="callout callout--warning">
NOTE: we generally do NOT use this library unless we are looking for very standard parts such as through-hole resistors, or header pins. ALL SMD COMPONENTS SHOULD BE FOUND USING THE MANUFACTURER PART SEARCH
</div>

In this case we know our design has a resistor. Let's assume we're ok with using a through-hole resistor FOR NOW instead of an SMD resistor (we'll change it later).

<div class="callout callout--success">
Go to the components tab, and using the drop-down select "Miscellaneous Devices.IntLib." Then type resistor in the box.
</div>

![Figure 2](lab1_fig2.png)

<div class="callout callout--success">
When you search, you'll have many options. You should see a "Res1" and "Res2" both of which have a footprint labeled as "AXIAL." AXIAL is another name for a through-hole resistor because of its construction. Right click on it, select "Place Res1" and put it on a random place in the schematic file you opened. Hit "esc" to change your cursor back to normal.
</div>

This is an example of how to use the built-in component library. Feel free to scroll through other options in the library. Note there's another library called the "Miscellaneous Connectors Lib" which contains things like header pins (those things you have on an arduino). Sometimes those are very useful as well.

## Manufacturer Part Search — JST Connector + LED

Now we have to select the LED and the JST connector (feel free to select one of your choice but use the manufacturer part search).

<div class="callout callout--success">
Go to "panels"-->"manufacturer part search" and then search "LED" in the box. Many options should come up, and generally these will all be surface mount components. You only want to look at LEDs that have the green "chip" symbol next to them, this indicates Altium has a PCB footprint for that part already in the software.
</div>

![Figure 3](lab1_fig3.png)

<div class="callout callout--success">
Select the LTST-C190GKT from Vishay Electronics. Right click on it, place the component.
</div>

Select the "properties" tab on the right. Notice that Altium can direct you both to where you can buy this part, but also the [datasheet](https://datasheet.ciiva.com/pdfs/VipMasterIC/IC/LITE/LITES10869/LITES10869-1.pdf?src-supplier=IHS+Markit).

![Figure 4](lab1_fig4.png)

<div class="callout callout--success">
Now do the same for the JST S2B-PH-K-S(LF)(SN) connector. We'll use this connector to provide power to the board.
</div>

## Selecting the Resistor

<div class="callout callout--success">
Now, use the technique presented above and the datasheet of the LED you selected to calculate a value of the resistor. Note that you can choose any input voltage you desire but it must be higher than the voltage drop of the diode (12V should work for most cases). Now, find a resistor of that value using Altium Manufacturer Part Search.
</div>

<div class="callout callout--danger">
 Also, look at the datasheet of the resistor to see if it is rated for the current you calculated. For example, you know the voltage drop across the resistor, the current going through it, calculated $P_{resistor} = V*i$ and make sure that is less than the power rating of the resistor (1/2W, 1/8W, etc).
 </div>

<div class="callout">
NOTE: for the rest of this lab, I assumed an input voltage of 12V and my diode had a rated current of 30mA and a drop of 1.7v. I chose a 1/2W 340 Ohm, 50V resistor from Vishay.
</div>

<div class="callout callout--danger">
 Note some datasheets for LEDs will list a REVERSE VOLTAGE before listing a FORWARD VOLTAGE. The REVERSE VOLTAGE is the voltage if we run current through the LED the wrong way. We want the FORWARD VOLTAGE drop.
</div>

<div class="callout callout--danger">
Note this resistor has a voltage rating. Something to consider when picking parts as well. As a rule-of-thumb, I generally select passive components to have a voltage rating of around 2-3x what I'm putting through them. So if I want a capacitor to hold 12V, I'll get like a 30V capacitor.
</div>


# Schematic Capture Basics (Net Labels, Pin Types)

A good tutorial on [Schematic Capture from Altium](https://www.altium.com/documentation/altium-designer/tutorial-capturing-schematic).

You should have all your parts selected and in the schematic now. Mine are placed like the image below. Note that if you select a component, you can use the space-bar to rotate it on a schematic (not in layout).

![Figure 5](lab1_fig5.png)

The little numbers are called "pins." And these are the things we connect together to make a circuit. There are many types of pins in Altium. I/O pins, power pins, etc etc. You can learn more about defining pin types [here](https://www.altium.com/documentation/altium-designer/sch-dlg-schcomponentpinspropertiesformcomponent-pin-editor-ad?version=18.1).

## Component Designators

The first step in a schematic, after placing your parts, is to assign designators to your components. The designator is the letter-number combination above the part on the schematic. They appear on the schematic, on the PCB itself, and in the bill-of-materials. This makes it really easy to identify components in a PCB. Each component should have a unique designator.

- "C" stands for "capacitor"
- "D" stands for "diode"
- "L" is for "inductor"
- "R" is for "resistor"
- "P" is a connector of some sort
- "U" is a surface mount chip

<div class="callout callout--success">
Select each component and go to the properties tab. Then enter a designator. For example, for the resistor, instead of R?, enter R1. Do this for all your components.
</div>

![Figure 6](lab1_fig6.png)

## Connecting the Parts

Now we have to connect the parts. We could use the "wire" tool and connect everything up like this. But for large schematics this isn't clean and it's not very informative.

![Figure 7](lab1_fig7.png)

So we're going to use net labels because it's CLEANER! First, we know that we'll be providing 12V power to the JST connector. So one side of the connector should go to a 12V power net, and the other side should go to a ground net. Think of a net as a web of connections. Anything in the web is connected to everything else in that web.

<div class="callout callout--danger">
EVERYTHING IN A NET IS CONNECTED SO ALL THE NAMES MUST MATCH. IF YOU WANT TWO POINTS IN A CIRCUIT TO CONNECT THE NAMES MUST MATCH, IF THEY DO NOT MATCH EXACTLY THEY WILL NOT CONNECT.
</div>

![Figure 8](lab1_fig8.png)

To place a power port, see the image above. In this case I want a 12V port, but you could place a 5V port. Or if you want a voltage different than 12V or 5V you can place a "VCC" port. Once you place the port you can edit the name of the port to be whatever you want it to be. However, the name is the net label, it's what defines the connection. Anything else with that same label will connect together IT DOESN'T MATTER IF THE SYMBOL IS THE SAME OR DIFFERENT. Also note the name of a net doesn't MEAN anything to Altium (only to you). For example, if I take a 50V power converter and connect it to a 12V net, Altium won't tell me that's a problem. It doesn't know, it doesn't care, it's just a label. So YOU as a designer need to ensure all the voltages and signals being passed are correct.

![Figure 9](lab1_fig9.png)

My connector now looks like this. Next, I'll do the diode. For the diode. I know one side goes to ground and the other side needs to connect to the resistor. I'll use a NET LABEL at the top of the resistor, and the same GND symbol used for the power connector at the bottom. A NET label is exaclty like the power labels we were placing except its not specific to power, it can be used for signal or anything else.

![Figure 10](lab1_fig10.png)

![Figure 11](lab1_fig11.png)

Now for the resistor I'll do the same thing I did with the diode except I'll connect one side to the 12V and the other side to the D+ net I created.

![Figure 12](lab1_fig12.png)

Now let me show you different variations of the same circuit.

![Figure 16](lab1_fig16.png)

![Figure 13](lab1_fig13.png)

![Figure 14](lab1_fig14.png)

<div class="callout callout--danger">
Note that this last schematic is NOT the same Schematic, GND /= to Ground, 12V /= to +12!
</div>

![Figure 15](lab1_fig15.png)

<div class="callout callout--success">
Connect your circuit together in the schematic using the techniques above.
</div>

# Layout Basics (LSM, Trace Widths, Design Rules)

OK yay, now we need to actually make the PCB itself. Schematic defines the circuit, layout defines the board.

<div class="callout callout--success">
Add a new PCB to the project the same way you added a schematic.
</div>

![Figure 17](lab1_fig17.png)

You'll see a very very blank board. You won't even see components. That's because we haven't linked the PCB to the schematic yet. So let's do that.

<div class="callout callout--success">
Go to your schematic file and select "Design-->Update PCB Document ..." from the top bar.
</div>

![Figure 18](lab1_fig18.png)

<div class="callout callout--success">
A pop-up will appear, hit "Validate Changes" and then "Execute Changes." It should automatically switch you to the PCB document and you should see your components.
</div>

![Figure 19](lab1_fig19.png)
![Figure 20](lab1_fig20.png)

The big black thing is the PCB, the components are self explanatory, the red box is called a "Room." A [Room](https://www.altium.com/documentation/altium-designer/working-with-rooms) is unique to a schematic in layout. All the components that are contained in a schematic should be contained inside the box that defines the room. This helps with component separation, for example separation of power and signal. In more complex boards, multiple schematics will be created for a single PCB, and each schematic will have its own "Room" on the PCB. What you need to know for now is if you drag a component outside the room, Altium will be angry.

<div class="callout callout--success">
Drag the room onto the PCB, select each component, and select the "Properties" tab. Use the "Rotation" parameter to adjust the rotation, and drag to change the location. Layout your parts. Resize the room if necessary.
</div>

This is what I did.

![Figure 21](lab1_fig21.png)

<div class="callout callout--warning">
Note that if you go to the bottom you'll see the layers of the PCB (Top, Bottom, Mechanical 1, ...), if you select "Top Overlay." You'll be able to move the yellow labels around to your liking.
</div>

<div class="callout callout--success">
It's good practice to label your circuit, especially inputs, we should label the 12V and Ground on the connector. Select the "Text" in the top bar and label your 12V and ground. CHECK YOUR SCHEMATIC TO SEE WHICH IS WHICH. Edit the actual text by clicking on it and going to the "Properties" tab.
</div>

![Figure 22](lab1_fig22.png)

You can also define these layers using the Layer Stack Manager. The default PCB is a two layer PCB. If you want to learn more about the LSM, [look at this tutorial](https://www.altium.com/documentation/altium-designer/defining-layer-stack). We'll cover it more later.

## Calculating Trace Widths + Spacing

All limits in electrical systems are either based on voltage or current. Limits due to voltage depend on the dielectric breakdown between any two points in the circuit (usually $V_{IN}$ and $GND$). If you give a chip a voltage any higher than the specific rating, it will explode immediately and you'll have to [shove all the magic smoke that makes chips work, back into the chip](https://www.sparkfun.com/products/retired/10622).

And ratings due to current are generally thermal limitations. ALL electrical items have resistance and therefore experience power dissipation at the rate $P_{thermal} = I^2 R_{component}$. If the device heats too much, it can melt or explode which is why components generally come with a "continuous" current rating where the device can release that heat to the surrounding environment just as fast as it's being generated. And a "burst current" rating where the device can operate at this higher current rating for a certain period of time before the heat build-up is too much.

Trace width is a function of the desired current capacity. The distance between the traces is a function of the required voltage difference between the traces and the dielectric of the material (generally air). [Here's a good tutorial on that.](https://www.smps.us/pcbtracespacing.html)

For this lab, we are working at low voltages and low currents. We won't consider distance between the traces this time (in the future we will teach you). But we'll look at current capacity for argument's sake. Here's a good starting table.

![trace width ](trc_wd.jpg)

There's a [really good tutorial](https://www.mclpcb.com/blog/pcb-trace-width-vs-current-table/) on calculating trace-width for power as well as the impedance of a trace from Millennium Circuits.

For this lab, you can use the table to select your trace width or **go with a standard 10mil trace**. 1 mil = 0.001 inch, note that the "weight" of the trace refers to the density of copper, higher density copper carries more current. The "thickness" of the trace is set by the "layer thickness" in the layer-stack-manager [(see "Defining the Layer Stack" on this page)](https://www.altium.com/documentation/altium-designer/defining-layer-stack).

<div class="callout">
NOTE, WE WILL GO THROUGH ALL THIS IN DETAIL IN THE LAYOUT LECTURE, THIS IS SIMPLY A "PREVIEW" OF THINGS TO COME AND FOR YOUR OWN PERSONAL INTEREST. FOR NOW WE CAN JUST USE THE DEFAULTS WITHOUT CHANGING ANY SETTINGS
</div>

## Routing the PCB

Routing means connecting things with traces. Traces are like wires. We'll just use all the default settings. Note that the components default to sitting on the Top Layer of the board, so we can only connect traces to them on the top layer. The exception to this is the JST connector because it's through-hole, and not surface mount. This means we can connect to it on the top and bottom layer. Internal layers of the PCB (if they existed, they don't in this case) need to be electrically accessed with VIAs, but that will come in a future lecture.

<div class="callout callout--success">
Select the Top Layer. Select the "Interactive Route" tool.
</div>

![Figure 25](lab1_fig25.png)

This tool will allow you to draw traces. Notice the thin grey lines in the layout between the components. These are the nets, they are what you have to connect to each other, and they should match your schematic.

<div class="callout callout--success">
Hover over the pad of the objects you want to connect, click to place one end of the trace, keep clicking to define the points that shape the trace, it's just like lines in a CAD software, and then click on the pad of the other component to finish the trace.
</div>

![Figure 23](lab1_fig23.png)

<div class="callout callout--success">
Keep drawing traces until all the net indicators disappear. This is how you know everything is connected.
</div>

![Figure 24](lab1_fig24.png)

<div class="callout callout--danger">
Note that Altium has a "auto-routing" tool. Never use it, it's garbage, you'll learn why next week.
</div>

# Defining Board Shape (Mechanical Layers)

Finally, you may have noticed that the board is comically large for the components on it. So let's define a shape.

<div class="callout callout--success">
Go to the "Mechanical 1" layer. Click on the "line tool" and draw an outline of your choice.
</div>

![Figure 26](lab1_fig26.png)

![Figure 27](lab1_fig27.png)

<div class="callout callout--success">
Next, use your cursor and select the individual segments of the line you drew on the mechanical layer while holding down the SHIFT key. Then go to "Design"-->"Board Shape"-->"Define Shape from Selected Objects."
</div>

![Figure 28](lab1_fig28.png)

The board should now look like this.

![Figure 29](lab1_fig29.png)

<div class="callout callout--success">
If you go to "View"-->"3D Layout Mode" you can view the board in 3D!
</div>

![Figure 30](lab1_fig30.png)

<div class="callout">
CONGRATS YOU JUST MADE YOUR FIRST PCB!!!!
</div>

# References

[Trace width table](https://electronics.stackexchange.com/questions/615680/fr4-1-oz-copper-pcb-trace-width-for-48v-8a-pulsed-8-ms-pulse-length-up-to)

[Voltage calculations](https://www.smps.us/pcbtracespacing.html)

[Current calculations](https://www.mclpcb.com/blog/pcb-trace-width-vs-current-table/)

[Schematic capture in Altium](https://www.altium.com/documentation/altium-designer/tutorial-capturing-schematic)

[Defining board shape in Altium](https://www.altium.com/documentation/altium-designer/defining-board-shape?version=19.0)

