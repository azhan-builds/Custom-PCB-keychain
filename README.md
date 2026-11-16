# Custom PCB keychain

A small PCB keychain, custom designed using seven addressable RGB Leds placed in the shape of an initial('W' here). A button is used to change the animation, and shaking the keychain also changes it.

## What it does

- Uses seven addressable RGB Leds to animate the initial 'W'
- Custom animations on the keychain
- A button to change the animation manually
- Shaking or tiliting also changes the animation
- Cool hidden animations too
- A chargin animation, and the initial shows the battery level too
- Rechargable using usb-c

## Hardware

- ATtiny1614 microcontroller
- SK6812 Mini-e rgb leds x7
- Accelerometer, preferably LIS2DH12
- Li-ion puch cell
- MCP73831 single  cell li-ion charger IC
- MCP1700-3302 regulator 
- USB-c charging port
- tacticle button
- slide switch as power switch
- 100nF and 10uF capacitors for decouplling
- 330ohm resistor
- 402030 li-ion pouch cell.

## Status

hardware has been finalized so far, and the next step is the schematics. The choice of software will be Kicad.
