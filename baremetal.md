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
Instead of searching for the best microcontroller ever, one must identify which features are important to each project.
Such features may include:
- General Purpose Input/Output (GPIO) pins
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

It is a good idea for starters to pick the most available microcontroller, due to their low-cost and lots of materials available online.
Such materials will minimize development time, with libraries ported and tested in the selected platform.
Only pick a different microcontroller when the specification demands, for example, hardware division must be available for performance reasons or energy consumption must be small to improve battery life.

For complex projects the memory size and amount of GPIOs must be carefully considered, as well as their placement around the IC.
Remember that some microcontrollers are 5V tolerant, which can save a few extra components.
The tools are usually free, their real cost is their learning curve, which may consume a long time for inexperienced users.
To avoid being tied to one IDE it is a good idea to start with a Makefile, which reveals the development stages instead of hiding the entire process behind a few buttons.

Here we will focus on the Arduino/AVR microcontrollers due to their low-cost, availability and vast library support.

## Makefile
To compile and flash a project outside an IDE one needs to execute separate tools in a specific order.
These tedious command sequences can be accomplished by a Makefile.
The Makefile can be used with ``make`` or ``make all`` to compile the project, ``make flash`` to program the microcontroller and ``make clean`` to remove generated files.
The following is my Makefile, based on [Florent Flament's post](http://www.florentflament.com/blog/arduino-hello-world-without-ide.html).

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

## Sketches
Common embedded projects usually follow the idea of [Wiring](https://en.wikipedia.org/wiki/Wiring_(development_platform)) sketches, a program with ``setup`` and ``loop`` functions.
During ``setup`` everything is initialized once, while the ``loop`` is repeatedly executed.
These two functions cover most basic programs, complex programs require their own functions and Interrupt Service Routines (ISRs).

```c
void setup(void)
{
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop(void)
{
  digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN)); // Toggle LED
  delay(1000); // Block program for 1000ms to see LED
}
```

The entry point of this program, a ``main`` function, can be ommitted.
Advanced users can add a custom ``main`` function to their program and avoid the unnecessary initialization and automatic event handling.
The hidden ``main`` function of a sketch looks like the following:

```c
int main(void)
{
  init(); // Configures internal peripherals, such as timer used by delay
  setup();
  while(true)
  {
    loop();
    if(event_available) execute_event(); // Execute external events
  }
}
```

## Pins
The most common usage of a microcontroller is to observe and control the state of pins.
The pins are exposed through [port registers](https://www.arduino.cc/en/Reference/PortManipulation):
- ``DDRx`` controls which pins are used to read or write, set by ``pinMode``
- ``PORTx`` stores the value of each pin, used by ``digitalRead`` and ``digitalWrite``

Arduino abstracts pin numbering with ``pinMode`` and ``digitalRead/Write``, but the problem lies in the implementation.
A look-up table is used to resolve pins at run-time, with a few extra security checks, which consumes processing time and memory.
Variations of such functions can identify port and pin at compile-time to obtain the same result faster.
This is specially useful with protocols that transmit a lot of data, such as the ones in displays, or responsive sensors for hazard applications.

<div class=split markdown=1>

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
<div class=split markdown=1>

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

Another detail about Arduino is that ``pinMode`` usually changes more than ``DDRx``:

   pinMode   | DDRx | PORTx
---          | ---  | ---
INPUT        | 0    | 0
INPUT_PULLUP | 0    | 1
OUTPUT       | 1    | unchanged

This behaviour may not be valid to certain boards, as different vendors may support Arduino functions in their hardware while supplying incompatible implementations of ``pinMode``.
Even if the implementation is equal there is a chance only the OUTPUT pins are set, as Arduino starts all pins as INPUTs, something to keep in mind while porting projects.

## Barrel shifter
Little microcontrollers may lack not only hardware division, but the useful [barrel shifter](https://en.wikipedia.org/wiki/Barrel_shifter).
Instead they can only shift by one, which means a loop to achieve ``(n << k)`` for ``k > 1``.
An idea to improve performance without using a better compiler is to avoid shifts altogether, using equivalent instructions.

To test the leftmost bit avoid ``if(n >> 7)``, use the equivalent ``if(n & 0x80)``.
Such single bit tests are actually available as instructions.
When in need of just the upper 4 bits it is possible to avoid the shift loop with the swap instruction.
The swap instruction returns ``(n << 4) | (n >> 4)`` for an ``n`` register, which means no loop, just a swap and a ``0xF`` mask.

Divide or multiply by 256 a 16 bit integer is free, as the result is already in upper byte.
For example, to avoid division by 10 one can multiply by 26 and divide by 256, which works for small integers and may be applicable to certain projects, as it was in [Aorist](https://github.com/Maumagnaguagno/Aorist).
This results in a single multiplication instruction, ignoring the least significant byte.
Remember to limit shifts to 1, 4 and 8 amounts to take advantage of the available instructions.