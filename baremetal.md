---
title: Bare Metal
description: Fast operations on microcontrollers
date: 2018-05-09
category: microcontroller
---

# Bare Metal
A common practice in software engineering is to avoid repetition and unnecessary complexity by encapsulating instructions into routines.
Collections of such routines become the libraries that make certain programming languages better suited to tackle specific problems.
A well know environment with such libraries for microcontroller programming is the Arduino platform, which abstracts away most of the initial and time-consuming setup step.

However, such easy-to-use approach may cost a lot in the long run, with slow or undocumented routines and changes from such libraries breaking projects, leaving novice users without a clue.
Without the need for a complete understanding of what is happening under the hood more people can play with electronics, but few can actually build a long-term project out of it.
In the following sections we explore the old blocks of microcontroller programming that are sometimes ignored by tutorials around the web.

## Selecting a microcontroller
Instead of searching for the best microcontroller ever one must identify which features are important in the project.
Such features may include:
- General Purpose Input/Output pins
  - Amount of digital and analog pins
  - Voltage and tolerance
- Energy consumption
- Physical size
- Protocols
- Instruction set
- Memory size
- Speed
- Cost
- Availability
- Tools to write, compile, flash and debug programs
- Learning curve, tutorials, libraries and examples

It is a good idea for starters to pick the most available microcontroller, as their cost is usually low with lots of materials available online.
Such materials will minimize your development time, as no library must be ported and tested in a new platform.
Only pick a different microcontroller when your specification demands, for example, hardware division must be available for performance reasons or energy consumption must be small to work with batteries.

For complex projects the memory size and amount of GPIOs must be carefully considered, as well as their placement around the IC.
Remember that some microcontrollers are 5V tolerant, which can save you a few extra components.
The tools are usually free, their real cost is the learning curve that may consume a long time for inexperienced users.
To avoid being tied to one IDE it is a good idea to start with a Makefile, which reveals the stages instead of hiding the entire process behind a few buttons.

## Makefile
To compile and flash your project outside an IDE you need to execute separate tools in a specific order.
Instead of repeating this process every day a makefile can do this job for you.
The makefile can be used with ``make`` or ``make all`` to compile the project, ``make flash`` to program the microcontroller and ``make clean`` to remove generated files.
The following is my Makefile, based on [Florent Flament's post](https://www.florentflament.com/blog/arduino-hello-world-without-ide.html).

```makefile
TARGET = main
BAUD = 57600
AVR  = atmega328p
TYPE = arduino
FREQ = 16000000
PROGRAMMER = /dev/ttyUSB0

CFLAGS  = -DF_CPU=$(FREQ) -mmcu=$(AVR) -Wall -Werror -Wextra -Os
OBJECTS = $(patsubst %.c,%.o,$(wildcard *.c))

.PHONY: flash clean

all: $(TARGET).hex

%.o: %.c
	avr-gcc -c $< -o $@ $(CFLAGS)

$(TARGET).elf: $(OBJECTS)
	avr-gcc -o $@ $^ $(CFLAGS)

$(TARGET).hex: $(TARGET).elf
	avr-objcopy -j .text -j .data -O ihex $^ $@

flash: $(TARGET).hex
	avrdude -p $(AVR) -c $(TYPE) -P $(PROGRAMMER) -b $(BAUD) -v -U flash:w:$<

clean:
	rm -f $(TARGET).hex $(TARGET).elf $(OBJECTS)
```

## Pins
The most common usage of a microcontroller is to observe and control the state of pins.
The pins are exposed through [port registers](https://www.arduino.cc/en/Reference/PortManipulation):
- ``DDRx`` controls which pins are used to read or write, set by ``pinMode``
- ``PORTx`` stores the value of each pin, used by ``digitalRead`` and ``digitalWrite``

Arduino abstracts port and pin numbering with ``pinMode`` and ``digitalRead/Write``, but the problem lies in the implementation.
To solve pin and port a look-up table is used at run-time, with a few extra security checks, which consumes processing time and memory.
Variations of such functions were developed to identify port and pin at compile-time to obtain the same result faster with the same board.
This is specially useful when working with protocols that require a lot of data being sent or received through pins, such as a display, or a responsive sensor for hazard applications.

<div class="split" markdown="1">

### Arduino
```c
void setup(void)
{
  pinMode(D2, INPUT);
  pinMode(D3, OUTPUT);
}

void loop(void)
{
  int v = digitalRead(D2);
  digitalWrite(D3, HIGH);
  digitalWrite(D3, LOW);
}
```

</div>
<div class="split" markdown="1">

### C
```c
void setup(void)
{
  DDRD &= ~(1 << PD2);
  DDRD |= 1 << PD3;
}

void loop(void)
{
  int v = (PIND >> PD2) & 1;
  PORTD |= 1 << PD3;
  PORTD &= ~(1 << PD3);
}
```

</div>