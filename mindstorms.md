---
title: Mindstorms
description: Power to the Brick
date: 2018-10-05
category: lego
hidden: true
---

# Mindstorms
Based on constructionism from [Seymour Papert](https://en.wikipedia.org/wiki/Seymour_Papert), MIT Media lab and the Lego group collaborated to push Lego to the digital world.
Lego was losing market space against the virtual worlds from the video-game industry and educational robotics looked like a good fit for the Lego parts, letting kids explore the real world through robots built with their own hands.
Gears, motors and many parts were already available from Lego Technic kits, but sensors, a programmable brick and a programming environment were missing to achieve [a robotics kit for education](http://hackeducation.com/2015/04/10/mindstorms).
The kits were named as Lego Mindstorms due to the book [Mindstorms: Children, Computers, and Powerful Ideas](https://en.wikipedia.org/wiki/Mindstorms:_Children,_Computers,_and_Powerful_Ideas) by Seymour Papert.

|| RCX | NXT | EV3
--- | --- | --- | ---
**Release** | 1998 | 2006 | 2013
**Display** | segmented | 100x64 pixels | 178x128 pixels
**Processor** | H8/300 @16MHz | ARM7 @48MHz | ARM9 @300MHz
**RAM** | 32 KB | 64 KB | 64 MB
**ROM** | 16 KB | 256 KB Flash | 16 MB Flash + microSDHC
**USB** | No | No | Yes
**Bluetooth** | No | Yes | Yes

A new model, [51515](https://www.lego.com/en-us/aboutus/news/2020/june/lego-mindstorms-robot-inventor/), became available in 2020.
This page is updated as new sets are released and more information is found online.

## RCX
The RCX is the first fully programmable Lego brick came to the market in 1998, after a long relationship between MIT Media Lab and the Lego company and appeared in [many sets](https://www.bricklink.com/catalogList.asp?catType=S&catString=59.631).
The RCX could control 3 motors or lights, and 3 sensors simultaneously, which included: a touch sensor, a light/color sensor, a temperature sensor, and an angle sensor.
The RCX itself could emmit sounds and use its built-in infrared sensor to be programmed and communicate with other robots.
The version 1.0 included a [power adapter](https://pbrick.info/2013/10/using-an-adapter-to-power-the-rcx-1-0/) that was later removed, which forced robots to be powered by 6 AA batteries.
The RCX starts with [3 built-in programs](https://pbrick.info/2013/10/rcx-3-built-in-standard-programs/) that are stored for testing purposes.
User defined programs are lost if the RCX is left without power.
Kits included an infrared serial tower to let a computer communicate with the robot, later on replaced by USB towers.
Such towers can still be [configured to work with Linux](https://pbrick.info/2013/10/configuring-the-lego-usb-tower-on-linux/), more specifically the [Raspberry Pi](https://minordiscoveries.wordpress.com/2014/01/20/using-nqc-on-a-raspberry-pi-to-program-a-lego-mindstorms-rcx-brick/).
A [firmware](https://pbrick.info/rcx-firmware/) must be sent to the RCX before programming starts, many options are available with different levels of compatibility.
In order to not be limited to Lego provided programming options, mostly based on drag-and-drop code blocks, users can program their robots with [NQC](http://bricxcc.sourceforge.net/nqc/ "Not Quite C").
Due to the lack of inputs and outputs some users extended their robots capabilities with modified hardware, for example with a [multiplexer](https://www.elecbrick.com/mux/).
You can find out more about the RCX in the following places:
- [RCX presentation](http://clark.cementhorizon.com/RCX-web-page-2013-09-29.html)
- [The RCX, 19 years later](https://www.johnholbrook.us/edu/RCX_guide.html "written by John Holbrook")

### Scout, MicroScout and VLL
The RCX is a great introductory robotics kit, but due to cost and programming requirements (which require a computer not at hand for many kids) the RCX is simply too much.
A simpler solution came in the Scout, MicroScout and Spybotics sets.
The idea is simple: have less electronic parts (merge controller, sensors, motors, battery into a single element) and a few behaviors preprogrammed to explore.
The Scout ([9735](https://brickipedia.fandom.com/wiki/9735_Robotics_Discovery_Set)) is very similar to RCX, with less input and output ports (2 for each instead of 3) while using the same wires, motors and sensors from RCX, with a light sensor attached to the controller.
The MicroScout ([9748](https://brickipedia.fandom.com/wiki/9748_Droid_Developer_Kit) and [9754](https://brickipedia.fandom.com/wiki/9754_Dark_Side_Developer_Kit)) and Spybotics ([3806, 3807, 3808 and 3809](https://brickipedia.fandom.com/wiki/Spybotics)) took the other path: a single electronic element for everything.

Such sets are easier to setup and play with the multiple behaviors programmed, but the Lego group noticed that some users would like go beyond.
After the user explored the default programs within the system, it is time to explore the ``P`` program.
- [Micro Scout VLL Transparencies](https://www.elecbrick.com/vll/)
- [Teaching the RCX VLL](https://waterpigs.co.uk/articles/rcx-vll/)
- [Controlling a MicroScout from an RCX using VLL](https://pbrick.info/2013/10/microscout-rcx-vll/)
- [R2D2](https://www.geocities.ws/laosoh/robots/r2d2new/R2D2.htm)
- [Adafruit Bluetooth Remote Control for the Lego Droid Developer Kit](https://learn.adafruit.com/bluetooth-remote-for-lego-droid/)
- [Swift and VLL](https://adriansieber.com/control-lego-via-swift-on-ipados/)

## NXT
The next generation of Mindstorms came in two versions, the first in July 2006 and the second one in August 2009.
Previous sensors and motors are no longer compatible with RCX as new connections have been introduced.

## EV3
The evolution of Mindstorms created the third release of Lego robots in September 2013.
Sensors and motors are retro-compatible with NXT ones due to no modifications to the cables.

## Extra bits
TODO
- Code Pilot ([8479](https://brickipedia.fandom.com/wiki/8479_Barcode_Multi-Set))

### Motors
TODO

### Power Functions
In 2007 Lego introduced a new connector for motors and sensors, resembling the one from the RCX era with [studs](https://en.wikipedia.org/wiki/Lego_Technic#%22Studded%22_(Beams)_versus_%22Studless%22_(Liftarms)).
Such system is seen as a simpler way to add motorized/remote control features to model that are otherwise manually operated.
Some recent sets have a dedicated space to add Power Functions.
A list of Power Function sets is available [here](https://en.wikipedia.org/wiki/Lego_Technic#Power_Functions).

### WeDo / Boost
Features smartphone app programming and is available through the sets:
- Creative toolbox ([17101](https://brickipedia.fandom.com/wiki/17101_Boost_Creative_Toolbox)) - 2017
- Droid commander ([75253](https://brickipedia.fandom.com/wiki/75253_Droid_Commander)) - 2019

### Prime
TODO