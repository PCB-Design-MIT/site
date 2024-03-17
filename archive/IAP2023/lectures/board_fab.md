---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Fabricating Boards
description: What files you need for the fab, how to prep them, and how to order them.

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
        content: Lectures
        url: '../../lectures'
---

Most of the time when you're fabricating your boards, you're going to need to generate gerbers. These are files that describe what each layer looks like on your board, so there'll be one to show where the copper goes, one for the silkscreen, another for the soldermask, and so on. Here are the Gerbers for the Bluetooth Speaker's top and bottom copper:

![]()

And if you'll notice, there's a bunch more for the other layers. We'll need to get Altium to generate these so we can give them to the fab, along with some extra information about how we'd like our boards to be made.

# What's an OutJob?
Altium generates Gerbers with something called an OutJob - a little tool that can make any kind of outputs we'd want from our project, including Gerbers. Here we'll just be using it for that, but you can also have it generate `.pdfs` of your schematics, a `.csv` or `.xlsx` of your BOM, or a `.step` of your PCB geometry. We've got a preconfigured OutJob for 2-layer boards that ~~we stole from FSAE~~ seems to work well. It's [here](2_layer_PCBWay.OutJob). If you need to make a custom one, the [Altium Docs](https://resources.altium.com/p/generate-gerber-files-altium-designer-step-step-schematic-pcb) have got you covered.

It's worth noting that our Gerbers only include what goes on each layer - but some of the features on our boards go _through_ layers! Vias, through hole pads, and mounting holes all get drilled into our board, and the gerbers don't specify those locations. So we'll also need a list of holes to drill, which is commonly produced as a `.txt` file with the X/Y coordinates of each hole, along with the drill size. This is called the _NC Drill file_, because we were making PCBs back when CNC machines were just NC machines.

# Producing Gerbers
Go ahead and add the OutJob to your project by dragging it on top of the project in the Project window, or by right-clicking the project and hitting _Add Existing to Project_ and selecting the OutJob. Double click the OutJob, hit generate, and you should be off!

If you add the OutJob to your version control in the side panel (right click the OutJob, hit _Version Control_, and then _Add to Version Control_) then A365 will be able to use it in the web portal. This means that you'll be able to click the download button in the corner of your Project page and download your Gerbers as a `.zip`, schematic printouts as a `.pdf`, or CAD as a `.step`. This is __incredibly__ handy.

# Fabrication Options

We're choosing to use PCBWay as an example in this tutorial to thank them for sponsoring the class, and also because it's the fab house we use the most. But there's a bunch of other fabs on the [resources page](https://pcb.mit.edu/resources/) if you're curious.

Grab your zipped Gerbers and NC drill file and head over to your fab's website, and they should have a gerber upload screen:

![]()

Throwing in your board dimensions, layers, and desired quantity gets you to the following:

![]()

At the top you can select the service that you want - if you want a PCB that's not just the normal sandwhich of copper and FR4, select one of the tabs at the top. Otherwise, you've got a fair number of options, and the little green help buttons will explain most of them - and the defaults are usually the cheapest. However, we find ourselves messing with a few of them rather often:

- __Solder Mask__: The gerbers only describe where the solder mask goes, not what color it should be - which you get to pick! Green is by far the most common color, so it's the default. Other colors are available too, but they don't always exactly match the exact shade that's on the website, so if you're particular about the color of the board, look around for pictures of other people's boards made from the same place with the same solder mask. If you don't want any solder mask and you want all of the metal exposed, you can also select that option.

- __Silkscreen__: Just like the soldermask, you get to pick the color of the silkscreen. Generally we use the silk for small text that labels components, so we mostly only care that it's readable. Black and white are the most popular, but if you don't want silkscreen, you can specify that too.

- __Surface Finish__: Parts on your board are connected with copper on the top/bottom/inside of your board, but copper tarnishes rather easily in air, which makes it hard to solder to. To keep it from oxidizing we choose to apply a surface finish, which is electroplated onto the copper. The surface finish you want depends on if you're going to be soldering components on with leaded or lead-free solder (putting a little lead in the the surface finish can help the solder wick onto the board better), but if accidentally use the wrong one it doesn't matter too much. HASL with lead or HASL lead free are the most common and cheapest finishes, but you can get a thin layer of gold plated on with ENIG, or a thick layer of gold with Hard Gold. Or you can do the same with silver. Or you can forgo the surface finish alltogether and just have bare copper.

- __Finished Copper__: This specifies how thick the copper layer is on your board, and it's rated in ounces. This refers to the weight of the copper applied to the board's area - a board with 1oz copper will have 1oz of copper per square foot, and a 2oz board will have 2oz of copper per square foot. This convention is rather annoying - but if you do the math out, 1oz copper is about 35 microns thick. PCBWay will go up to 13oz copper, which is just under half a millimeter thick.

- __Via Process__: This option tells the fab whether or not they should coat your vias with soldermask. If you choose to _tent_ your vias, they will apply solder mask over the via, putting a thin film of it over the top and bottom of the hole. This leaves an empty void inside the via, but if you'd like that filled too, then you can _plug_ your vias and fill them entirely with soldermask. At PCBWay, tented vias are free, but since we have the gerbers to tell us where to put soldermask, they'll only do it if your gerbers specify it.

# Ordering

Once you've selected the options you'd like, you'll save the order to your cart and upload your design files. This will send the order to an engineer will inspect your gerbers, NC drill, and board specifications. If everything looks good, the order will be approved, and then you pay and the boards are fabricated. If it doesn't they'll let you know what's wrong, and then it's on you to fix it.