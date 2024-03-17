---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Lab 01 — Introduction to PCB Software (KiCAD Version)
description: In this lab, help you get started with getting your block diagram and example schematics into your preferred PCB software! We want to make sure you're able to easily get help with using the software before we leave you to design the full schematic for assignment 02. You should feel free to ask any of our staff your help! As a general rule, if it takes you more than 5 minutes for a simple task you think should take much less time, ask us and we can help get things moving.

# Micro navigation
micro_nav: true

# Page navigation
page_nav:
    prev:
        content: Lab 1
        url: '../lab_01'
    next:
        content: Lab 01 (Altium Specific)
        url: '../lab_01_Altium'

author:
    title: Assignment 02
    title_url: '../../assignments/assignment_02'
    description: Lab 1 provides a guide on how to schematic, assignment 02 is meant to be a checklist on best practices and completion before turning in the schematic.
---

<div class="callout callout--warning">
NOTE: this lab is a CRASH COURSE and is really just to get you familiar with what is POSSIBLE in KiCAD. I'll link to external tutorials when I can but don't worry about understanding every detail of everything! Just think of it as dipping your toe into the North KiCAD Sea. It's overwhelming at first, but with time and practice you'll learn to sail too.
</div>

![Moana!](moana.jpg)

# Installing KiCAD

The first step is to [Install KiCAD](../../lectures/altium_setup/#kicad) if you have not done so already. No licensing is required and it *should* work on all platforms.

# Installing Templates and Plugins

We suggest that you use [Winnie's templates](https://github.com/wszeto9/KiCAD_Lib) for this class. They change a few things that will make our lives easier, such as:

- Defaulting to 8.5" x 11" sheets rather than A4 (You may print out your schematics at some point, and this is handy)
- For some reason there's no place for your name besides the comments by default
- Adding rules for fab houses, trace widths for controlled impedance nets

You don't need to use this provided template, but you may want to manually set these things up yourself otherwise. Obviously, change the name on the template sheet if you use it. You can go to KiCAD and go to ***Drawing Sheet Editor***. Open the template file (***WS_Worksheet_Letter***) and edit the name. Edit the sheet as you wish and then save.

Installation is fairly simple-- Download the zip file and unzip it. Then in KiCAD, go to preferences -> edit paths and tell KiCAD where your downloaded folder is.

There'a also a few plugins that you might find handy. You can open KiCAD and open the "Plugin and Content Manager" (the bottom icon with the package image) and scroll through the various items. We enjoy using:

- Interactive HTML BOM - lets us make things like [this](https://htmlpreview.github.io/?https://github.com/PCB-Design-MIT/Wi-Fi-Controlled-RGB-Light/blob/main/bom/ibom.html)

- KiBuzzard - it's how we made the labels in [this board](https://htmlpreview.github.io/?https://github.com/PCB-Design-MIT/Wi-Fi-Controlled-RGB-Light/blob/main/bom/ibom.html)

- PCBWay Plug-in for KiCAD or Fabrication Toolkit - it makes manufacturing file data a simple click away.


# Introduction

This lab is a super simple, super short lab! It's basically just to get you familiar with the following in KiCAD.
- project structure
- schematic capture
- layout tools
- component libraries

We will be designing a very simple PCB from start to finish, not accounting for ANY parasitics, or following really any "good design principals." Remember, this lab is JUST FOR YOUR FAMILIARITY with the software. This is by no means a "well designed PCB."

The order you will do things in this lab is...
- select your components
- create a schematic
- create a layout of these components
- define a board shape

<div class="callout">
Typically, when defining "board shape" we do this in the "Edge Cuts" layer so that when exporting the PCB to be manufactured the manufacturer's software can identify the layer easily, otherwise they'll probably email you with questions. **This is not necessary for this lab since we aren't manufacturing these, but is the accepted practice so you should do it this way.**
</div>

# Overall Design

In this lab we'll be designing a very simple board with an LED, a resistor, and a [JST-XA header](https://www.digikey.com/en/products/detail/jst-sales-america-inc./B2B-XH-A(LF)(SN)/1651045) (here's the [datasheet](https://drive.google.com/file/d/1zLJboW5hgA_aoW00pe7Z8NA9uS-hs0MI/view?usp=drive_link) in case the link is broken). Pretend you need a tiny PCB with an LED on it, maybe you're making a flashlight. But none of the power electronics are actually on the board. You'll use the JST header to send a voltage to the board (connect a power supply / battery), the resistor will limit the current, and the LED will shine.

## How does an LED work?

Brief aside for those who don't know, anyone who does know can skip this section. LEDs shine with a brightness proportional to the current flowing through them. Most LED circuits have what is called a "current limiting resistor" which limits the current from the power source to an appropriate level for the LED. If the resistor is not there, the LED would burn out immediately when hooked up to a voltage source.

LEDs also have a voltage drop across them when they are turned on. This voltage drop is NOT proportional to the current through the LED nor the applied voltage, however is specific to the LED and is based on the internal characteristics of the silicone. We can draw the following circuit diagram for the LED, resistor, and power connector.

![circuit diagram for lab](LED.png)

If we look @ the [datasheet for a typical LED](https://drive.google.com/file/d/1RfvqDrj0AQ-POEH9POQfMgnB-iZPkdQp/view?usp=drive_link) we can see the values of the two parameters we care about, the forward voltage, and forward current per chip. In this case, the forward current needed for the LED to work is 20mA. Let's call this $ I_{fw} $. When on, the LED will have a voltage drop between 1.7-2.6V, let's call this $V_{drop}$.

Now let's assume we'll power the board with four AA batteries (if we're making a flashlight), that's a voltage of 6V if they're in series. Let's call this voltage $V_{in}$. The current can be calculated as follows. The value of the resistor is $R_{limit}$.

$I_{fw} = \frac{V_{in}-V_{drop}}{R_{limit}}$ (Ohm's Law)

That means the voltage across the resistor at MAXIMUM will be 6V-1.7V which is 4.3V, so if we want 20mA of forward current, $R_{limit}$ must be 215 Ohms. Note that 20mA is extremely bright! For a flashlight this might be fine, but for an indicator LED for troubleshooting or such you might want 0.5 mA - 1 mA of current so you can still see the board.

<div class="callout">
Note that we took the lower bound of the typical LED forward voltage range, this is to ensure we don't accidentally under-size the resistor and send too much current through the LED.
</div>

# Component Selection


Create a new project by going to "File"-->"New Project from Template" if you're using a template, or "File"-->"New Project" if you're not. Name it "Lab 01_{your kerberos}". For now you can save it wherever, but if you're doing your project design for assignment 02, be sure to save it in a folder where Github will recognize it and back up your files. See more about Github installation [here](../../../lectures/altium_setup/#setting-up-github-repositories-needed-for-assignment-02-not-for-lab-01) if you haven't already, and be sure you have it working soon.

![Figure 1](lab1KiCAD_fig1.png)

When designing PCBs, we need to remember that we work with REAL components. We need to be able to find these components, and purchase these components. Therefore PCB design is entirely design for manufacturing (DFM) where component selection based on what is available is critical.

**A Schematic defines the components of a PCB and how they are connected.** It also tells KiCAD what components the design uses. KiCAD has many in-built parts libraries, some of which automatically pull the pin-out and the footprint of each part into your design. **A Pinout is the definition of what to connect to what pin for a part, the footprint is the physical shape of the pads on the PCB.** This mapping is absolutely crucial for components-- if an incorrect pinout is defined, the footprint will be wrong and you'll have to end up modifying the PCB. To ensure the pinout is correct, take a moment to look at the symbol and footprint. The pin that is described by the schematic should map to the correct pin on the footprint (as described on the datasheet). This is done with the pin numbers that are on the schematic and footprint. This is a very important concept, and we'd like you to take the time to understand what this means and how to check for a correct footprint.

![Pin Mapping Diagram](pinMapping.png)

## KiCAD In-Built Component Library

KiCAD has a built-in library for very basic components and footprints (such as basic resistors and capacitors). It can be accessed from the "Add a symbol" icon (or hotkey A) on the right side of the screen.

<div class="callout callout--warning">
NOTE: We generally need to specify the exact part number when using this library. For us soldering one resistor on a board it's not a huge deal, but if we're sending a list of 100 capacitors to a manufacturer to assemble, they will want to know which exact capacitor to use.
</div>

In this case we know our design has a resistor. Let's assume we're ok with using a through-hole resistor FOR NOW instead of an SMD resistor (we'll change it later).

<div class="callout callout--success">
Open the symbol browser, and search for "R_SMALL". "R", "R_US", "R_SMALL_US" are all valid as well, and this is just personal preference. We recommend keeping this consistent for your entire schematic-- it might be jarring to see a US Freedom resistor and a resistor in the same schematic. Look at the resistor in the image to the right and make sure it looks like what you expect. It's a resistor for this example, but you can imagine it's more important if you're looking at a 100-pin IC and it might be missing something. Once it looks right, select it and place it down.
</div>

![Figure 2](lab1KiCAD_fig2.png)

Now we have to select the LED and the JST connector. Let's put down a generic LED.

<div class="callout callout--danger">
This is one downside to using KiCAD. You'll likely not find the exact part number for the exact LED you want to use. For this reason, you'll need to modify a generic part to fit your needs.
</div>

![Figure 3](lab1KiCAD_fig3.png)


Once you put down the LED, we're going to need to edit it so we remember what part it is. We suggest you do this right after putting down the part so you don't forget and need to re-trace your search history. Select the part and double click it, or right click on it and click ***edit***, or click on the part and press ***E***. You'll notice a lot of the fields are empty, so we'll have to fill it in. Under ***datasheet*** let's link the [datasheet](https://datasheet.ciiva.com/pdfs/VipMasterIC/IC/LITE/LITES10869/LITES10869-1.pdf). What's cool about this is if you save and exit the properties menu, you can now select the part and click ***D*** for the datasheet. It can be really handy!

We'll also need to edit the footprint. Let's double click the field to edit it. We're going to need to look through the libraries to find the right footprint. Libraries can be a lot to deal with, and it's good to be familiar with the different library names to pull from. Since we're looking for a footprint for an LED, we're going to search for "LED" on the very left column. Notice how there's no implication for *what size LED* we're looking for. This is because a library has the structure shown below, and we're looking for the group, not the individual size.

- LED_SMD
    - LED_Small_Package
    - LED_Medium_Package
    - LED_Big_Package
    - LED_SurfaceMountInOtherSizes
- LED_THT
    - LED_3mm
    - LED_5mm
    - LED_AnotherSizeButStillThroughHole

![Figure 4](lab1KiCAD_fig4.png)

Once we have the right library, let's search for the exact footprint. In the datasheet you might notice the LED is 1.6mm x 0.8mm. You'll learn in lecture 3 that this means the package is called 1608Metric (noting the size of the LED). Searching for that gives two results. Both are valid, the HandSolder version has bigger pads that make it easier to hand solder.

For a quick sanity check, click on the measure tool (ruler on the left side) and measure the size of the red copper pads. It should be slightly bigger than 1.6mm x 0.8mm.

![LED Footprint Measurement](LEDMeasureFootprint.png)

I have a different color theme so your colors may look slightly different. I'm looking at the handSolder version. Regardless, it looks like it should fit our LED fine, so let's select it! We can now exit out of the properties window.

Let's look at another way to edit property fields. Head to the top and click on the spreadsheet icon, labeled "Bulk-edit fields of all symbols in schematic". There, you'll find a sheet of all symbols with all their fields.
![Bulk Edit Window](BulkEdit.png)


We're also going to need to add a field for the ***Manufacturer Part Number (MPN)*** and the ***Digikey Part Number (PN)***. The Digikey PN is just for this class to make it easier to order parts, but MPN is pretty standard to add to make sure things are ordered correctly. Let's do this by selecting ***"Add field"*** in the bottom left, and name it "MPN". Do this again, naming the second one "Digikey PN". We're now going to need to add this information to the LED, so you can click on each cell and enter it. Note that if you copy it with an enter symbol, it may move to the next cell. The Digikey site's copy button by the MPN and Digikey PN avoids this issue, so we recommend copying there and pasting into KiCAD. You can find the part [here on Digikey](https://www.digikey.com/en/products/detail/liteon/LTST-C190GKT/269230). Be sure to use the cut-tape part number whenever possible, and ***avoid*** the ***Digi-Reel*** and ***Tape&Reel*** numbers at all costs for this class.

![Copying Digikey PN](DigikeyPN.png)

Here's what yours should look like (We hid the footprint and datasheet entries for viewing simplicity). We also edited the value of the LED to be LED GREEN for ease of debugging if we were to debug this board later on. Once your done, hit "Apply, Save Schematic & Continue".

![The finished Bulk Edit table](FinishedBulkEdit.png)


<div class="callout callout--danger">
Now do the same for the JST B02B-XASK-1(LF)(SN) connector. We'll use this connector to provide power to the board. You can use a Conn_01x02_Pin for the symbol. Don't worry about the footprint for now, and get the other fields filled out.
</div>

## Importing libraries

You may have noticed that there is no footprint for the B02B-XASK-1(LF)(SN). You can try looking under ***Connector_JST*** (JST being Japan Solderless Terminals, the company that makes this part) but you won't find it. Rather, we're going to need to import a footprint. There's various sites that can help you find a footprint, including:
- Digikey (under "EDA/CAD Models")
- snapEDA
- Octopart (under "CAD Models")
- Ultra Librarian

Octopart is really good for this, as it searches multiple sites for the footprints. They're also a [subsidiary of Altium](https://octopart.com/pulse/p/octopart-is-joining-altium-2) and it's what helps make Altium's part libraries (specifically its Manufacturer Part Search tool) so powerful. We love using better tools from $the$ $other$ $side^{TM}$. By the end of class you'll need to sign up for a fair number of footprint sites, so might as well start now and use some burner emails and passwords for those accounts. Oh wait, you don't hear that from me...

Go ahead and download the right footprint. Unzip the files, and ignore the install instructions as if they're Ikea instructions. You're looking for a file called ***[Part Name].kicad_mod***. If you're importing a symbol too, such as for an IC you can't find, you'd be looking for a file called ***[Part Name].kicad_sym. Once you locate the files, be sure to remember where it is.

Go back to the schematic editor in KiCAD and select the Footprint editor on the top.

![Footprint Editor](FootprintEditor.png)

Go to File -> New library. The scope of the library is up to you-- if this is a part you'll use often, stick it in a Global library. If it's a one-off for a project, select the project scope. We'll select the project scope, and save the library in the same folder as the KiCAD schematic file. You're saving a .pretty file, which is just a fancy folder that KiCAD creates for each library. Now, we're going to import the footprint we downloaded. Go to file -> import -> footprint. Select the ***.kicad_mod*** file you downloaded.

![Import footprint](ImportFootprint.png)

Go ahead and hit file -> save and select your newly created library. We're going to name it something readable, like the part number. Be sure to select your project specific library. Note that this library is for all parts in our project, so we only need one library for all the footprints we'll import.

![Save footprint](SaveFootprint.png)

Let's now go to File -> Footprint properties and select the 3D Models tab. You'll notice that there's no 3D model right now. Let's change that.

![No 3D Model](No3DModel.png)

Go to the downloaded folder and see if there's a .stp, .step, or .wrl file. If not, you might need another source that has the part's 3D model. I'm going to use the 3D model from [here](https://www.snapeda.com/parts/B02B-XASK-1%28LF%29%28SN%29/JST%20Sales%20America%20Inc./view-part/) because I think it looks nicer, but that's personal preference. Once you located the part, copy it and paste it inside the .pretty library folder you created using file explorer.

![Pasting the 3D model in the .pretty folder](Paste3DModel.png)

Back in Kicad's footprint properties, you should be able to type in ***./[File Name.FileExtension]*** and have the part show up (KiCAD 7 only). You can also use the file explorer tool, but this looks more clean.

![Imported 3D Model](Imported3DModel.png)

We now see the model, and you can use the tools to change how it's oriented. And we're done! Remember to save. Let's go back in the schematic and add in our newly created footprint.

## Selecting the Resistor

<div class="callout callout--success">
Now, use the technique presented above and the datasheet of the LED you selected to calculate a value of the resistor. Note that you can choose any input voltage you desire but it must be higher than the voltage drop of the diode (12V should work for most cases). Now, edit the resistor you put down to reflect that value. Naming convention for resistors should be 220R for resistors from 1 to 999 $\Omega$, or 2.7K for 1k$\Omega$ to 999k$\Omega$.
</div>

<div class="callout callout--danger">
 Also, look at the power dissipation for that resistor you specified. For example, you know the voltage drop across the resistor, the current going through it, calculated $P_{resistor} = V*i$ and make sure that is less than the power rating of the resistor (1/2W, 1/8W, etc). Check out [our Component Selection Guide](../lectures/ComponentSelection/#resistors) to see what package size you need. We're going to suggest you use 0603-1206 (Imperial) sized resistors for most things. Once you select a package size, be sure to add the appropriate footprint to the resistor. Also, name your part [resistance] (space) [Package]. For instance, 12K 0603, 1K 1206.
 </div>

<div class="callout">
NOTE: for the rest of this lab, I assumed an input voltage of 12V and my diode had a rated current of 30mA and a drop of 1.7v. I chose a 1/2W 340 Ohm, 50V resistor from Vishay.
</div>

<div class="callout callout--danger">
 Note some datasheets for LEDs will list a REVERSE VOLTAGE before listing a FORWARD VOLTAGE. The REVERSE VOLTAGE is the voltage if we run current through the LED the wrong way. We want the FORWARD VOLTAGE drop.
</div>

<div class="callout callout--danger">
Note this resistor has a voltage rating. Something to consider when picking parts as well. As a rule-of-thumb, go with a voltage rating of around 30% over what your specification is. So if I want a capacitor to hold 12V, I'll get like a 16V capacitor. This is called de-rating a component, and it's typically done to give us a little margin of safety in case something happens. You'll generally want to de-rate *most specs* including <b>current limits, voltage limits, and power limits.</b>
</div>


# Schematic Capture Basics (Net Labels, Pin Types)

You should have all your parts selected and in the schematic now. Mine are placed like the image below. Note that if you select a component, you can use the ***r*** key to rotate it.

![Figure 5](lab1KiCAD_fig5.png)

The little numbers on the leader are called "pins." And these are the things we connect together to make a circuit. There are many types of pins in KiCAD. I/O pins, power pins, etc etc. You can learn more about defining pin types [here](https://klc.kicad.org/symbol/s4/s4.4/).

## Component Designators

The first step in a schematic, after placing your parts, is to assign designators to your components. (This is a Altium thing, KiCAD should have this automatically work) The designator is the letter-number combination above the part on the schematic. They appear on the schematic, on the PCB itself, and in the bill-of-materials. This makes it really easy to identify components in a PCB. Each component should have a unique designator.

- "C" stands for "capacitor"
- "D" stands for "diode"
- "L" is for "inductor"
- "R" is for "resistor"
- "J" is a connector of some sort
- "U" is a surface mount chip

<div class="callout callout--success">
KiCAD automatically numbers your parts. Notice how if you put down a 2nd resistor, it's labeled as R2. If you see something like ***C?*** or ***J?*** then click on the auto-assign designator button that's on the top. You can also type it manually, but it's not recommended as it's possible to accidentally make 2 identical designators.
</div>

![Figure 6](lab1KiCAD_fig6.png)

## Connecting the Parts

Now we have to connect the parts. We could use the "wire" tool (or click on a pin that's not connected to start a wire) and connect everything up like this. But for large schematics this isn't clean and it's not very informative.

![Figure 7](lab1KiCAD_fig7.png)


So we're going to use net labels because it's CLEANER! First, we know that we'll be providing 12V power to the JST connector. So one side of the connector should go to a 12V power net, and the other side should go to a ground net. Think of a net as a web of connections. Anything in the web is connected to everything else in that web.

<div class="callout callout--danger">
EVERYTHING IN A NET IS CONNECTED SO ALL THE NAMES MUST MATCH. IF YOU WANT TWO POINTS IN A CIRCUIT TO CONNECT THE NAMES MUST MATCH, IF THEY DO NOT MATCH EXACTLY THEY WILL NOT CONNECT.
</div>

To place a power port, search for it in the symbol library or the power symbol library (the icon below the symbol library). In this case I want a 12V port, but you could place a 5V port. Or if you want a voltage different than 12V or 5V you can place a "VCC" port. Once you place the port you can edit the name of the port to be whatever you want it to be [Altium only for now, KiCAD will have this feature in KiCAD 8 in March 2024 or so]. However, the name is the net label, it's what defines the connection. Anything else with that same label will connect together IT DOESN'T MATTER IF THE SYMBOL IS THE SAME OR DIFFERENT. Also note the name of a net doesn't MEAN anything to KiCAD (only to you). For example, if I take a 50V power converter and connect it to a 12V net, KiCAD won't tell me that's a problem. It doesn't know, it doesn't care, it's just a label. So YOU as a designer need to ensure all the voltages and signals being passed are correct.

![Figure 9](lab1KiCAD_fig9.png)

My connector now looks like this. Next, I'll do the diode. For the diode. I know one side goes to ground and the other side needs to connect to the resistor. I'll use a NET LABEL at the top of the resistor, and the same GND symbol used for the power connector at the bottom. A NET label is exaclty like the power labels we were placing except its not specific to power, it can be used for signal or anything else.

![Figure 10](lab1KiCAD_fig10.png)

![Figure 11](lab1KiCAD_fig11.png)

Note how there's a line drawn over the entire net label. This is a bit of an extra thing to make sure the net label is connected. This may seem obvious, but here's two different pictures. The first one is connected, the second one isn't. This is only noticeable by the tiny dot, so a line is much more obvious.

![Not Connected Net Label](NotConnected.png)
![Not Connected Net Label](Connected.png)

Now for the resistor I'll do the same thing I did with the diode except I'll connect one side to the 12V and the other side to the LED+ net I created.

![Figure 12](lab1KiCAD_fig12.png)

Now let me show you different variations of the same circuit.

![Figure 16](lab1KiCAD_fig16.png)

![Figure 13](lab1KiCAD_fig13.png)

![Figure 14](lab1KiCAD_fig14.png)

<div class="callout callout--danger">
Note that this last schematic is NOT the same Schematic, GND /= to Ground, 12V /= to +12!
</div>

![Figure 15](lab1KiCAD_fig15.png)

<div class="callout callout--success">
Connect your circuit together in the schematic using the techniques above.
</div>

# Multi-Sheets and Hierarchies for Bigger Designs!

In this example, we've connected an LED, a resistor, and a battery. However, you'll be working on a much larger project for this course that has more components, connections, signals, and other fun stuff. Placing all of these elements in the same sheet can get really messy! It can also make it harder to debug your PCB later. To avoid this, we can use multi-sheet designs to separate components into modules and show how modules connect to one another.

If your design is looking large, but you can separate it into independent modules that do not connect to one another, you can make a *flat design*. This is a project that has multiple independent sheets that are not connected in any way.

On the other hand, if you can split your design into modules that are interdependent, a *hierarchial design* can work for your project. This involves multiple sheets with shared signals or connections between modules in each sheet, like a tree with branches off of it. The schematic for your project for the course will likely be a hierarchial design.

Regardless of whether you choose a flat vs. hierarchial design, you can organize the sheets in your design using a *top sheet*. This shows what sheets are in your design and how they connect.

![Figure 17](lab1KiCAD_fig17.png)

Above is an example of a top sheet, taken from [SEVT's battery management system](https://drive.google.com/file/d/1KLIEmnSQEz9ZXyKeI9kjRNrVwL-6MiZv/view?usp=drive_link) and included in last year's class as a case study. Signals that are shared between sheets are represented as ***Hierarchical labels*** (Altium calls them ports). These are different from net labels. Net labels have a sheet-only scope. For instance in the U_IO sheet I can make a net called ***GPIO1*** and the net name will actually be ***U_IO/GPIO1***. This means if I make a net label in U_Power called ***GPIO1***, it will be called ***U_Power/GPIO1***. This means they're not connected, despite both being net labels in seperate sheets.
<!-- how to make a top sheet / make connections -->
For more information on how to implement a multi-sheet design, check out  [this page](https://docs.kicad.org/7.0/en/eeschema/eeschema.html#hierarchical-schematics) for KiCAD.

# Layout Basics (LSM, Trace Widths, Design Rules)

OK yay, now we need to actually make the PCB itself. Schematic defines the circuit, layout defines the board.

<div class="callout callout--success">
Let's open the PCB Editor. It's the green PCB icon in the top.
</div>

![Figure 18](lab1KiCAD_fig18.png)

You'll see a very very blank board. You won't even see components. That's because we need to tell KiCAD to import the components from the schematic to the PCB.



<div class="callout callout--success">
A pop-up will appear, hit "Validate Changes" and then "Execute Changes." It should automatically switch you to the PCB document and you should see your components. Make sure you have no errors. If errors pop up, make sure all the footprints are assigned and that they have all pins specified.
</div>

![Figure 20](lab1KiCAD_fig20.png)

Once you exit this menu, you can click and put down your components.



This is hard to demonstrate with 3 components, but KiCAD has some algorithm for placing down components separated by sheets. Here's what our LED Demo PCB looks like when first imported. You can sort of make out each section just by what's duplicated. You can also see that if a sheet or a set of components are selected in the schematic, they all get highlighted in the PCB view. This makes it really handy for large PCB project layout. You can also then right click this highlighted group and select "Pack and Move Components (P)" and KiCAD will re-group that highlighted set based on what thinks is a decent layout, and then you can move it anywhere!

![Figure 21](lab1KiCAD_fig21.png)


Back to the tutorial. Since we only have 3 components, we can drag and rotate them individually until it's to your liking. You can also use a larger grid for a neater layout. We like to use 1mm or so.

![Figure 22](lab1KiCAD_fig22.png)

<div class="callout callout--warning">
Note that if you go to the right you'll see the layers of the PCB (F.Cu, B.Cu, F. Silkscreen...), if you select "F. Silkscreen" You'll be able to move the white labels D1/R1/J1 around to your liking.
</div>

<div class="callout callout--success">
It's good practice to label your circuit, especially inputs, we should label the 12V and Ground on the connector. Select the "Text" in the right bar (looks like a letter T) and label your 12V and ground. Make sure the layer is F.Silkscreen. CHECK YOUR SCHEMATIC TO SEE WHICH IS WHICH. Edit the actual text by double clicking on it. Knocked out text can be nice for bolder labels, and there's a box to check for that in the edit text box.
</div>

![Figure 23](lab1KiCAD_fig23.png)

You can also define these layers using File -> Board Setup -> Physical Stackup. The default PCB is a two layer PCB. KiCAD can do basic layer stackups but can't do certain things like Rigid-Flex and a bunch of fancy math stuff. You can learn about what Altium's Layer Stack Manager does [here](https://www.altium.com/documentation/altium-designer/defining-layer-stack) but for our class and ***most things that aren't a computer or RF, Kicad's stackup works fine***. We'll also talk about layer stackups later this week.

## Calculating Trace Widths + Spacing

All limits in electrical systems are either based on voltage or current. Limits due to voltage depend on the dielectric breakdown between any two points in the circuit (usually $V_{IN}$ and $GND$). If you give a chip a voltage any higher than the specific rating, it will explode immediately and you'll have to [shove all the magic smoke that makes chips work, back into the chip](https://www.sparkfun.com/products/retired/10622).

And ratings due to current are generally thermal limitations. ALL electrical items have resistance and therefore experience power dissipation at the rate $P_{thermal} = I^2 R_{component}$. If the device heats too much, it can melt or explode which is why components generally come with a "continuous" current rating where the device can release that heat to the surrounding environment just as fast as it's being generated. And a "burst current" rating where the device can operate at this higher current rating for a certain period of time before the heat build-up is too much.

Trace width is a function of the desired current capacity. The distance between the traces is a function of the required voltage difference between the traces and the dielectric of the material (generally air), or whatever the board fabricator can make. [Here's a good tutorial on that.](https://www.smps.us/pcbtracespacing.html) You'll also want to space things out if you're routing sensitive signals and can't have signal noise from one trace to another. We'll talk about this more next week, and you won't have to worry about it much.

For this lab, we are working at low voltages and low currents. We won't consider distance between the traces this time (in the future we will teach you). But we'll look at current capacity for argument's sake. Here's a good starting table.

![trace width ](trc_wd.jpg)

There's a [really good tutorial](https://www.mclpcb.com/blog/pcb-trace-width-vs-current-table/) on calculating trace-width for power as well as the impedance of a trace from Millennium Circuits.

Note that the "weight" of the trace refers to the density of copper, higher density copper carries more current. The "thickness" of the trace is set by the "layer thickness" in the physical stackup. You'll also want to make sure the trace isn't too thin to manufacture. Generally most PCB fabricators are good with 6 mil traces and larger including PCBWay (which we're using), but check with your fab. For this lab, you can use the table to select your trace width or **go with a standard 10mil trace**. 1 mil = 0.001 inch.

<div class="callout">
NOTE, WE WILL GO THROUGH ALL THIS IN DETAIL IN THE LAYOUT LECTURE, THIS IS SIMPLY A "PREVIEW" OF THINGS TO COME AND FOR YOUR OWN PERSONAL INTEREST. FOR NOW WE CAN JUST USE THE DEFAULTS WITHOUT CHANGING ANY SETTINGS
</div>

## Routing the PCB

Routing means connecting things with traces. Traces are like wires. We'll just use all the default settings. Note that the components default to sitting on the Top Layer of the board, so we can only connect traces to them on the top layer. The exception to this is the JST connector because it's through-hole, and not surface mount. This means we can connect to it on the top and bottom layer. Internal layers of the PCB (if they existed, they don't in this case) need to be electrically accessed with VIAs, but that will come in a future lecture.

<div class="callout callout--success">
Select the Top Copper Layer (F.Cu). Select the "Route Tracks" tool and click on a copper pad to start.
</div>

![Figure 26](lab1KiCAD_fig26.png)

This tool will allow you to draw traces. You can click to put down a trace or to create a curve-- play around with it! Notice the thin grey lines in the layout between the components. These are the nets, they are what you have to connect to each other, and they should match your schematic. The grey border around the trace is a "keepout zone" for the track and it tells you where you can't have a trace that's a different connection than the one you're routing. This sometimes shows up depending on your settings and if you set stuff right. Let us know if this doesn't show up for you.

<div class="callout callout--success">
Hover over the pad of the objects you want to connect, click to place one end of the trace, keep clicking to define the points that shape the trace, it's just like lines in a CAD software, and then click on the pad of the other component to finish the trace.Keep drawing traces until all the net indicators disappear. This is how you know everything is connected. There's also a counter at the bottom to tell you how many unrouted nets are left.
</div>

![Figure 25](lab1KiCAD_fig25.png)

<div class="callout callout--danger">
Note that KiCAD does not have a "auto-routing" tool. Never use it on any software, it's garbage, and your stuff may not work. It's like asking an AI to design your circuit-- it might be able to, but it may ignore a lot of design constraints we work with on real systems.
</div>

# Defining Board Shape (Mechanical Layers)

Finally, you may have noticed that the board is comically large for the components on it. So let's define a shape.

<div class="callout callout--success">
Go to the "Edge Cuts" layer. Use the line/circle/arc/rectangle/polygon tool to draw whatever shape your heart desires.
</div>

![Figure 27](lab1KiCAD_fig27.png)

<div class="callout callout--success">
FYI, Altium is weird and needs extra steps. We're using KiCAD so we don't care about this, but you might have to use Altium one day.

For Altium: use your cursor and select the individual segments of the line you drew on the mechanical layer while holding down the SHIFT key. Then go to "Design"-->"Board Shape"-->"Define Shape from Selected Objects."
</div>

<div class="callout callout--success">
If you go to "View"-->"3D Viewer" or ***Alt + 3*** you can view the board in 3D!
</div>

![Figure 31](lab1KiCAD_fig31.png)

<div class="callout">
CONGRATS YOU JUST MADE YOUR FIRST PCB!!!!
</div>

<div class="callout callout--success">
***TODO:*** Using the skills you learned in today's lab, start working on your project schematic! You will likely need to use a heirarchial multi-sheet structure to organize your design. We suggest you use your block diagram as a start for which components should be split into which sheet. If you have any questions or need guidance, feel free to reach out to course staff in-lab or on Slack!
</div>

# References

[Trace width table](https://electronics.stackexchange.com/questions/615680/fr4-1-oz-copper-pcb-trace-width-for-48v-8a-pulsed-8-ms-pulse-length-up-to)

[Voltage calculations](https://www.smps.us/pcbtracespacing.html)

[Current calculations](https://www.mclpcb.com/blog/pcb-trace-width-vs-current-table/)
