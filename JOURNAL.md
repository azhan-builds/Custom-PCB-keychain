---
title: "W shaped keychain"
author: "azhan.builds"
description: "A custom pcb keychain designed to use 7 leds to illuminate the initial W"
created_at: "2026-11-15"
---

# November 17: finalised the hardware and started the schematics

## POWER circuit schematics;
Today, i started by finalising the hardware for the custom keychain and began with the schematics.

The idea is to create a small pcb keychain using seven addressable RGB leds that will arranged in the shape of the letter 'W'. It will illuminate and have different animations, manual control and accelerometer as well.

The finalised hardware was:
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

I decided to begin with the power circuit first, as it is the most important part.
The first component was a usb-c receptacle:
![usb-c receptacle](https://cdn.hackclub.com/01a1e5c5-dca1-775b-b1e6-25bc2d2caa30/Screenshot%202026-11-17%20004128.png)

i selected the 'USB_C_Receptavle_PowerOnly_6P symbol because i need usb only for charging.
and i decided to go with compoenent to component wiring instead of net labels, because i think that is more satisfying.

usb-c required pull down resistors so the device can be identified as power consumer. So i added two 5.1k ohm resistor. Where in R1 si connected between CC1 and GND, and R2 is connected between CC2 and GND.

![r1 and r2 connection](https://cdn.hackclub.com/01a1e5ca-0a39-7586-a31d-b48336d9d11e/Screenshot%202026-11-17%20004617.png)

next i added the MCP73831-2-OT single cell charging controller, and also added two 10kohm resistors and two 4.7uf capacitors.

![complete power circuit](https://cdn.hackclub.com/01a1e5cc-ec6b-773b-bec4-c0e10ee5c75e/Screenshot%202026-11-17%20004925.png)

R3 is connected to Prog pin of the chargin controller while the R4 is connected to the charger's STAT pin and acts as a pull up resistor. 

C1 is connected between VDD of charger and GND, while C2 is connected between VBAt and GND.

This completes the power circuit.

## added battery management and voltage regulation

after completing the usb-c battery charging circuit, i moved on to the battery management part.
I've decided that the keycahin will use a 402030 500 mah li ion pouch cell.

I began by adding two pin connector so i can connect battery through jst ph connection.
![two pin connector](https://cdn.hackclub.com/01a1e5e5-64cb-7f54-a83c-21baf1002cf8/Screenshot%202026-11-17%20011605.png)

pin 1, which is the positive pin is connected to the vbat charger ouput and pin 2, which the negative pin is connected to GND
![battery connected to charger](https://cdn.hackclub.com/01a1e5e7-1acc-7f4c-af1a-dd590cde2fd5/Screenshot%202026-11-17%20011745.png)

i also added an spdt slide switch to control the power supply manually.

Now we need to add a 3.3v regulator, so i decided to use MCP1700 regulator to provide stable voltage to accelerometer and microcontroller. I also added two 1uF capacitors accross it's VIn and Vout pins

here are the connections so far:
![schematics](https://cdn.hackclub.com/01a1e5e9-c6a2-7632-a689-4176393a33bf/Screenshot%202026-11-17%20012056.png)