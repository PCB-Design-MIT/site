---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Lecture 04 - Bluetooth Speaker Design!
description: How does the speaker work from spotfy to diaphragm?

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
        content: Lecture 03
        url: '../lecture_03'
    next:
        content: Lecture 05
        url: '../lecture_05'
---

# Reference

## Flash your ESP32!

<script
  type="module"
  src="https://unpkg.com/esp-web-tools@9.0.3/dist/web/install-button.js?module"
></script>

<esp-web-install-button
  manifest="/media/firmware/manifest.json"
></esp-web-install-button>


## Firmware Source Code

Available on our [GitHub](https://github.com/PCB-Design-MIT/speaker-firmware)!

# How was this designed?


## What should it do?
Good question! As with all good design, we'll start with design requirements:

- Accept audio data over bluetooth, just like a normal bluetooth speaker or pair of wireless headphones/earbuds.
- Play said audio data out over a speaker, at a reasonably loud volume. We're cool people, and cool people have loud speakers.
- Be portable, and include a battery or battery pack of some sort.
- Be powered by 18650 cells, since we had a bunch of those lying around (thanks Joe!).
- Handle the batteries safely, including reasonable protection circuitry to prevent overcharge and overdischarge.
- Be chargeable over USB, since most people have USB chargers lying around.
- Be about as large as a normal off-the-shelf speaker.
- Be programmable, so that people can hack with the firmware in case they'd like to mess with the EQ, add RGB lighting, or whatever else.
- Sound pretty good.
- Be as cheap as possible.


## Bluetooth Profiles
The speaker uses an ESP32 to do most of the heavy lifting. It's a 32-bit microcontroller, with a builtin WiFi and Bluetooth modem. We'll use the Bluetooth modem so that it'll connect to a phone/laptop/smart fridge. Bluetooth itself is actually a stack of protocols (much like Ethernet), but at the top of the stack are things called _profiles_, which are application-specific specs that a Bluetooth device can implement to accomplish it's desired function. Bluetooth's been around for 20 years and there's a ton of protocols, but you've definitely interacted with a few:

- The Human Interface Device (HID) profile is used for keyboards, mice, and interestingly enough, Wiimotes.
- The Headset Profile (HSP) is used for, well, headsets. If you've taken a call through a set of wireless headphones/earbuds, you've used HSP.
- The Hands-Free Profile (HFP) which is meant for taking calls in cars, which because of historical reasons gets its own protocol instead of just using HSP.
- The Advanced Audio Distribution Profile (A2DP), which streams generic, stereo audio between two Bluetooth devices. This is the profile we'll have our Bluetooth speaker use, and it's not an uncommon choice - basically every Bluetooth speaker/headsets/headphones/earbuds use it.

Our firmware is responsible for implementing this spec, getting audio data out of our phone/laptop/smart fridge, and into our speakers. A2DP uses a bunch of other specs in the process (it's kind of alphabet soup the whole way down), but thankfully the makers of the ESP32 (Espressif) have given us a Bluetooth API through a set of C libraries, which takes care of all of that for us. Phil Schatzmann combined said A2DP library with Espressif's I2S libraries (more on that in a second) and rolled into the lovely [ESP32-A2DP](https://github.com/pschatzmann/ESP32-A2DP) project, which our firmware ~~pretty much exactly copies~~ inherits from.

## USB-UART Converter
Getting this firmware onto our ESP32 is also a nontrial task that requires external circuitry. We program ESP32s by loading them over an interface called UART, but we write programs on computers that don't have UART ports.[^0] We do have USB ports though, so we'll include a USB-UART bridge to let us program the ESP32 with our computers. This is pretty common practice for most microcontrollers, and we've lifted the exact circuitry from some SparkFun/Adafruit development kits.

## DAC
Once we've got our data in our ESP32, we need to get it out to the speaker. The ESP32 can't drive the speaker directly - we need to put many watts of electrical power into the speaker in order to get something reasonably loud, and the ESP can't run that much power through it's input/output (I/O) pins. As a result, we'll lean on external circuitry for this. In our case we'll need to put an _amplifier_ ahead of the speaker. We'll get into the amplifier in a second, but for now just know that we need to feed it with an _analog_ waveform. Let's talk about what this means.

Our audio data thus far has lived entirely in a digital form - a set of bits in the ESP, set to some value. However, our ears don't hear bits - they hear vibrations of air, which are continuous (at least for the music I listen to...well, [mostly](https://open.spotify.com/track/7CUkeiG7QtB7tPU9f8SANS?si=53faaa0ea94742f9)). We need to convert the digital representation of our music that lives on the ESP32 into an analog form. This will later get fed to our amplifier and blasted out the finest speaker $11 can buy, but we need a time-varying analog voltage representing our music first.

We'll do this with something called a Digital to Analog Converter, or DAC for short. The ESP32 has one builtin, but it's only 12-bits and sounds more or less like a potato. We wanted to give you something somewhat respectable (unlike Fischer's music taste), so instead we're using an external, 32-bit DAC. Both DACs do the exact same thing - they slice up their working voltage range (in this case, 0-3.3V) into a finite number of values - it's just that we get more precise control over what value it outputs with a DAC with a higher bitwidth. 

To do the math out here, the internal 12-bit DAC can output $2^{12} = 4096$ values, which when spread over the voltage range, comes out to $3.3V/2^{12} \approx 0.815mV$. This means that our 12-bit DAC will let us control the output to be some voltage within the range 0-3.3V, where a value of 0 produces 0V, and a value of 4095 produces 3.3V; therefore, each step produces a change in the output of $\approx 0.815mV$. Which sounds like not that much, until you realize that the 32-bit DAC gives $\approx 76.8nV$ of resolution.

We'll admit that it's hard to see numbers in the mV or nV range and know what's good sound quality and what's not - but trust us, you can hear it. 

None of this means anything though if we can't get that data on the DAC, which we'll need another protocol for.

## I2S
Normally if we were trying to get arbitrary data from a microcontroller onto a DAC, we'd probably use a protocol like I2C or SPI. However, there's a niche protocol called I2S (short for Inter-IC Sound) that's only for moving sound data between devices. We'll use it because it's the standard, and there's a bunch of DACs specially designed for audio stuff that have some nice bells and whistles on them that make them sound better - and they all speak I2S. And conveniently the ESP library we're using will dump our A2DP data out over I2S by default. Fabulous.

There's a few different styles of I2S, and our PCM5102 DAC can speak a number of them depending on what we do with the FMT pin (pin 16) on the chip. But we've chosen to use the standard variety, which has a timing diagram that looks like this:

![](https://upload.wikimedia.org/wikipedia/commons/a/a7/I2S_Timing.svg)

*Image Courtesy of [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:I2S_Timing.svg)*

There's three signals - Serial Clock (SCK), Word Select (WS), and Serial Data (SD). SCK is generated by the ESP32, and provided to our DAC, and it makes sure both devices stay in sync during the transfer of the audio data. The ESP will send each bit of the _word_ (the binary representation of the audio sample we're trying to transfer) starting with the most significant bit. It'll also get to choose whether or not WS is high or low during the transfer - if it's high, the word corresponds to the right channel, and if it's low, the left channel.

If you look at our boards, these signals are labelled a little differently - SCK is BCK (bit clock) and SD is DIN (data in) - but it's still the same protocol. 

Once we've got I2S data going into our DAC, we'll have an analog output coming out, which we'll pass into our amplifier.

## Amplifier
Which is exactly what comes next in the signal path. Here we're using another chip from TI - the TPA3116. Its job is to take the signal from the DAC as an input, and, well, amplify it. The output of the chip has the same data as the input - it's just at a higher voltage, and can push more current through the speaker coil. 

It's worth noting here that a _substantial_ amount of electrical engineering history has been devoted to making better and better amplifiers, and as a result there's many types out there we could have chosen. This one's called a Class D amplifier, which we picked because it's one of the most efficient topologies, and there's a bunch of chips that wrap up a Class D amplifier into a super convenient little package. And definitely not because it's exactly the same chip as what's in Fischer's [favorite little JBL speaker](https://youtu.be/mH7WxuXU8l8?t=369).

A Class D amplifier works by using four switches to control the voltage on the speaker, like so:

![](class_d.svg)

Each of these switches is actually a transistor - a controllable switch that we can turn on or off. Depending on how we do so, we can control the voltage on each end of the speaker. For instance, if we turn on S1 and S4, we connect the left side of the speaker to V+, and the right side of the speaker to GND. A current then flows through the speaker, making a magnetic field and pushing the magnet in the diaphragm, moving air and making sound.[^2] We could also make current flow in the other direction by closing S2 and S3, and opening S1 and S4. We could choose to have no current to flow through the speaker by turning off all the switches also.

This lets us program the voltage on the speaker, but if you'll notice, we only have three discrete states that our switches can occupy: current going left, current going right, or none at all. This seems like a step backwards - after all the work we put into making an analog signal with nanovolt precision, we're going to throw that away?

Not really. Our audio amplifier is going to switch really fast between these states in order to produce a voltage across the speaker that's _on average_, exactly what our analog waveform is asking for. We'll do this switching really fast so that we can't hear that we're switching between two states - on our particular chip, we switch about 25 times faster than our audio data comes in. This technique is called Pulse Width Modulation, and it's one of the biggest circuit building blocks in all of EE. It does introduce a little bit of extra high frequency noise though, so we filter that out by adding an inductor and capacitor to the output of each transistor pair. This is called an LC filter, and it's a lowpass filter, which lets low frequencies through while blocking the extra high frequency noise we just made.

If you'd like to play with an interactive simulation of this circuit, feel free to check out this little [applet](https://tinyurl.com/2luox4kj).

If you take 6.131/1311, you'll actually get to build one of these amplifiers from scratch - transistors, gate drivers, control circuitry and all. And it'll sound like a potato, because making a good Class D audio amplifier is hard. But we can buy a well-tuned one off the shelf, drop it on our board, and be on our way.

And that's it! That's the signal path, how we go from Spotify to sound waves. Bluetooth to bass. This is the core of what the speaker is meant to accomplish, but there's one last thing that makes the whole thing possible.

## Power Circuitry
None of this would work without a power supply. Which, in the case of this board, is actually pretty nontrivial to engineer. We know we'll need a battery - we've chosen lithium - to power the device, but we've got 

- Power the speaker from a battery, so it can be used while not plugged in.
- Automatically switch over to wall power once plugged in, and charge the battery.
- Charge and discharge the lithium cell(s) within the working voltage range of the battery.
- Provide power to the ESP32 and DAC, which run at 3.3V
- Provide power to the amplifier, which runs between 12-20V [^3]
- Include a power switch to disconnect the battery from the entire circuit to prevent accidental deep discharge.

There's a few bits of circuitry that accomplish this. Fundamentally they all need to work with the operating voltage of the battery (2.8-4.2V for our LiIon cells that use the LiFePO4 chemistry), and there's some nuance to that as we'll see.

### 3.3V regulator
The ESP32 and DAC require 3.3V, which happens to be in the middle of the 2.5 - 4.2V working range of our batteries. This range is actually a little larger than most lithium ion cells with this chemistry (LiFePO4). Most drop out in the ~2.7V range, and ours happen to be special high-discharge batteries that go down to 2.5V since that's what we had lying around.

In our testing we found that both will keep working down to around 3V. This is still in the safe range of our battery, so we'll just let those brown-out. 

### 12V boost converter
We've chosen to power the amplifier off of 12V (more on that in a second) but our battery voltage isn't that high so we'll need a circuit to raise the voltage for us, and keep it constant as the battery voltage changes. For this we've elected to use a boost converter, which is a type of switching converter - a circuit that switches the voltages in the circuit around to convert from one voltage to another, much like we saw in the Class D amplifier. If you're curious how they work, check out the [Wikipedia explanation](https://en.wikipedia.org/wiki/Boost_converter#Circuit_analysis) or take 6.131/6.334!

We didn't have a choice in picking a switching converter (they're basically the only way to increase voltage in an efficient way), but we did have a choice in picking a boost converter as opposed to another circuit topology, like a SEPIC/flyback/flying capacitor converter. These are a little more exotic and have some nice features, but we don't really need any of them for our speaker, and they don't have as high of an efficiency, or nearly as many parts on the market. A boost converter was the move here.

When designing the boost converter, we had a choice in the voltage we wanted it to output. The amplifier takes anywhere from 4.5V to 20V - but that doesn't mean that the speaker will work (or sound good) through the entire range. We did some testing and found that it sounds absolutely _nasty_ at 4.5V, and then cleans up pretty quick after around 10V. We settled on 12V because it's a relatively common voltage, and therefore easier to find parts for - we could only find one in-stock boost converter that went up to 20V from battery voltage, and it had a super hard to solder footprint. We also found that the chip heats up pretty quick at 18V, so running it lower lets us save some heat while not sacrificing the sound quality really. 

There's one design decision we implicitly made here. Speakers usually have an impedance of either 4 or 8 Ohms, and we've chosen an 8 Ohm speaker for cost reasons - the cheapest decent-sounding one I could find happened to be an 8 Ohm speaker. Because Ohm's law works like it does, it means that we need twice the voltage as a 4 Ohm speaker to get the same current. This means we probably could have run the amplifier at a lower voltage had we used a 4 Ohm speaker, but that would have either increased cost or made the speaker sound worse - and we would have needed to include a boost converter anyway.

The last thing is that our boost converter needs to turn off and stop pulling power out of our battery once we've finished discharging it and our battery voltage is low. Some boost converters will include a Undervoltage Lockout (UVLO) pin that'll automatically turn it off once the voltage gets low. Our boost converter doesn't have one, but it's got an enable (EN) pin, which we'll use for the same thing. That's a logic-level pin that enables the regulator when there's 3.3V on it, and disables it when it's connected to ground. In our design we've tied the EN pin to an IO pin on the ESP32 - this means that when the ESP32 browns out it's IO pins will too, turning off the regulator automatically. It also lets us programmatically enable the regulator - meaning we could turn off the boost converter (and therefore, amplifier) when we're not receiving any A2DP data, saving us some battery life. Our firmware doesn't do this, but it's left as an exercise to the reader.


### Battery Charger
Relative to the regulators, the battery charger is a relatively simple affair. All we do is give the battery charger 5V from our USB connector, and connect it to the battery, and we're basically set. We did throw a 2.2K resistor on the battery charger IC to have it pull just shy of 500mA from the USB port, which is the limit for the most conservative USB standard. This means that no device should disconnect itself from our speaker when we plug it in to charge, so we should be able to charge it (albeit a little slowly) from pretty much anything. We're using a MCP73831 for this, which we lifted from some SparkFun/Adafruit boards we like.

### Auto failover
The last thing our power circuitry needs to do is automatically switch to USB power when plugged in. We've actually chosen to do this in a partial fashion - the entire speaker doesn't switch to USB power once plugged in, just the digital logic. The reason for this is because the amplifier draws a varying amount of power - a big bass kick will cause a spike in the power going to the amplifier, and if we were powering the whole thing off of USB, the device giving us power might not like that. So instead, we've chosen to always run the boost converter/amplifier off the batteries, and the ESP32/DAC will automatically switch to USB power if it's available. This is done with a little FET that we've placed upstream of the 3.3V regulator:

![](failover.png)

Which pulls current from whichever source has a greater voltage (modulo the threshold voltage of the FET, but since 5V > 4.2 + 0.7V, we're good).

## Final Remarks
There's a lot of design here, and it took us a while to put it all together. We started by picking a few parts (like the ESP32, DAC, and amp) and throwing some modules we found on Amazon together. This worked well enough for us to want to put it on a PCB, so we made a first revision of the board and played with it. This uncovered a few bugs, which we fixed and rolled into version 2 - which you're getting!

# Resources We Used

- [A2DP Sink](https://github.com/pschatzmann/ESP32-A2DP)
- [Adafruit ESP32 Feather Tutorial](https://learn.adafruit.com/adafruit-esp32-feather-v2?view=all)
- [Adafruit ESP32 Feather Schematic](https://learn.adafruit.com/assets/41630)
- [Sparkfun ESP32 Thing Plus Schematic](https://cdn.sparkfun.com/assets/5/9/7/4/1/SparkFun_Thing_Plus_ESP32-WROOM_C_schematic2.pdf)
- [ESP32 Web Flashing](https://esphome.github.io/esp-web-tools/)
- [Binary Exports from PlatformIO](https://community.platformio.org/t/export-of-binary-firmware-files-for-esp32-download-tool/9253/2)

[^0]: Well, at least not anymore. The serial ports that we had on computers back in the day spoke a variant of UART called RS232, but we've since traded those in for more modern things like Type-C. 
[^1]: If the OSI stack means anything to you, Bluetooth is just the L1 physical layer that A2DP rides on top of. 
[^2]: If you've ever done any motor control, you'll notice that this is exactly the same as an H-Bridge for controlling a DC motor. That's not by accident - a speaker and a DC motor are fundamentally just coils that produce movement - in one case it's air, in the other it's a rotating output shaft.
[^3]: It can technically run from 4.5V as an absolute minimum, but it sounds downright _nasty_ that low. Raising it up to 12V made it sound much more reasonable, but going all the way up to 18V or so made the chip get super hot. I think this is the increased conduction losses from the higher voltage burning up the internal switches.

