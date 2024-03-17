---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Lab 05 — Population and Assembly!
description: Puttin' it all together, quite literally.

# Micro navigation
micro_nav: true

# Page navigation
page_nav:
    prev:
        content: Schedule
        url: '../../schedule'
---

<div class="callout callout--danger">
There's a lot going on in this lab, and it's got a lot of moving parts in order to make it work. Read the whole thing before you start.
</div>

# Safetyin'

You'll be soldering a bit in this lab, this time with hot plate and solder paste instead of soldering irons and solder in wire-form. The regular rules still apply towards:

- __Hot things:__ The hot plate heat the boards up by being a hot plate which is a little more forgiving than a super hot soldering iron, but it'll still burn you. Keep an eye on where your hands are, and don't touch the metal surfaces. The wood may be a bit warm, so be sure to test the temperature before you stick your hand anywhere.
- __Sharp things:__ The tweezers are sharp, don't stab yourself with them. Be gentle with them too - it doesn't take a ton of force to keep a part in your tweezers, and you'll send it flying if you sqeeze too hard.
- __Tiny things:__  For the reason above. We'll keep our safety glasses on to keep any stray flying bits out of your eyes. Grab a set from us if you don't have normal glasses.

# Stencilin'

We've set up some stations around lab with a setup that looks about like this. Here's a video that shows you how to do the stenciling:

<video width="600" height="400" controls>
    <source src="https://pcb.mit.edu/static/videos/paste_demo.mp4" type="video/mp4">
</video>

This video is from last year. Our setup is a bit different, but the idea is still the same-- apply paste on your board. If you accidentally soldered stuff on the top 3 LED rows, let us know and we can remediate that. This lab won't work if you have stuff on the first 3 rows.

Feel free to have a quick look at your board under the microscope once you've got paste on. You can see the little solder balls under there, and how crisp your stenciling job was.

![](paste.jpg)

![](always_has_been.jpg)

# Populatin'

Once you've finished stenciling your board (yay!) we'll go ahead and start populating the board. Conveniently, ~~Altium's~~ KiCAD's got an assembly feature that helps us with this. Go ahead and open the staff reference design in KiCAD [here](https://github.com/PCB-Design-MIT/Wi-Fi-Controlled-RGB-Light).

Click on the "Interactive BOM" linked in our demo project Github. It should look like this

![](altium_assembly.png)

Here KiCAD's got all the parts that need to go on the board on the left. It'll tell you where they're supposed to go, and keep track of what you've placed on the board. Go ahead and click _Placed_ once you've got that part on board. It may or may not reset your progress if you refresh the page, so just be careful!

On paper, you just keep placing things until everything's on, but there's a little more nuance to this. We're going to practice what we preach with regards to component population. We suggest you populate the small stuff (0805 resistors and capacitors) first, since the big components might get in the way. We placed things on the table such that you can go front to back on one column, then move to the next column.

<div class="callout callout--danger">
Please keep everything in the bag! A pile of tapes carries no information about what parts are what, and it'll cause more issues down the road.
</div>

Before we jump in though, a few things to note:

- The 0805 Jumpers (R24-R27) should not be populated yet. We hid them in the back. Just like we talked about in [lecture 06](../lecture_06), the 0 $\Omega$ jumper allows us to disconnect the 36V boost circuit, meaning we can safely test our circuit one section at a time without frying the LEDs.

- __Programming Header (J2, J3)__ - don't populate it either! We're leaving it off as a backup for USB/UART bridge, and we're going to keep spreading good vibes by leaving it off.

- __Testpoints__ - no need to populate them yet! They're through-hole, so soldering these means you can't place your board on a hot plate.

<div class="callout callout--danger">
Be super careful to get the orientation right on your ICs, diodes, and LEDs as you put them in. The marking on the part should match the orientation marker on the part - usually it's a little dot or line. Feel free to ask us if you're unsure which way to put it in - preferably <em>before</em> you place it on the board and solder it in.
</div>

Go ahead and pop everything onto your board that is within reach. Towards the last 15-30 minutes of lab, we'll be asking if you need to go home at 12:00. If you do need to leave immediately, we're going to suggest that you reflow your board so the components don't come lose. The last thing we want is for you to lose progress.

# Reflowin'

<div class="callout callout--danger">
Board check: before moving on this step, have an LA check your board.
</div>

- Turn on the hot plate if it's not on already. We're going to set it to $270 ^ \circ C$ since that's what our paste wants in order to reflow. Wait for it to reach at least $260 ^ \circ C$. If we put it on too early, it could boil off the flux before the board is hot enough, and we'd be left with bad solder joints.
- Slap it on the hot plate
- The hot plate makes your board hot. Wow such hot
- Once it's been a minute or two you'll see the solder paste change color as the goo holding the parts in evaporates off, leaving the solder balls behind. This means the paste's heating up, keep going until you see it properly melt and reflow.
- Once you're done, keep the station on so others don't need to wait to heat it up. Pull the board off onto the piece of wood. We don't want to bump the parts off while the solder's still molten. Because that would be like, super sad. And we're in this for happy vibes 💖

If your electrolytic caps or inductors don't get all the way there, that's totally okay! We can hit those with a soldering iron afterwards :)

# Stashin'

You're probably not going to get through the whole thing in one go. Your boards are going to be safer chillin in lab for a few days then they would be floating around in your backpack, so we'll ask you to leave your boards in lab rather than take then with you. We'll show you where to stash them. Be sure to stick your name on your board before you stash it.

# Debuggin'

## Find Shorts and bring up power section

Once you've got your parts soldered onto the board, go ahead and make sure you don't have any shorts. Grab a multimeter (they're at your lab benches, grab leads from us if you need them) and make sure you don't have any shorts between:

- 5V and GND
- 36V and GND
- 5V and 36V
- Any 2 pins on the Boost IC. The KiCAD interactive BOM should show what traces connect to what if you click on a pad.

We forgot a 3.3V test pad, so use this diagram to see where you can probe 3.3V. The pin labeled 3.3V is a good place to probe.

![](3.3VTrace.png)

If you do, that's okay! Have a look around your board and look for shorts - feel free to use the microscope if you need to. Once you've found them, use some solder wick to clean 'em up and you're on your way. You might also need to ask us to help with repositioning a misaligned IC. That's okay! Some of these chips are the smallest stuff _we've_ soldered!

If you don't, nice work! Go ahead and test the 3.3V circuit like we described in lecture 06. You might need a helping hand-- LAs or your table buddy are great options :) We've pasted the instructions below for simplicity.

1. Check for shorts (done!)
2. Set your power supply's current limit to 0.3A and the voltage to 3.3V Please ask an LA to check this for you before proceeding.
3. Connect +5V and GND to the power supply while the supply is OFF. Remember, red goes to +5V.
4. Have a helper turn on the power supply and measure the 3.3V circuit. You should see something like 3V on the 3.3V rail.
5. Slowly increase the power supply voltage in 0.1V steps. If the 3.3V rail exceeds 3.4V, stop and ask for help. You shouldn't go above 5V on the power supply either.

Same process for the 36V Boost circuit.

1. Check for shorts (done!)
2. Your power supply should already be set at 5V/0.3A. But change it to this if you haven't already.
3. Connect the power supply's 5V to your board's test points while the power supply is off
4. Ask a friend to flip the power supply on
5. If the LED lights up, cool! You may also want to grab a multimeter to check the voltage on the +36V rail. Note-- it might be closer to 34V. If it's good, then great! The power section is brought up.

## Flash Firmware

We're going to ask that you check the CP2104 for solder defects first. This is U5, the chip on the "USB to Serial" section. If you see anything that looks like it's not properly soldered, fix it or ask us for help.

Once that's done, go ahead and grab a USB-C port from us. The port goes on the same side as the components, which means you should be looking at the PCB's back when you're soldering the port.

If you don't trust plugging your board into your computer, we have a burner laptop you can use. Winnie found it in the trash so it's no big deal if it gets fried.

Head to [our Google Drive](https://drive.google.com/drive/folders/1hhbp_we_4uPLRrP0yes8oWsGb9OF0U6O?usp=sharing) and download our firmware files.

If you know Arduino already, cool! The .ino file is in the folder and you can program your board in Arduino IDE as if it were a typical ESP32 Dev Board. If not, no worries. Head [here](https://espressif.github.io/esptool-js/) and connect to your device with the default speed. It should show up as a Serial port on your browser. If you're unsure of which COM port to choose, unplug your board. The correct port is the one that disappears when you unplug it.

If you see `Connecting*******______*******` for multiple seconds, then press and hold the boot button. The console should spit out some words about the ESP32 and its specs and stuff. You should now have the option to upload a file. Here's how you upload the .bin files:

![ESP32 Programmer](ESP32ToolProgrammer.png)

Be sure to use these exact flash addresses. Note that the last address has an extra 0, so it's 10,000 and not 1,000. DO NOT ADD THE COMMAS IN THE ADDRESS.

 `0x1000 Fade.ino.bootloader.bin`

 `0x8000 Fade.ino.partitions.bin`

 `0xe000 Fade.ino.boot_app0.bin`

 `0x10000 Fade.ino.bin`

click upload and... you're done! If you uploaded your code correctly, the test LED D3 should start blinking slowly. You should also see this on the console:

![](GoodProgram.png)

## Solder the 0 $\Omega$ Jumpers

You've soldered 0805s before. A few more can't hurt... This is R24-R27.

## Plug it in and test

When you plug it in, the white LEDs should start fading in and out. If you press the boot button, it switches the light into RGB mode. You should then see the RGB LEDs fade in and out.

## You're done! Celebrate in the glory of the LEDssss

# Appendix

## Code Walkthrough

### Define statements

Define statements tell the Arduino software to essentially replace every instance of the variable with the equivalent value before it sends the compiled code to the microcontroller. It's a great way to make variables that you might need to change later on without dedicating microcontroller resources to storing that variable.

### Variable declaration

We are going to declare that we want a variable called `mode`. `Mode` will be telling the microcontroller whether to display RGB color or white color. Mode == 0 means RGB mode, Mode == 1 means white mode. We're doing to tell Arduino that we want this to be an integer, notated as `int mode = 0;`.

### Pin setup

For our button, pressing it means the signal is pulled down with a low resistance. If the button is not pressed, the signal line is left floating. We're going to want a pull-up resistor so when the button is not pressed, the signal line is pulled up to be high instead of leaving it undefined. The ESP32 has built-in pull-ups for many GPIO pins, and all we have to do is call

`pinMode(BUTTON, INPUT_PULLUP);`

so the microcontroller connects the pull-up resistor. We also want this to be an input pin, since we're measuring data from it.

For all other pins, we're using it as an output so we can send data to another circuit. For that reason, we declare them as outputs. We also want their default state to be low to indicate an off LED, so we tell the microcontroller to set this pin low initially.

`pinMode(RED, OUTPUT);`

`digitalWrite(RED, LOW);`

### The main loop

The microcontroller then goes into a forever loop. This means everything in the loop will be run over and over until the end of time (or when it's unplugged). Let's have the LED changing colors as the thing being repeated over and over.

#### Checking modes

We have 2 modes-- RGB and white mode. The first thing to do is to check the mode state. Running

`if(mode){doThis();}`

means it will only run the thing after it if mode is high. We can also use inverted logic (a `!` ) to check for the opposite case.

`if(!mode){doThis();}`

#### The sine wave generator

Look, we copied it from [here](https://forum.arduino.cc/t/sinefade-extremely-simple-led-fade-using-a-pwm-pin-and-sine-function/600990). We added some more lines for R, G, B. But it's all the same principle. For the RGB mode, we want 3 outputs and some delay between the 3 sine waves so it looks like we are making multiple colors. We want to space them $\frac{360^\circ}{3} = 120^\circ$ apart. In our code, we added this offset to the colors, but in radian mode.

![](RGBLEDSineWave.png)

We also have fudge factors in the beginning, since the blue and red LEDs seem to be brighter than the green LEDs. They're common in industry... not that I know of any being used...

The code uses `analogWrite(pin, dutyCycleMax255)`. What this does is generate a 1kHz PWM signal on the pin with the specified duty cycle. A write of `255` results in a duty cycle of `100%`, and a write of `0` results in a duty cycle of `0%`.

#### Reading the button state

We can use `digitalRead(BUTTON)` to return `true (1)` or `false (0)` based on if BUTTON is pressed or not. Again, we're using a `!` to invert the logic to check for a pressed button.

With push buttons, they often "bounce". We'd like them to turn on and off in a clean signal, but the physical springs and such inside the switch quite physically bounce up and down after you press and un-press them. This means that if you don't filter the push button signal, it often looks messy. Here'a scope shot. The left peak is when I let go of the switch. It bounces back low though, and stays low for 8-10 us before remaining high. If your code is fast enough, it might think those two events are two button presses, and not one!  

![](debounce.png)

What we need to do is called "debouncing". As the name implies, switch debouncing gets rid of the extraneous data from a bouncing switch. The simplest way to do this is with a delay circuit. We can assume the bouncing stops after ~ 1 ms, so that's why there's a 20ms delay after the button state changes. Once the wait is over, `break` is called, which tells the microcontroller to leave the first loop (in this case, the `for` loop) it sees.

Repeat this for the other mode, and you have our code!
