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
BAUD = 57600
AVR  = atmega328p
TYPE = arduino
FREQ = 16000000
PROGRAMMER = /dev/ttyUSB0

CFLAGS  = -DF_CPU=$(FREQ) -mmcu=$(AVR) -Wall -Werror -Wextra -Os
OBJECTS = $(patsubst %.c,%.o,$(wildcard *.c))

.PHONY: flash clean

all: main.hex

%.o: %.c
	avr-gcc $(CFLAGS) -c $< -o $@

main.elf: $(OBJECTS)
	avr-gcc $(CFLAGS) -o $@ $^

main.hex: main.elf
	avr-objcopy -j .text -j .data -O ihex $^ $@

flash: main.hex
	avrdude -p $(AVR) -c $(TYPE) -P $(PROGRAMMER) -b $(BAUD) -v -U flash:w:$<

clean:
	rm -f main.hex main.elf $(OBJECTS)
```

## Pins
The most common usage of a microcontroller is to observe and control the state of pins.
The pins are exposed through [port registers](https://www.arduino.cc/en/Reference/PortManipulation):
- ``DDRx`` stores which pins are used to read or write operations, as ``0`` or ``1`` respectively, set by ``pinMode``
- ``PORTx`` obtains or sets the value of each pin, used by ``digitalRead`` and ``digitalWrite``

Arduino abstracts port and pin numbering with ``pinMode`` and ``digitalRead/Write``, but the problem lies in the implementation.
To solve which pin is what a look-up tables is used to identify port and pin with a few extra security checks, which consumes processing time and memory as they are being solved at run-time.
Variations of such functions were developed to identify port and pin at compile-time and obtain the same result faster without requiring an overclocked Arduino board.
This is specially useful when working with external device protocols that require a lot of data being sent or received through pins, such as a display or a responsive sensor for hazard applications.

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
  int value = digitalRead(D2);
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
  DDRD  &= ~(1 << PD2);
  DDRD  |= 1 << PD3;
}

void loop(void)
{
  int val = (PIND >> PD2) & 1;
  PORTD |= 1 << PD3;
  PORTD &= ~(1 << PD3);
}
```

</div>