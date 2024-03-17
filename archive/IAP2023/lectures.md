---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Lectures
description: Lectures are held MWF 1-2:30 in 34-101. Recordings are posted roughly a day after the lecture.

# Author box
# author:
#     title: About Author
#     title_url: '#'
#     external_url: true
#     description: Author description

# Micro navigation
# micro_nav: false

# Page navigation
# page_nav:
#     prev:
#         content: Previous page
#         url: '#'
#     next:
#         content: Next page
#         url: '#'
---

# Week 1

#### Lecture 01 - Design as an Art + Process

- [content](./lecture_01) [video](https://www.youtube.com/watch?v=tYbshePrX4I)

#### Lab 01 - Introduction to Altium Designer

- [content](./archive/IAP2023/lectures/lab_01)

#### Lecture 02 - PCB Structure, Components, Schematic Capture

- [content](lecture_02) [video](https://www.youtube.com/watch?v=NnycUXUgmk0) [slides](https://pcb.mit.edu/static/slides/lecture_02.pdf)

#### Lab 02 - Schematic Capture (Case Study)

- [content](lab_02)

#### Lecture X - Motors + Motor Control

- [content](lecture_x) [video](https://www.youtube.com/watch?v=Sb1JIDUw3kM) [slides](https://pcb.mit.edu/static/slides/lecture_x.pdf)



# Week 2

#### Lab 03 - SMD Components and Packages

- [content](lab_03)

#### Lecture 03 - LAYOUT! Yay!

- [content](lecture_03) [video](https://www.youtube.com/watch?v=y0QJRCx4N0A) [slides](https://pcb.mit.edu/static/slides/lecture_03.pdf)

#### Lab 04 - Bluetooth Speaker Layout

- [content](lab_04)

#### Lecture 04 - Bluetooth Speaker Design

- [content](lecture_04) [video](https://www.youtube.com/watch?v=4PWwAQjCfhA) [slides](https://pcb.mit.edu/static/slides/lecture_04.pdf)

<div class="callout callout--warning">
Quick Errata: In the lecture recording, I was asked about how to spec the LC filter on the output of the Class-D amplifier, and mistakenly said that we're trying to filter out the harmonics of the switching frequency, but leave the fundamental. This is untrue. We're trying to filter out the switching frequency <em>and</em> its harmonics, but leave the spectra belonging to our input signal alone. I didn't draw the spectra of our input on the Bode plot I drew on the blackboard which is my mistake, but if I did we'd see another peak far below the switching frequency. And that's the peak we'd want to preserve, while smearing out everything else. -fischerm
</div>

# Week 3

#### Lecture 05 - Population + Debugging

- [content](lecture_05) [video](https://www.youtube.com/watch?v=YX0Y1V_SeqU) [slides](https://pcb.mit.edu/static/slides/lecture_05.pdf)

<div class="callout callout--warning">
Quick Errata: In lecture a student asked towards the end "we should figure out why the diode blew right?" This is CORRECT because after-all the root cause of the problem was the fact that the diode blew causing the relay to blow, causing precharge to fail, causing the contactor to weld, causing the overcurrent error during startup. I want to explain WHY I didn't address the question in detail because as engineers we should find out why the diode blew. The answer is I DON'T KNOW why the diode blew. We tried debugging that for some time, we tried going through the circuit to see what could have caused the diode to blow and came up empty. The best explination we had was it was an old diode, and maybe it just failed. It's unsatisfying for sure. If I do find a better explination, I'll update it here. -adim
</div>

#### Lab 05 - Population + Debugging

- [content](lab_05)

#### Lecture 06 - Layout Tutorial (in more detail)

- [video](https://www.youtube.com/watch?v=pGbh2fy4VME)

#### Lecture 07 - Intuition Building + the Importance of Art

- [video](https://www.youtube.com/watch?v=FST6fiX4XhU)


# Week 4

#### Lecture 08 - Hardware's not Dead.

- [video](https://www.youtube.com/watch?v=TlRmg9Cv0sk) [slides](https://pcb.mit.edu/static/slides/lecture_08.pdf)

# Extra Tutorials

#### Installing Altium Designer
- [content](altium_setup)

#### Exporting + Ordering a PCB from a Service
- [content](board_fab)

#### Design a Mechanical Housing for your Speakers!
- [Cat Arase's Acrylic Speaker Box](mechanical)
- [Lee Zamir's Speaker Box + Mertopolis Class on Speaker Design]()
