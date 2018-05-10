---
title: Baremetal
description: Fast operations on microcontrollers
date: 2018-05-09
category: microcontroller
hidden: true
---

# Bare Metal
A common practice in software engineering is to avoid repetition and unnecessary complexity by encapsulating instructions into routines.
Collections of such routines become the libraries that make certain programming languages better suited to tackle specific problems.
For microcontroller programming a well know environment with such libraries is the Arduino platform, which abstracts away most of the initial and time-consuming setup step.
However, such easy-to-use approach costs a lot on the long run, with routines that are slow and undocumented, leaving novice users without a clue about how they operate without reading the code.
Without a complete understanding of what is happening under the hood more people can play with electronics, but few can actually build a long-term project out of it.
In the following sections we explore the old blocks of microcontroller programming that are sometimes ignored by the tutorials around the web.

## Selecting a microcontroller
Instead of searching for a single better microcontroller one must select which features are important first.
Such features may include:
- Amount of General Purpose Input/Output pins
- Amount of analog and digital pins
- Voltage and tolerance
- Physical size
- Protocols
- Instruction set
- Memory size
- Speed

## Makefile
To compile you project outside the IDE you need to run certain tools in a specific order, depending on which files you modified to avoid running unnecessary commands.
Instead of repeating this process every day a makefile can do this job for you.

```makefile
TODO
```

## Pins
The most common usage of a microcontroller is to control the state of pins.
The pins are exposed through [port registers](https://www.arduino.cc/en/Reference/PortManipulation).
Arduino exposes read/write control with ``pinMode`` and state control with ``digitalWrite`` functions.
This is a very nice approach to abstract the port number and focus only on the pin name, the problem lies in the implementation.
To achieve the same effect Arduino uses look-up tables to identify port and pin with a few extra security checks, which consumes processing time and memory as they are being solved at run-time.
Variations of such functions were developed to identify port and pin at compile-time and obtain the same result faster without requiring an overclocked Arduino board.
This is specially useful when working with external device protocols that require a lot of data being sent or received through pins, such as a display or a responsive sensor for hazard applications.

<div class="split" markdown="1">

### Arduino
```c
pinMode(D2, OUTPUT);
digitalWrite(D2, HIGH);
digitalWrite(D2, LOW);
```

</div>
<div class="split" markdown="1">

### C
```c
DDRD  |= 1 << PB2;
PORTD |= 1 << PB2;
PORTD &= ~(1 << PB2);
```

</div>