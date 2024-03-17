---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Lecture 1 - Design as an Art + Process
description: Design is a process, and it's a super fun one too! The best way to learn is by doing, the second best way to learn is by being shown the design process end-to-end. Here we show you the design process for a hydrogen-powered motorcycle focusing on the use of physics in the design. It's not a PCB, but the way we break things down is very similar in both cases.

# Author box
author:
    title: Fundamentals of Design
    title_url: 'https://meddevdesign.mit.edu/fundamentals-of-design/'
    external_url: true
    description: Good resource on design from Professor Alex Slocum

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
# 00 — Why this lecture?

Ok fair, valid question. This is a PCB design class after all and I'm about to spend an hour NOT talking about PCBs. But humor me for a second.

When I was an undergraduate student @ MIT, I felt like I had a lot of tools. Equations, tid-bits of knowledge, experiences. But part of me was unsure how to combine all of these to actually DESIGN something. Sure I'd BUILT things before. Sure I'd done math for my classes before. But combining those two was a struggle sometimes in how to turn that math into something that informs our design in a defined, measurable way. This, I think, is both harder, and easier than most people think.

Maybe you feel like you know how to design something, maybe you have a process that works for you. And if that's the case, maybe you don't need this lecture and you can skip to the next one.

But maybe you're like me, where you feel it would be super beneficial to WATCH someone design something, watch HOW they develop design questions, answer those questions with physics. And then, how they use the physics in the design to attain a specific, measurable performance metric.

<div class="callout callout--success">
So my goal with this lecture will be to show you how very smart you actually are. I'll present some brief "good design principles" I like to follow, but then mostly just show you (in an interactive way) how I designed a thing with like actual math.
</div>

YOU'RE SMART, YOU'RE GREAT, YOU CAN DO ANYTHING. We just want to help you get there :) <3

![our favorite quote from the Mythbusters](failure.jpeg)

# 01 - Physics doesn't care about your feelings

I like to use the following definition of design —

<div class="callout">
Design is the process of combining art + science to solve a problem. It is the act of ensuring a system satisfies an initial set of fundamental requirements to perform a desired function. It is rooted in physics but it is also a beautiful method of self-expression. You can tell how passionate someone is by the quality of their design.
</div>

Essentially, it's you asking the question, "I need to make this thing I want to make do a thing, what things do I have to do so the thing can do the thing."

That's a lot of things, but you get the idea.

It turns out you already know the answer to the question, "what is good design?" I just need to convince you that you do. Think of your favorite products, and list reasons you like them. For me, it's this.

GOOD DESIGN IS...
- functional... it works
- durable... it works more than once
- detailed... it doesn't ignore things it shouldn't
- intelligent... well thought out for a specific application
- SIMPLE SIMPLE SIMPLE SIMPLE
- and an artsy / creative flair is always a bonus :)

The nice part about this is design isn't fundamentally difficult, only difficult to execute. In fact being a good engineer isn't hard either and starting is very simple. To engineer really is to just ADMIT YOU DON'T KNOW SOMETHING, and then to go learn it. Knowing what it is you don't know takes you farther in the design process than most other things. It's ok to not know. There's many things many of us do not know.

So start with the problem, identify what we do and do not know, learn what we don't know, and **use physics to prove every statement we make.**

<div class="callout callout--danger">
Physics is our fundamental limit, physics does not care about our feelings, that's why we need to care about physics. But we like physics! So that's OK :)
</div>

Without further ado, let's design a motorcycle.

# 02 - Design of a Hydrogen Fuel Cell Powered Motorcycle

In the lecture video I will go through everything in detail in the following order.
- High level design requirements, how did we get these high level requirements?
- Systems-level understanding of a HFC motorcycle, what components are we designing?
- Feasibility study of hydrogen-powered vehicles
- Component selection using physics (battery, fuel cell, motor, gear ratios)
- PCBs + other protection circuitry needed for this bike (for future work)

In these lecture notes, I'll link the design paper that was written for the 2.70 FUNdaMENTALs of Precision Product Design class as it contains the information in detail. The lecture video will be posted as well. The paper is really the best set of notes I could've written for this, the lecture video probably has better explainations.

[DESIGN PAPER PREVIEW — HYDROGEN POWERED MOTORCYCLE, RELEASED FOR CLASS ONLY](https://drive.google.com/file/d/1ZRbdgjy_SRn_V38HRuDLAPU6Pffzuthy/view?usp=drive_link)

!['toothless' the HFC bike](bike-1.png)

# 03 — Thinking about PCB Design @ a High Level

PCB design is very similar to the process we just presented. We have to identify what a PCB needs to do, we need to divide those functions into system components and a system diagram, and we need to use physics to specify each component while considering other parasitics in the later, more detailed levels of design.

![a tasteful depiction of an atom](AD.png)

### you are an electron herder, but don't take it personally. electrical design and PCB design really are just about herding electrons from where they are to where you want them to be.

The following lectures (and the rest of the class) will try to breakdown all the parts of a PCB and how to design them, what considerations there are for certain components, and how to specify components, read datasheets, route pairs, separate power and signal, and more!

# A01 — Extra Resources
[MIT Solar Car 'What is Good Design?' Presentation](https://docs.google.com/presentation/d/1rmKZ-h0TRs0zH-Cb3S1vvYz99pOnX4bk8C4aLKD9BdA/edit?usp=sharing)

[MIT 2.70 — Fundamentals of Precision Product Design](https://web.mit.edu/2.70/)

[Additional Resource on Motor + Motor Testing](https://www.adim.io/_files/ugd/067a81_c7165eb397794bbf8eab9d27706da229.pdf)

# A02 — Interactive Design Sheets

Please open these using 'PAGES' on 'MAC' — [Interactive Design Sheets for HFC Motorcycle](https://drive.google.com/file/d/1LViTJlUcTCg_5fHZ4LiYjy-qjI_ScLZ1/view?usp=drive_link), here's a [PDF version](https://drive.google.com/file/d/1VxFqY1ZHXqymq0rEDPpHuAAuoWUEWyE6/view?usp=drive_link) for my windows/linux users.