---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Assignment 5 - Bringup Plan
description: We'd like you to start considering how you plan to bring up your PCB. Staff don't have pre-planned bringup instructions for you! This means you'll have to write your own. We don't need this to be super detailed, we just want you to consider bringup more than simply turn it on and hope it works. We don't want you spending more than 2 hours on this. 


# Micro navigation
micro_nav: true


---

# Suggested Reading

- Chapter 8.2 of <u>Designing Electronics that Work</u> covers Bringup. It should be a short read. The rest of chapter 8 is what you'll see in industry, and we won't go into such rigorous bringup processes. 

# Updating your block diagram

You're going to want to update your block diagram from back in assignment 01. It's important to have the blocks from there match the blocks you have in your board. 

# Creating a test plan

Consider the order you want to test your blocks. Recall from Lecture 6 - test power first, then the blocks connected first to power blocks. For each block, state what your inputs are and what you expect the outputs to be. If it's a digital system, we suggest including a screenshot of the communications protocol timing diagram. Optionally, you might find it helpful to list out the I2C address of each chip you're using, if applicable.  

# Turning it in

Send it to your channel on Slack. Any format is good with us, this is mostly a document to help you. Please get it in by Monday night, and we'll review it by Tuesday lab. 