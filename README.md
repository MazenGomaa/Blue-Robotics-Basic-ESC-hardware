# Blue-Robotics-Basic-ESC
![](https://bluerobotics.com/wp-content/uploads/2017/11/BESC30-R3-2.jpg)

So i decided to reverse engineer the blue robotics' [Basic ESC](https://bluerobotics.com/store/thrusters/speed-controllers/besc30-r3/)
and also add more information about it here

So here we go:

## Software

The Basic ESC is using a firmware called BLHeli, you can find All information and source code about it here : https://github.com/bitdump/BLHeli , turns out, this firmware is actually quite very famous and is used for lots of other ESCs in the market, Which is nice since it's open source with all the support and all

In the mentioned repo you can actually find a PC and even an android app for Atmel ESCs, pretty cool

Our Basic ESC is using the SiLabs Version of this Firmware, since the main micro is a SiLabs Busy Bee part The EFM8BB21F16G-QFN20

## The EFM8BB21F16G-QFN20
### Where:
>The EFM8 is for: Silicon Labs EFM8 Product Line

>The BB2 is the: Busy Bee 2 Family

>The "1" is for: Family Feature Set (the 1s Fam)

>The "F": Memory type (Flash)

>The "16": Memory size - 16KB

>The "G": Temperature Grade (-40 to +85)

>And the QFN20 is for the physical component package type



### And here is a not so short snippet about this beefy tiny little bugger: 

### Core:

• Pipelined CIP-51 8-bit Core

• Fully compatible with standard 8051 instruction set

• 70% of instructions execute in 1-2 clock cycles

• 50 MHz maximum operating frequency

### Memory:

   • Up to 16 KB flash memory, in-system re-programmable
from firmware, including 1 KB of 64-byte sectors and 15
KB of 512-byte sectors.

   • Up to 2304 bytes RAM (including 256 bytes standard 8051
RAM and 2048 bytes on-chip XRAM)

### Power:

   • 3.6 Max V-input With maximum current consumption of 10mA


   • Power-on reset circuit and brownout detectors

### I/O: Up to 22 total multifunction I/O pins:

   • All pins 5 V tolerant under bias

   • Flexible peripheral crossbar for peripheral routing

   • 5 mA source, 12.5 mA sink allows direct drive of LEDs

### Clock Sources:

   • Internal 49 MHz oscillator with accuracy of ±1.5%
   
   • Internal 24.5 MHz oscillator with ±2% accuracy
   
   • Internal 80 kHz low-frequency oscillator
   
   • External CMOS clock option
   
   like bro WTF amirite?
   
### Timers/Counters and PWM:

   • 3-channel Programmable Counter Array (PCA) supporting
PWM, capture/compare, and frequency output modes, WOW OMG i bet this can be very very useful in something

   • 5 x 16-bit general-purpose timers
   
   • Independent watchdog timer, clocked from the low frequency oscillator
   
### Communications and Digital Peripherals:
> 16 Digital Port I/Os (in Total)
> 
   • 2 x UART, up to 3 Mbaud
   
   • SPI™ Master / Slave, up to 12 Mbps
   
   • SMBus™/I2C™ Master / Slave, up to 400 kbps
   
   • I2C High-Speed Slave, up to 3.4 Mbps
   
   • 16-bit CRC unit, supporting automatic CRC of flash at 256-
byte boundaries


### Analog:
   • 12-Bit Analog-to-Digital Converter (ADC)
   > 15 ADC0 Channels
   
   • 2 x Low-current analog comparators with adjustable reference (Yes, it has internally implemented OpAmps)
   > 10 Comparator 0 Inputs and 7 For Comparator 1
   
   • On-Chip, Non-Intrusive Debugging, Which imply that it dose not have Debuggy issues, nice
   
   • Full memory and register inspection
   
   • Four hardware breakpoints, single-stepping
   
   • Pre-loaded UART bootloader
   
   • Temperature range -40 to 85 ºC or -40 to 125 ºC
   
   • Automotive grade available (requires PPAP)
   
   • Single power supply of 2.2 to 3.6 V or 3.0
   
   • QFN28, QSOP24, and QFN20 packages
   

And of course you can find alot more about it here in the datasheet : [BB21F15](https://www.silabs.com/documents/public/data-sheets/efm8bb2-datasheet.pdf)

# Hardware
Since the website of the original hardware designer is no longer available, and i also happen to be a good stalker, did some searching and found their:

Facebook page at: https://www.facebook.com/FVTChina/ 

Email: jennyxcy2008@gmail.com

Some Websites about the company Favourite Electronics (Shenzhen) Co., Ltd:

https://www.made-in-china.com/showroom/jennyxcy2008

http://www.91103.tradebig.com/

https://www.listofcompaniesin.com/Favourite_Electronics_Shenzhen_Co_Ltd_Company_1881385.html

https://www.company-listing.org/favourite_electronics_shenzhen_co_ltd.html

Ah yes, A 1st grade Chinese product, what did you expect? apparently, blue robotics just relabeled it

# Behavior
while testing, some notable behavior was noticed:

- at very low speeds at about 1550uS the motor dose not spin constantly, instead it just starts and stops, this is happening due to the way this ESC is designed, since it's a Sensorless 


### The Schematic:

![Blue Robotics Basic ESC Schematic](https://github.com/MazenGomaa/Blue-Robotics-Basic-ESC-hardware/blob/main/Hardware/Schamatic/Schematic_Basic%20ESC_2023-04-21.png)

## This design:

It's straightforward and simple, code is on the main microcontroller chip, the micro controls the MOSFET driver IC while reciving a feedback signal from the phase sense resistor network

(You see, when you drive a bldc you need to know which magnetic pole of the rotor magnets is facing which one of the stator coils so you can always power the correct coil that's facing a magnet of opposite polarity and thus making the motor spin and all, and to do that, the circuit measures the Back EMF that's coming from the motor and calculates what coil will be energized next)


The MOSFET driver IC then drives the Gates of those big power MOSFETs, and the MOSFETs drives the bldc motor, the 9 10uf caps acts as power Buffers since once the motor starts it'll draw alot of current and having such high capacitance as close as possible to the MOSFETs is very very important (and in this design i really recommend adding a big 3300uf cap because the damn thing consumes alot current at the start of the motor so much so that the voltage drops below the supply voltage and then it shuts down it self sometimes)

- BLDC motors are brushless (lmao would you believe that?) which means that there's no brushes nor there's a commutator, and this circuit is basically that, A solid state BLDC commutator, this specific ESC utilities a trapezoidal motor control method which means that the output of our ESC is something that looks like this:
![trap_waveforms](https://www.bacancytechnology.com/blog/wp-content/uploads/2021/08/BLDC-motor-min.jpg)

- This is a very basic way to Drive a BLDC, and it is also the worst due to the issues that this squared waveform is gonna make, not to mention harmonics and inducative losses and such, but hey, it works, just not at low speeds.

- In order for to maintain a good precise feedback signal, a precious <1% resistors are used, which is nice.

- Putting two MOSFETs in parallel generates less heat and reduce overall power loss since both of them share the load current, thats a good one.

- In the FD6288 Motor driver datasheet, it is recommended to add a resistor for each gate of the power MOSFETs since at high frequencies, the gate capacitance will draw some current and that will cause the MOSFET driver IC to heat up, THIS IS A BAD ONE, because this is a one big reason why these ESCs die after sometime.



### The PCB:
![]()



