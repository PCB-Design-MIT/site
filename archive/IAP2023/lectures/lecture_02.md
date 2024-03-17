---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Lecture 2 - What's a PCB?
description: There's a lot that goes into a PCB - both in the physical fiberglass, metal, plastic, as well as the design process that produced it. Let's get some context for the PCBs we're trying to build, along with how they're designed and built.

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
        content: Lecture 01
        url: '../lecture_01'
    next:
        content: Lecture 03
        url: '../lecture_03'
---

## What's a PCB?

I think it'd be more useful to start with what a PCB isn't, rather than what it is. When we were putting together this course we sent out a survey asking people what they'd build if they knew how to design PCBs, and we got some answers like the following:


<div class="callout callout--info">
    <p><em> - I don’t know, but I feel very inspired by someone in my floor who made custom PCBs for his light up settlers of catan board. I think I would want to do something for a board game I made or a toy.</em></p>
    <p><em> - Go kart</em></p>
    <p><em> - Firespinning light</em></p>
    <p><em> - Motor controller</em></p>
    <p><em> - boba machine pcb pls</em></p>
    <p><em> - An over-engineered black box with de-bounced front-end, a low-noise amplifier, an overpowered DSP, delicately containerized RF transmission and reception sections using the second-harmonics of oven-controlled crystal oscillators, carefully-tuned distributed-element filters, and double-balanced RF mixers - all used to transmit incredibly simple data using 256-QAM with CRCs and Huffman coding, all powered by a custom switched-mode power supply with active power-factor correction - all leading to a VFD-controlled IGBT-driven motor that pushes back the switch on a useless machine.</em></p>
    <p><em> - Make my BB8 robot follow me (not sure if that’s something u can make a pcb do)</em></p>
</div>

To the person behind the last response, we're here for you :) And to the person before that, you'd make a fabulous [turboencabulator](https://en.wikipedia.org/wiki/Turbo_encabulator) engineer.

A PCB is a _Printed Circuit Board_, and fundamentally it's **just a flat piece of material that connects electronic parts together**. You've likely seen them before, but here's a couple examples of what they can look like:

![](beavercube.jpg)
![](qwiic_temperature.jpg)

Those are called rigid PCBs, and they're made by taking a flat piece of fiberglass substrate, bonding conductive copper to either side, and then selectively etching away the copper to make connections between the components you want. Components mount to either the surface of the board, or they have leads that go through the board and solder to the hole. It's basically a fiberglass sandwich, and looks like this if you slice into it:

![](cross_section.jpg)

*Image Courtesy of [Evil Mad Scientist Laboratories](https://twitter.com/EMSL/status/1385779310233980931/photo/1)*

This is called a rigid PCB, and it's most of what you'll see. There's a few variations on it too:

- _Multi-layer PCBs_ - Exactly the same thing, except taken a little further. After the copper etching is done, a thin layer of fiberglass called _prepreg_ is applied to either side, and then another layer of copper is applied to that. That copper gets etched, forming another set of copper layers on the PCB. This lets you route more signals, since you can now have more space to go through/around other signals on the board. Super high layer count boards are possible - you'll be making 2-layer boards for simplicity, but 12-layer boards are pretty common in super mass produced consumer electronics.

- _Aluminum/Ceramic PCBs_ - Which are just like a normal PCB, except they exchange the fiberglass for aluminum, ceramic, or some other substrate. This is normally done if you need a PCB that's more thermally conductive and can conduct heat away from components faster than fiberglass can. Aluminum can conduct better than fiberglass, ceramic can conduct better than aluminum.


- _Flex PCBs_ - Which use polyimide instead of fiberglass. Polyimide is flexible, which makes flex PCBs really good for actuators (which have moving parts, but the PCB itself is the moving part) and interconnects between boards - since the PCB is bendy, the cables that normally connect PCBs can become the PCB itself. This technology is the backbone of the iPhone, which uses them everywhere, like this:


- _Rigid-Flex PCBs_ - As you can probably guess, this is just a mix of the two technologies. We can laminate fiberglass and polyimide in the right order, and make something that has a rigid section, and something with a flexible section. 

- _Substrate-like PCBs (SLP)_ - Also exist and allow for really complicated routing. This almost makes a chip-to-chip interconnect with the density of a chip. [Zhen Ding](https://www.zdtco.com/en/product) appears to be Apple's supplier.


## What else can you do with them?

### Enclosures
Volja Antonic wrote an [excellent article](https://hackaday.com/2015/06/03/how-to-build-beautiful-enclosures-from-fr4-aka-pcbs/) about this, but you can use FR4 to make boxes for your projects, like this:

![](fr4_enclosure_1.jpg)
![](fr4_enclosure_2.jpg)

The top of these things are PCBs. The bottom of these things are PCBs. The sides of these things are PCBs. They're all PCBs. And they're all beautiful. You also don't need to use fabbed PCBs for this either, people have made enclosures from just bare copper-clad FR4:

![](fr4_enclosure_3.jpg)

*Image Courtesy of [Dave Richards AA7EE](https://aa7ee.wordpress.com/2011/07/24/the-wbr-a-simple-high-performance-regen-receiver-for-40m-by-n1byt/)*

### Actuators
You can put a bunch of piezoelectric elements in a ring and use that to excite rotating standing modes in a circle-shaped PCB - all of which seems very boring until you put a hard object on top of it. The rotating wave couples into the object causing it to rotate, resulting in a motor on a PCB that requires no moving parts:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/uFZsH62ewYo?start=12" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

These are the standard in high-end cameras for rotating lenses since they're small, maintenance-free, nearly silent, and trivially backdrivable - super neato. Magnetic actuators are also somewhat popular, and [Carl Bugeja](https://www.youtube.com/@CarlBugeja/featured) has quite a few mechanisms based on flapping coils made on flexible PCBs. Those are super neat - he's made some displays and motors from those.

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/yhKtwCmlFfg?start=238" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


# How do you design a PCB?
Loosely, it's a six step process. 

## Circuit Design
This is probably the easiest to understand part of the process, but it's where you design the circuit that you want to put on a PCB. This class is on PCB design, not circuit design, so 2.678/2.679/6.2000 will do a better job telling you how this part works than we will. But we will say that every circuit is different - some circuits (like a motor controller or power converter) might follow a set topology, but require a lot of care in choosing the exact parts that go in them. Other circuits might just be copied from the manufacturer's recommendations. And others might be something you find in the back of [some random book Fischer brought to book club in middle school](https://web.mit.edu/6.101/www/reference/op_amps_everyone.pdf). Each circuit follows a different design process - just as the design process of a mechanical part varies between doorknobs and doorhinges, the design process of an amplifier and a microcontroller will be different.

That's a little vague (sorry), but the one thing we can say is that at some point you'll have to choose parts to put on your circuit. When you get here you'll be looking at the tech specs, pricing, and availability, and variants of each part. 

- _Specs:_ Some parts are more capable than others. Some digital chips have more memory than others. Some resistors can dissipate more power than others. Some capacitors can be charged up to a higher voltage than others.

- _Packaging (of the individual part):_ Some parts come in multiple footprints - I could get a [74LS00](http://ecelabs.njit.edu/student_resources/datas/74ls00.pdf) in either a SOIC, SOP, or DIP package. The underlying silicon is the same, just the form factor it's given to you is different. Depending on your design you might be constrained to a particular form factor; for example, if you're making a smartwatch, you probably don't want your microcontroller to come in a package as large as a DIP - but if you're making an alarm clock you might not care. Or, you might not want to pay the factory for extra precise tolerances on your PCB - so you use a larger package to keep your tolerances (and production costs) low.

- _Packaging (of all the parts):_ The same part will sometimes come in a few different packages - depending on what the manufacturer has, your chips could be shipped to you in a plastic tube, a paper tape, or a paper tape wound onto a plastic reel. You might not care how you get them - if you're just putting things together yourself, it might not make much difference if you're getting parts out of a tube or off a tape. But if you're mass-producing things, you might want a tape (or tape and reel) so that a pick-and-place machine can automatically place your parts on the board for you.

- _Pricing:_ Parts are usually procured through distributors (our two favorites being Mouser and DigiKey) which will give you discounts based on quantity. Sometimes the cheapest part at quantity 10 isn't the same as the cheapest part at quantity 10k. The price break is pretty strong after the first few orders of magnitude, but it eventually tapers off and the price converges to (approximately) the actual cost of the part that the distributor pays the manufacturer.

- _Availability:_ Normally this wouldn't deserve a mention, but the chip shortage has made this a more nuanced issue. Back before Covid, most distributors had stock of most chips most of the time. Now that there's a shortage, they can have no chips one morning, get a shipment of a few thousand that afternoon, and then sell out of all of them by the evening as everyone panic-buys them. This has made it really hard to design some types of circuits (particularly things in automotive) because stock is fluctuating so much - you can design a circuit for your PCB around a particular chip, and then have it be out of stock by the time the PCB design is done.


## Schematic Capture

The completed design is represented as a schematic, which is just a way to represent what's connected to what in a circuit. You've probably seen one of these before, here's an example:

When we design a PCB in CAD, we need to have the CAD program be aware of what gets connected to what, so that it routes the traces on our PCB properly. We do this by _capturing_ the schematic in our design tool of choice, in this case Altium. This results in a schematic that looks about like this:

![](schematic_capture.png)

On first glance, it's exactly the same as our pen-and-paper schematic, and that's by design! Most of the design intent is carried in the schematic, and it's important that's not obfuscated by our CAD program. However, the component boxes in Altium carry more information than the ones in our sketch do. These are linked to footprints and 3D models for each part, so that when we include a part on our schematic Altium knows what copper pattern to put on the PCB to connect to it. Altium's also pretty cool in that it'll also track supply chain for your parts automatically and help with sourcing/ordering, but we're getting ahead of ourselves.

The important thing is that once schematic capture is done, you have a digital representation of your design in a CAD tool of your preference, and that this representation carries all the information associated with your components.

## Board Layout

Once you've got this, you'll move onto board layout. This is where your CAD tool will reference the mechanical information for the parts you lovingly laid out during schematic capture, and dump all your parts onto a board for you. This looks about like this:

_image should go here_

At this point it's time to lay out the board, which mostly comes down to two tasks:

- _Component Placement_, and it's exactly what it sounds like. Notice that all the parts have been placed off the board - that's because your design tool doesn't know what the best location for every part is, so it's up to you to place each part on the board. The goal here is to place related components next to each other as closely as possible. For the circuit above, all components should be placed near each other on the board, to minimize the amount of wiring that needs to run across the board. Speaking of the wiring that needs to run across the board...

- _Routing_ follows component placement, and it's where you draw the connections between each component. This takes some time since traces on the same layer can't cross, so there's some cleverness that goes into moving between layers with vias. Some components also have special requirements for the copper around them (like thermal vias, or local ground pours), and those need to be respected at this stage. 

I normally spend about 70% of my time on component placement when designing a board. It's just the easiest way to make things neat and tidy, and good component placement makes routing a piece of cake. If you look at the bluetooth speaker design, all the components in each subcircuit are next to each other, and most traces aren't longer than a quarter inch. Placing components on that board took some time, but routing was relatively quick.

- _DRC_ or Design Rule Check, which checks the board for mistakes such as traces that connect to nothing, or accidental short circuits on the board. The rules that it checks against are configurable, and you might want to change them depending on the project, or the factory that's building your boards. Once the board's been verified, we move on to generating gerbers. 

- _Gerber Generation_ is the last step, and just consists of exporting a set of files (called Gerbers) that describe what's on each layer of the PCB. There's a separate gerber file for each layer on the PCB (plus an extra file for where holes get drilled) and so you generate the whole set. It's convention to send these around as a .zip file, and it's what the fab expects to get.

## Fabrication

### Professional Fab House
Once your design is sent off to the fab, it's mostly out of your hands and you just wait for it to come back, so you could easily ignore this step. But that's no fun, especially when there's some _butteryyyyy_ footage from PCBWay that shows you how the show's done:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/o8NOK1JJbgw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### PCB Mill
It's worth noting that there's options outside of professional fab houses. You can also make boards with a PCB mill, which takes a sheet of copper-clad FR4 and mills out everything that shouldn't have copper on it. We've got one of these in EDS next to 38-500, and they look like this:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/3EHa0PyN1ok?start=154" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

These are super quick relative to waiting for PCBs from overseas, but they come with some serious limitations. You don't get any soldermask or silkscreen, and you're limited to two layer boards, and you can't have spacing between copper that's smaller than the drill bit. This technique also tends to rip up thinner traces, since thinner traces have a really small amount of glue holding them to the fiberglass, which can't take the out of plane forces from the end mill. But if you've got a chunky circuit that you need made quickly, these are great.

### Toner Transfer Method
The main alternative to a PCB mill for homebrew PCBs is the toner-transfer method. It works by printing out a positive mask (ie, everywhere that should have copper) onto a piece of photo paper with a laser printer. You then press this against a piece of copper-clad FR4 with a clothes iron to melt the laser toner onto the copper (laser toner is secretly just plastic), and then dunk the thing in water to melt the paper away. Once it's just the toner on the paper you dunk the board into an etchant to dissolve away the exposed copper. Everything under the toner is protected from the etchant, so the copper under the toner is preserved. Once the etch is done, you remove the toner with some acetone, and your board is done!

The output looks about like this:

![](toner_transfer.jpg)

Which is taken from the [excellent guide](https://hackaday.com/2016/09/12/take-your-pcbs-from-good-to-great-toner-transfer/) put together by Elliot Williams of Hackaday. Some variations exist on this method - a particularly good one uses spray paint and a laser cutter to accomplish the same effect - but the fundamental concept is the same. 

## Assembly
Once you have your circuit boards, you'll need to put your parts on them! This is done by soldering - a method that uses liquid metal to join our parts together, making an electrical connection in the process. You'll learn all about doing this manually in [lab 03](../lab_03), but it fundamentally works the same at the large scale. The quick version is that it's a three step process:

- __Applying Solder Paste:__ A thin layer of solder paste is applied to the board. This stuff is just microscopic solder balls suspended in goo, and it gets applied wherever a solder joint should go - normally using a metal stencil. We'll do this manually in lab, but big machines automatically do this at the production-scale.
- __Placing Parts:__ We'll be doing this manually with a set of tweezers and a sharp eye, but we place the parts on top of the solder paste. Since the solder paste is - well, pastey - the parts stay in place. At production-scale this is done with a _Pick and Place_ (PnP) machine that grabs parts off of the reels they come in, and puts them on the board. It's got a little vacuum gripper doodad to do this.
- __Reflowing Boards:__ The whole board is heated up (usually in an oven), which burns off the goo and melts the solder balls, which flow onto the metal parts, and join them together. The surface tension from the molten metal bring everything into perfect alignment, resulting in some of the most satisfying .gifs on the internet:

![](reflow.gif)

*Image Courtesy of [Botfactory](https://www.botfactory.co/blog/what-s-new-at-botfactory-1/post/solder-with-botfactory-153)*


There's some other steps that go into it (cleaning residue off the boards, depanelizing them if needed) but that's the core of it. If you've got a labkit, you'll get to walk through this process yourself in [lab 03](../lab_03)!

## Testing
Once your board's together, you'll need to make sure it works. This works a little differently depending on if you're bringing something up for the first time or the n ^th^ time.

### First time
When you're putting something together for the first time it's pretty likely that it's not going to work, and as a result bringing up a board turns into a cycle of finding bugs and fixing them. And as much as we'd like to have a definite path to go through it, the way this cycle of testing and debugging plays out is super dependent on the project. There's some nuance to this though and we'll talk about that in a [future lecture](https://www.youtube.com/watch?v=dQw4w9WgXcQ)!


### n<sup>th</sup> time
If you're producing a large quantity of your board, you want an automated way to test if one works or not. This is normally done by making something called a __test fixture__, which is a thing that connects to your board, applies some power/signals, and tells you if it produced the right outputs. These normally connect with something called _pogo pins_, which are little spring loaded pins that you push against a board to make contact with its pads. That might look something like this:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/NxxQZ3JyjiE?start=32" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

And then there'd be some circuitry connected to the pogo pins that does the testing.

# Appendix: How else can I put circuits together?
Make no mistake! You don't need a PCB to put circuits together! There's a bunch of options for putting circuits together, depending on what exactly you're trying to do. Here's a handful of them:

### Solderless breadboards
You might have seen these before, and they're a great way to put together circuits for prototyping. They're a block of plastic with a series of holes in them, and underneath the holes live a set of metal spring contacts that connect to the metal leads coming off of your part. By sticking your parts on the breadboard, the metal leads on your parts go inside the holes and make contact with the metal springs below. Doing this a bunch of times for all the parts lets you connect them together, and make a circuit!

![](breadboard.jpg)

*Image Courtesy of [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Breadboard.png)*

![](breadboard_circuit.jpg)

*Image Courtesy of [SparkFun Electronics](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard/why-use-breadboards)*

These metal strips are arranged like shown - most of the strips are horizontal, but there's some long vertical ones going down the side of the breadboard that are meant to be used for power, since a good bit of your circuit usually needs to be connected to power and ground. 

While these are great for putting things together quickly and prototyping fast, circuits built on them don't always last very long - after all, it's just friction holding them in. And more importantly, not every circuit will fit onto a breadboard. Breadboards only work with through-hole components, which might cause problems for your circuit. A lot of components come in both through-hole and surface mount versions, but some are surface mount only, and don't come in a through hole version. This affects chips (integrated circuits) the most, and so adapters like these are pretty common to make surface-mount devices breadboard-friendly:

![](smd_adapter_board.jpg)
![](smd_adapter_board_assembled.jpg)

*Images Courtesy of [SparkFun Electronics](https://www.sparkfun.com/products/13655)*

Breadboards almost exclusively use a 0.1" spacing between holes, which is the spacing between leads on most through-hole ICs. Most through-hole ICs come in what's called a _DIP_ package (short for _Dual, In-line Pins_), which also uses the 0.1" spacing. DIP chips fit in 0.1" grids, which nearly all breadboards are.

### Solderable Breadboards
These take the breadboard concept and turn it into something more permanent. They're a specially laid out PCB that has a series of holes just like a breadboard does, but instead of connecting to the part with a spring, you solder to it. This means chemistry's holding in your part instead of friction, and makes something that's way more durable:

![](busboard.jpg)
![](busboard_circuit.jpg)

*Images Courtesy of [Adafruit](https://learn.adafruit.com/collins-lab-breadboards-and-perfboards/learn-more) and [jmsaavedra](https://www.instructables.com/Perfboard-Hackduino-Arduino-compatible-circuit/)*

These are also known as a _busboard_, depending on who you talk to. These seem to be pretty popular in electronics education because it's really easy to transfer a circuit on a solderless breadboard to a solderable breadboard. Solderable breadboards also have cousins called stripboard, which replaces the breadboard-style connections with strips that run down the entire board:

![](stripboard.jpg)

*Image Courtesy of [Wikipedia](https://commons.wikimedia.org/wiki/File:VEROBOARD_sample.jpg)*

Usually you don't need the entire strip to make the connection, so you cut the copper on the board to the length you want. This stuff is also sometimes called _Veroboard_ since Vero was the first company to make it.

### Perfboard
These take a more free-form approach than the solderable breadboards do. These drop the bread-board like connections, and instead are just a grid of solderable holes. These are also called _protoboards_ in some circles. In my experience, these are the most common in the real world. 

![](perfboard.jpg)
![](perfboard_circuit.jpg)
![](perfboard_smd.jpg)

*Images Courtesy of [Circuit Specialists](https://www.circuitspecialists.com/64-8933.html), [Jiří Praus](https://twitter.com/jipraus/status/1106990634156605440), and [osgeld](https://www.instructables.com/Put-Your-SMD-Parts-on-Standard-Perfboard/)*

It's also pretty common to route connections across the board by just adding solder across holes you'd like to connect, as shown above. Jumper wires that 'jump' across the board do the same thing too, just like we saw in breadboards and busboards. These also come in both 0.1" or 2mm grids - 0.1" is the most common, but some metric parts are on a 2mm grid.


### Point-to-Point Construction
The 'boards we've covered thus far are the most common ways to put circuits together, but there's a few other methods that are worth mentioning, either for historical significance, artistic freedom, or just plain coolness.

Point-to-point construction falls into the former. It's how circuits were assembled before the concept of a board emerged, and it's quite simply just components soldered to each other in the shortest path possible. Larger mechanical parts were mounted to the case, but smaller things were usually just left floating. Here's the insides of a 1948 Motorola VT-71 7" television to show what I mean:

![](point_to_point.jpg)

*Image Courtesy of [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Motorolagoldenviewchassis.jpg)*

As you can imagine, repairing or debugging this would be...not fun. But it worked! And circuits of moderate complexity were mass-produced by skilled workers using this technique. 

### Wire Wrapping
We eventually moved on to wire wrapping, which got popular in the 60s and 70s. Everything was mounted to a board, but the connections between modules were made by tightly wrapping wire around the legs of components. Doing this with the proper tool made a super tight, robust connection that looked about like this:

![](wire_wrap_closeup.jpg)

*Image Courtesy of [Jameco Electronics](https://www.jameco.com/Jameco/workshop/TechTip/wirewrap.html)*

This was used on big boards that had components on one side, and then wiring on the other. Here's an example that shows this:

![](wire_wrap_top.jpg)

*Image Courtesy of [Nuts and Volts](https://www.nutsvolts.com/magazine/article/wire_wrap_is_alive_and_well)*

This was extended to really, *really*, **really** large systems, like mainframe computers. Here's what the backplane of a PDP-11 (the first machine to ever run UNIX) looks like. This was a computer the size of a few filing cabinets, and this is just one of its modules.   

![](wire_wrap_bottom.jpg)

*Image Courtesy of [Trammell Hudson on Flickr](https://www.flickr.com/photos/osr/7152104275). Click to zoom in on it, it's a work of art.*

Wire wrapped circuits were super reliable and serviceable - probing around inside a circuit was super easy since all the wires were exposed, and if you needed to adjust something, you could just unwrap the wire and move it. Despite this it had a limit on how densely it could be routed and manufacturing it was somewhat difficult to automate, so it fell out of popularity around the time the integrated circuit came on the block in the late 1970s. ICs work by routing a bunch of flat layers of silicon together to make transistors and connections between them - which is a design paradigm that's exactly the same as our modern PCBs. Needless to say the mentality of 'many flat layers connected together' stuck, so now you'd be hard pressed to find anything other than a PCB in your electronics these days.

### Free-form assembly
This technique doesn't use any kind of structure at all, and really just lets your circuits flap in the breeze. Most of the time when Adi and I do this it's because we're lazy,[^1] but it's a genuine art form when well-executed. I can't think of a better example of this than [The Clock](https://techno-logic-art.com/clock.htm), which is absolutely beautiful:

![](the_clock.jpg)

And [Mohit Bhoite](https://www.bhoite.com/)'s VU meter is adorable:

![](freeform.gif)

As is [koogar's](https://www.instructables.com/Crystal-cMoy-Free-Form-Headphone-Amplifier/) headphone amplifier, which they set in epoxy resin:

![](freeform_amp.jpg)

### Manhattan Style
Manhattan Style assembly works by cutting raw double-clad FR4 into a bunch of little squares, soldering those to a larger board, and then using those little squares as connection points in the circuit. This is best explained with a picture (actually from the same project as we saw the FR4 enclosure for earlier):

![](manhattan.jpg)

*Image Courtesy of [Dave Richards AA7EE](https://aa7ee.wordpress.com/2011/07/24/the-wbr-a-simple-high-performance-regen-receiver-for-40m-by-n1byt/)*

Since these small pads rise up from a bigger pad like skyscrapers, it's called Manhattan Style. It's really popular for prototyping RF stuff since it lets you space your parts however you'd like, and you get a big ground plane underneath everything. It's not very compatible with ICs though, unless you flip the chip upside down in what's called a _dead bug_ arrangement:

![](dead_bug.jpg)

*Image Courtesy of [IEEE Spectrum](https://spectrum.ieee.org/with-the-dead-bug-method-hobbyists-can-break-through-the-highfrequency-barrier)*

In this case they've removed some sections of the top copper to make the isolated 'islands' that would otherwise be made with the small bits of stacked FR4 - but the principle is the same.

That's all for now! Glad you made it to the end, and let us know on Piazza or email if you've got any questions.