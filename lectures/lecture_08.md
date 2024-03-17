---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Lecture 08 - Microcontrollers 101
description: How we use microcontrollers to debug and test PCBs

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
        content: Lecture 07
        url: '../lecture_07'
    next:
        content: Lecture 09
        url: '../lecture_09'



---

# Assumptions

In this lecture, we're assuming that people have seen some sort of code before-- whether that's scratch / blocks programming, python, C, Java, or literally anything else. We won't cover what variables or functions are-- rather we're going to start from the assumption that you understand how the general structure of code works, and that we're giving you a Arduino "C" specific tutorial on how to use microcontrollers for debugging and testing PCBs.

If you already know how to do stuff like Digital and Analog Reads and Writes, as well as Serial Monitor and libraries, feel free to skim the slides and skip the lecture! We have this lecture to ensure that everyone starts next week on the same page.

# Installing Arduino IDE

Before lecture, please install [Arduino IDE](https://www.arduino.cc/en/software). You'll also need to install the [ESP32 specific libraries](https://docs.espressif.com/projects/arduino-esp32/en/latest/installing.html). Once you get that set up, you should be able to go to Tools -> Board and see "ESP32". Please also come to lecture with your demo PCB and our USB-C cable to plug it into your computer! We'll be doing a follow-along lecture.

![](LibraryInstalled.png)

# Variables and Arduino "C" formatting

## Define statements

Define statements tell the Arduino software to essentially replace every instance of the variable with the equivalent value before it sends the compiled code to the microcontroller. It's a great way to make variables that do not need to change during the microcontroller's run time without dedicating microcontroller resources to storing that variable. We're going to use it to declare what pins on the microcontroller connect to what. You can copy this over since we're using the same board.  

`#define RED 22`

`#define GREEN 21`

`#define BLUE 19`

`#define WHITE 17`

`#define DEBUG 12`

`#define BUTTON 0`

## Variable declaration

Variables are a little different than define statements. Variables are stored on the microcontroller, rather than replaced by Arduino. They're great for anything that may change. We are going to declare that we want a variable called `mode`. `Mode` will be telling the microcontroller whether to display RGB color or white color. Mode == 0 means RGB mode, Mode == 1 means white mode. We're doing to tell Arduino that we want this to be an integer, notated as `int mode = 0;`.

### Variable types

- `int` - A signed (negative or positive) integer with a maximum value of $2^{15}$ and a minimum value of $1 - 2^{15}$
- `long` is like an `int` but with a larger maximum value of $2^{32}$ and a minimum value of $1 - 2^{32}$
- `uint8_t` - An unsigned (no positive or negative) integer with a length of 8 bits. Used often in digital protocols
- `uint16_t`An unsigned (no positive or negative) integer with a length of 16 bits. Used often in digital protocols
- `float` and `double` - Used for signed numbers that have a decimal value
- `bool` - Used for boolean (true/false) variables.

## Semicolons

This is annoying. But every line of code that isn't a function or loop needs to end in a semicolon. Formatting...

# Serial Monitor - We have text

Let's do a "hello world" - Microcontroller style! We want to get data from the microcontroller, and we can have it send data back to the computer in a serial monitor. In Arduino, create a new sketch (File -> New Sketch). You'll see `void setup(){}` and `void loop(){}`. As the comments suggest, setup is run first and is run once. After setup is done running, loop is run in a loop forever until the end of time.

For setting up the Serial communication, we need to call `Serial.begin(9600);` in `void setup(){}`. What this does is it establishes a UART connection with the USB to Serial IC. The 9600 means it's sending data at a rate of 9600 bits / second. We need to make sure to remember this, or things will look like gibberish.

To send data to the computer, we can use `Serial.println("Hello world!");`. If you want it to be done once, it can go in setup. Otherwise, put it on loop.

That's it! Upload to the right port (Should be CP2104 USB to Serial device on Mac, or COM__ on Windows. The port should disappear if you disconnect it from your computer). Then, go to Tools -> Serial monitor, and a window will pop up at the bottom. Be sure to check the "baud" at the right side and make it 9600. You should see "hello world!". You might notice that it's a bit fast, so you can add a `delay(1)` so it's a bit slower to send.

![](SerialMonitor.png)

But come on, you didn't take a PCB class to write words to see the correct words on a screen, did you? Our class number is 2.S980, not 6.009[6.1010]. So let's make some physical things happen!

# Initialization

By default, your microcontroller doesn't know what to do with its pins. You need to tell it to perform the desired function! Recall this I/O pin equivalent from [lecture 5](../lecture_05). There's many functions to potentially enable! Let's go through each one of them and see what we can do with them.

![](../lecture_05/PinEquivalent.png)

Image credit: [ST](https://www.st.com/resource/en/reference_manual/rm0360-stm32f030x4x6x8xc-and-stm32f070x6xb-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)

# Digital Read - Inputs

Let's read whether I/O pin 2 is high or low. To do this, we tell the ESP32 that we're going to use this pin as an input pin. In setup, insert `pinMode(2, INPUT);`.

Now, we can read pin 13! In the loop, we can run `Serial.println(digitalRead(2));` to print what the ESP32 is reading on the pin.

Pin 2 is here, the rightmost pin on the bottom row of pins. Take a 100K resistor and touch one of the resistor to the pin. You'll want to hold the metal lead. What you'll see is quite strange-- it reads 1 sometimes, and 0 sometimes! You can also go to Tools -> Serial Plotter and see the same thing. Why is this? Nothing in the room is changing. We're staying still, and the microcontroller is getting DC power. This doesn't seem that reliable if we're trying to to measure something! We don't want a person touching our circuit to change how it behaves.

![](InputNoPullup.png)

# Analog Read - More inputs

To investigate, let's instead do an Analog read. This will give us a voltage reading from 0V to 3.3V instead of high and low. To do this, replace the println in the loop with `Serial.println(analogRead(2));`. Let's upload and see what it looks like!

![](AnalogInputNoPullup.png)

_Huh_. What's that sine wave with a 60 Hz frequency? Hmm. Could it be... _a sign that MIT is putting some weird electromagnetic force field in the air? I knew they were doing something weird with those 5G towers._ Believe it or not, that could genuinely be a reason why! We'll talk about this more on Monday, but the noise is in fact coming from _the air_. You're looking at the 60 Hz wall power getting transferred from the air into your board! Since the input pin has no path for current, the energy is converted into voltage waveforms.

What we can do instead is provide a path for that energy to go using a pull-down resistor. This also means a certain amount of current must flow through the resistor in order to change the input voltage. So let's try poking at GPIO pin 2 again, but this time, press the other side of the metal lead into the ESP32 shield (which is grounded). Be sure to hold the metal side that's touching the GPIO pin. And look at that, there's no more noise!

![](PullDown.jpg)

In the microcontroller, they also have an internal pull-down resistor, as shown in the output configuration diagram. To configure this, we can run `pinMode(INPUT_PULLDOWN);`. Unfortunately pull-downs only work if we're doing a DigitalRead, and not a AnalogRead. This is why we did it with an external resistor. Nevertheless, let's try doing a `digitalRead()` while declaring that pin 2 is an `INPUT_PULLDOWN`. We're going to repeat the experiment before where we are touching the GPIO 2 pin. And look at that, the blips of 1s and 0s are gone!

![](../lecture_05/PinEquivalent.png)

Image credit: [ST](https://www.st.com/resource/en/reference_manual/rm0360-stm32f030x4x6x8xc-and-stm32f070x6xb-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)

Let's use the boot button to do stuff! The button is on I/O pin 0, and we're going to use the button to change the state of a variable. Before we change the variable, we need to declare the variable. We do that at the very top to ensure that the variable can be seen by all functions, not just in setup or loop. Let's add `int mode = 0` so we can have a variable named mode that starts out as being 0. In the loop, let's add this code:

```
if(!digitalRead(BUTTON)){ //if button pressed
    mode = !mode; //change the mode
    while(!digitalRead(BUTTON)){} //wait until un-pressed
}         
```

We can also print `mode` so we know what's going on!

# Digital Write - LED Blinking

Let's propose: if `mode` is 1, then we turn an LED on. Otherwise, the LED should be off. Our test LED is on GPIO12, so let's use that to output a voltage. To use the output capabilities of the microcontroller, we have to declare `pinMode(12, OUTPUT);` so the microcontroller can use its output drivers to change the output state. Then, in the loop, we can do `digitalWrite(12, mode);`. This will turn the LED on if `mode` is nonzero, and will turn the LED off if `mode` is zero.  

# Analog Write - LED fading

Let's say we want something that's halfway between on and off-- a dimmer let's say. Well, just like we talked about in [lecture 05](../lectures/lecture_05), we can use PWM to achieve dimming! The way we call this on the ESP32 is by using the `analogWrite();` function. Let's introduce a different variable, named `brightness`. We're going to declare it the same way as we did with `mode`. Now, let's insert this code in the loop:

```
if(mode){
    brightness = brightness + 1;
}
else{
    brightness = brightness - 1;
}
```

We also need to use `constrain`, since `analogWrite` only accepts values from 0 to 255. Let's add in `brightness = constrain(brightness, 0, 255)`.

And if we upload the code, we should get an LED that fades on and off!

# Libraries - Let's get the time from the Internet

<https://lastminuteengineers.com/esp32-ntp-server-date-time-tutorial/>
