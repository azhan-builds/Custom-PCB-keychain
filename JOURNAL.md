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

**Total time spent: 1h 23min**

# November 17: added battery monitoring section and microcontroller has been integrated

Last night, i completed the power management section, and today i started working on the battery sensing section.

I want the microcontroller to monitor the battery level so i designed a simple voltage divider using 2 1Mohm resistors. The two resistors, R5 and R6 are connected in series between battery and GND. The junction between R5 and R6 will be connected to microcontroller's ADC input.
![voltage divider](https://cdn.hackclub.com/01a1e82e-b31c-7a85-9c22-6fd0d50b3a48/Screenshot%202026-11-17%20115526.png)

This voltage divider helps in dividing the bettery voltage into half.

Next, i added the microcontroller ATtiny1614-SS to the schematics. I will be using the microcontroller to control the leds, monitor battery level, button and a few other things(each will be discussed)
![ATtiny1614](https://cdn.hackclub.com/01a1e82e-0fbc-7705-ba43-4ec9373f355c/Screenshot%202026-11-17%20115348.png)

i went forward and connected the microcontroller to 3.3v output ofthe regulator. I also added 100nF capacitor between the power supply and gnd to stabalize the power supply.
![microcontroller power connection](https://cdn.hackclub.com/01a1e835-15f6-735e-b5fc-50ddc3096346/Screenshot%202026-11-17%20120226.png)

Now for the microcontroller's signal connection, i added two resistors 4.7k(R7) ohm and 330ohm(R8) respectively.
R7 is connected to PA0, and will be used for programming. R8 is connected to PA1, which will be carrying sgnal to the addressable led.
![resistors](https://cdn.hackclub.com/01a1e83a-454e-7472-8fda-43125839feb0/Screenshot%202026-11-17%20120805.png)

**total time spent: 45min**

# November 23: Added the accelerometer
After quite a bit of time, i finally got back to the project. Now i began by adding the accelerometer. I've decided to use LIS2DH12 which is a small 3 axis accelerometer that allow the keychain to undersant movement and changes in orientation.

I couldn't fint the exact symbol for this component in kicad's symbol library so I built a custom symbol
![LIS2DH12 custom symbol](https://cdn.hackclub.com/01a20805-8169-75c4-9d76-e4e11a6f656a/Screenshot%202026-11-23%20161738.png)

Next i needed the correct footprint, which i found easily on github. I downloaded it and assigned it to my symbol.
![footprint for accelerometer](https://cdn.hackclub.com/01a20807-8c7a-7d42-822f-8eac000905b4/Screenshot%202026-11-23%20162033.png)
I found it at: https://github.com/KiCad/kicad-footprints/blob/master/Package_LGA.pretty/LGA-12_2x2mm_P0.5mm.kicad_mod

With that, the accelerometer was in place and the next step was wiring. So let's get into it.

I started with the power connection, connecting VDD and VDD_IO to 3.3v supply and i also added two 100nF capcacitors for decoupling. The 3 ground pins and the RES pins were connected to GND.
![power connection](https://cdn.hackclub.com/01a20867-3672-7997-b89e-f7249c1a61fc/Screenshot%202026-11-23%20163534.png)

Next i made sure the sensor is configured to use I2C communication. For which I connected the CS(pin2) of the sensor pin to 3.3v so it selects I2C mode, while SA0 was connected to GNd in order to set its I2C address to 0x18.

I also added two 4.7k ohm pull up resistor for the communication line SDA and SCL. I went forward the connected SDA and SCL to PB1 and PB0 of the microcontroller respectivtly.

Then i connected INT1 pin of the sensor to the PA3 pin of the microcontroller. INT2 was left unconneccted for now
![screenshot](https://cdn.hackclub.com/01a208a7-7f15-706a-8694-21a910481cab/Screenshot%202026-11-23%20191510.png)

**Total time spent: 1hour**

# November 24: changes in led and power design
I moved on to the led part of the schematics. As mentioned earlier, the plan was to use 7 leds. But now i will be using 11 addressable leds.

I search the kicad's symbol library, and found the exact match for the leds that is SK6812MINI-E. I checked its pins and stuff and verified everything.
![led](https://cdn.hackclub.com/01a20c41-0fe5-723e-8e3a-74b5d21c7f63/Screenshot%202026-11-24%20120145.png)

I then placed 11 led symbol and started connecting them in a chain. The PA1 pin of the microcontroller will be used to control the leds, and that pin passes from the existing 330ohm resistor (R8) and enter the DIN pin of the first led. Each led's dout pin connects to the next led's Din pin. This way, all the leds could be controlled using a single data line.
![led connection](https://cdn.hackclub.com/01a20c45-12a0-79f0-93e9-4c37567ae66e/Screenshot%202026-11-24%20120606.png)

However, there is one problem. While going over the datasheet of these leds, i noticed that these leds require a voltage supply of 5.5v to 3.7v. Our li ion battery's voltage drops as it discharges. Supplying the voltage directly from the battery could potentially cause some problems.

After researching for a while, i decided on to use a 5V boost converter. The plan is to use a TPS61022 to step up the battery voltage to 5V. I also realized that we will need a logic level shifter between microcontroller and led. So i am currently moving on to that.

I added the boost converter, but kept the microcontroller and the accelerometer on 3.3v supply. I added SN74AHCT1G125 buffer to convert 3.3v data signal coming from microcontroller to 5v signal for the LEDs

I completed the led array, connected 5v to each led and gnd too. The data line passed from one led to another.
I also added 3 pin connector for programming the keychain. The pins are 3V3, GND, and UPDI 

I ran ERC, faced a few errors but fixed them swiftly so that's no issue. This completes the scheamatics of the keychain, and the next stop is the PCB design.

**Total time spent: 2hours 19mins**

# November 25: Footprints assigned

Today, i assigned footprints to each of the component. The resistors and capacitors were the easy part, cuz most of them used the same footprint. Resistors and capacitors got 0603 except a few. A few other problems and researching later, i finished the assinging. Now i will be moving on to the pcb part. 
footprints:
![footprints](https://cdn.hackclub.com/01a20f05-9cf4-7ad4-942c-5234c162609a/Screenshot%202026-11-25%20005540.png)
![footprints](https://cdn.hackclub.com/01a20f06-7802-7aa7-a2b9-b637d66afda1/Screenshot%202026-11-25%20005642.png)

**Total time spent: 45mins**

# November 27: PCB and routing

With the schematics being completed, i moved on towards the pcb. I ran update PCB from schematic option and everything came in one click.

First step was to decide the shape of the board. The keychain has to fit in the pocket, so i went with a round pcb. I tried various sizes, until i finally settled on 45mm diameter.

However, at 45mm, 11 Leds were too crowded, so i decided to reduce it down to 9. 
![W shape](https://cdn.hackclub.com/01a21963-bb23-70fb-bb56-a1382aa687e7/Screenshot%202026-11-27%20011426.png)

I decided to keep most of the components on the back, including the leds because the leds are reverse  mount, which meants they sit on the back and shine through small cutouts in the board. The front  part will have only the mode button and tiny capacitors next to each led.

I made three custom foot prints, which inlcuded:
- 3 round pads for programming the kechain. I did this to avoid soldering a header.
- 2 pads to solder the battery wires
- the gold W art for the front.

Then i went forward and placed the rest of the parts, beginning with the leds. And rest of the compoenents around them with the usb-c port being on the bottom edge.
![pcb](https://cdn.hackclub.com/01a21969-ed06-7e2f-af13-2f55f37e17c8/Screenshot%202026-11-27%20012116.png)

Then i went forward with routing, and this was literal nightmare.

I routed it in several batches like power, 3.3v, 5v rail for leds, signal rail, and more. 
![routing](https://cdn.hackclub.com/01a2196b-b87d-7a44-9c43-cc7da47b72f4/Screenshot%202026-11-27%20012331.png)

I changed a few things while routing, to make it easier:
- I swapped two pins on the microcontroller that is ATtiny. The led data line is now on PA4 while the battery sense is on PA5
- I also removed R4, which acted as a pull up in the STAT pin of charger. The ATtiny has internal pull ups so it's one less part.
- I had to tie the SA0 pin of the accelerometer to 3.3v instead of GND which changed the I2C address from 0x18 to 0x19.
- Moved a few small capacitors

Now for ground, i used GND pour on both front and back layer. A few issues arrived, so I fixed them on the hand and moved ahead.

After routing everything in batches, i finally completed it. I ran DRC, a lot of debugging and error handling later, i was able to bring them down. 

So the pcb is now complete, and we shall  move on to the firmware now.

But before the pcb, there is one more thing
Here is how I used AI fro the job
- Because the keychain was extremely cluttered, i used AI to give me coordinators so i can arrange everything proerly=
- I also had the AI check for clearence and various other issue.
- It suggested a few of the changes, and they were worth changing

Rest everything was done by a human

**Total time spent: 3hours 30mins**