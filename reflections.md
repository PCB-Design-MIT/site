---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Reflections
description: Thoughts from Winnie + Will from running this class

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
# IAP 2024 Reflections

## Lecture 01

### Winnie

- Think I went a bit fast on lecture, probably could have covered more stuff
- It just felt kinda like... oops I ran out of slides? When I practiced I was at ~ 40 minutes, but looking back, I think I practiced when I was also correcting my slides. I was practicing to ensure that I wouldn't go over 50, but didn't consider how far under I'd go
- Students didn't seem to stop me for questions. I probably should have asked more often for Q&A

## Lab 01

### Winnie

- I could have prepared a better announcements speech, I don't think I thought about it hard enough and lost my words
- Lab 1 should have been lab 0 according to some feedback, though I think for this class (just 6.2000 knowledge) it might make sense to have this as lab 1
- 7/10 of students use KiCAD :0 We surprisingly didn't have computer issues during lab :)
- Need: symbol assignment guide
- Need: heiarchical vs global vs net labels. Students seem to be a bit confused on it
- We kept the LED + header for less stuff to rewrite, but maybe it should change? Then again some people did spend 2 hours on lab

## Lecture 02

### Winnie

- I don't know how to strike the right balance between covering enough things that might pop up, but also making sure they're covered adequately. I think I tried to cover too much in too little time... maybe 1 hr lectures don't work well? I felt like I had to cram everything into a 50 minute lecture, and I think having a 1.5 hour lecture would've been nice so I could get more content in. Just thoughts for next year.
- I don't think it stuck that well, still seeing some things pop up in lab where I remind students of lecture. Maybe a longer lecture would help.
- Maybe this should include "how to select a microcontroller pin"? It came up a lot during lab

## Lab 02

### Winnie

- having a list of announcements helped me a lot, I could go up and speak and remember all the points I have to cover
- Seemed to go pretty well, people submitted on time for the most part
- Having LAs staff lab was ***so*** helpful, I can't emphasize this enough. We could bring up stuff to change as lecturers, and LAs could go around and help students when we were busy reviewing schematics
- I reviewed stuff early and stayed up (I purposely asked for a midnight submission so we could review them before lab) and I think that helped me stay on time. I think Will did stuff in real time and personally I can't look at schematics in real time as well as I can look at them before and give feedback day of
- Got feedback that lectures should cover more depth, will change that in the future

## Will

- The KiCAD and layout split is really cumbersome and problematic to support. Need a better split of LAs (most currently known Altium) ir cross platform knowledge/training but that's hard to get

## Lecture 03

### Winnie

- Mics mixed with IS&T very likely. Oops. Why the heck do they need a paper clip to repair? Ugh.
- Teaching both KiCAD + Altium is a pain, need to switch between how to use both softwares when doing demos
- Got feedback that we were assuming students took 6.2000. (More general than lecture 03) Fair criticism, I think this is my bad since I assumed and forgot that some students haven't. Also mentioned that the website is good for more information, might keep the format where lectures are an overview and website has more information.
- Post recording comment: turn up mic gain

## Lab 03

### Winnie

- Holy shit don't order your boards close to a holiday. We ordered from PCBWay on Jan 2 and got told 24h turnaround, observed 5 day turnaround. It delayed our demo board bringup by almost a week, and I almost went into panic mode because the boards may not have shown up. I don't trust PCBWay now and I want to double order JLC + PCBWay to be safe. I can't take the risk and it's worth the extra few $200 or so.
- Digikey seems to be shipping slower than expected when going through B2P (coupa). Ordered 1/3 and received 1/10. Might be a new years thing? But we'll plan to order stuff earlier for the real boards students get. Luckily the BOMs are due by Lab 3, and we can order stuff soon after this lab so they arrive in time.
- Oops forgot to not do solid ground pours. Guess they'll be really good at soldering.
- Think there's a bit too much stuff to solder. One person was done with white + blue at 11:45 (end time 12) so the 2 strings setup is handy. I think not doing ground pours + reducing LED count to maybe 8 might help?
- some people didn't read our lab and ended up with placement issues (honestly I think we provided a fairly clear explanation for things)
- snowstorm go brr, T go burrnnnn and people came a few mins late
- the plastic can be a good indication that you're sticking too much heat into the part and people learned that the hard way. may want to consider getting some desoldering method in the future
- checking BOM and layout hard. tbh I was expecting to catch all errors, and now I'm leaning towards getting stuff to be fixable. Fixable meaning someone can run a jumper / resolder resistors / cur a trace and it'll still work. Of course this shows just how time intense things can be, and I don't have a good understanding for how week 4 will go because of this. I'm glad it's a small class, quite honestly I wouldn't know how to handle a class that's any bigger than this.

# Overall thoughts on projects

- at this point, I've recognized that the class has designed a lot of circuits! I can't access how well they know the circuit they're designing, but I do think that giving students more freedom in making stuff helps in getting them to get more rigor out of the course without feeling overwhelmed. I believe last year we had a lot of comments on how people didn't understand what they're designing, and this year I'm watching people add stuff to their circuits to get a fair amount of complexity despite not having much of a background. If I had infinite resources I think this would be a very motivating factor in allowing students to pick their own project (albeit with some limitations). Of course, resources (people and money and time) are limited, so this may not be possible :(
- please no power supply it's too complicated
- the plush project (xyla + becky stern) seems to be fairly expensive :( I think maybe a cheaper pressure sensor + cheaper battery might work? but each board is close to $30-$50. Battery was $15, pressure sensor was $10, ESP + USB + stuff is $5.
- Honestly so glad we're using just the ESP32 S3 for the class. USB routing is the same for everyone, the cost savings is huge because we don't need to pay like $4 for a stinking USB Serial IC.

# Lab 04

## Will

- (Context: we had a Ask-Me-Anything lecture regarding layout, so for the first 30-60min was lecture on considerations for PCB layout, similar to lecture 4)
- Not much engagement from students, but to be expected with such a small class.
- I'm not sure how useful the lecture was, maybe it would have been better to stick with LAs going around looking at boards
  - Not sure how much LA consensus there is regarding layout design, as in if they know to layout as was discussed in lecture 4, perhaps something to address via LA training or something like that
- Impedance analogy wasn't very great from both myself and Winnie, I feel it didn't explain impedance very accurately and made perhaps too many abstractions. Need to come up with a better explanation or perhaps include no explanation at all
  - Would be easier to explain for those with 6.200 or greater background
- Didn't have enough time to talk about each student's layouts, did the not check the layouts of some students I should have checked.

## Winnie

- Agree not much engagement, though that seems consistent with our other lectures
- I think the information presented was useful, and there's a different argument to be made about whether it was worth the tradeoff with less review time. In the future I think these topics should be incorporated into lectures, as I don't think lab was the best time to have a lecture. Feedback seems to suggest this too, one comment said they wished lecture 4 went in more depth, another saying they prefer 1:1 time in lab.
- Def need a better impedance analogy, I didn't think of it too hard and didn't have the time to.
- for the future: I think will spent too much time per student. This isn't like a negative thing-- I think students should get a lot of time with instructors. The problem comes when you have n>100 students and logistically it's a nightmare.

# Lecture 5

## Will

- Great demos,
  - PWM dimmer demo
  - logic analyzer with datasheet demo

## Winnie

- Feedback for lecture seems to be positive
- demos took so long to work
- TBH the last bit with the numbers got confusing. I was expecting this, even I didn't understand it well. I think we should pick and choose what devices to use better so it's not a word blob.
- I wrote it the night before, website will be updated late

# Final Board Layout Checks

## Will

- Need to make a better standardization guide for creating a Github for the KiCAD boards (also can explore doing so for Altium's)
  - Procedures for students continuously pushing their board changes and using forking/pull requests for staff changes needed
  - Possible could utilize MIT GitHub enterprise system
- Database library for common passives might also be helpful

## Winnie

- I hate this process it's so tedious and time consuming
- should've used Github more, agree with you there Will. I didn't write good instructions for git and now it shows the pain points
- Altium is confusing as fuccckkk I spent 3 hours after midnight fixing board issues and export issues.
- Give a better guideline to LAs for what to check for/fix. There was some ambiguity and that needs to be cleared up. Also I think we should errr on fixing less things for students since part of debugging is fixing your own mistakes.
- We only really edited one student's board since they had over 10 deal-breaking errors.
- Solder mask expansion and USB-C mounting slots are things to look out for
- DBLib would be cool, though idk how to set it up for KiCAD. Maybe someone needs to sit down and say "these are the components you can use" or something to standardize stuff more.

# Lecture 06

## Winnie

- Wrote it up like yesterday, didn't know runtime. I feel like it's "okay"? The hard part is done so I think I'm okay with the shorter lecture.

# Lab 05

## Winnie

- Paste took ~ 30 mins for 10 people. Could've streamlined it better.
- Taping LEDs before pasting should've happened, but didn't for everyone. Had excess solder paste that people needed to clean off.
- Lab went a bit long, I think removing some stuff like ESP32 -> S3 would help.
- Everyone got their thing soldered!
