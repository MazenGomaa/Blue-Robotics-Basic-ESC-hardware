# Blue-Robotics-Basic-ESC-hardware

So i took some time and you know, braid, and decided to reverse engineer the blue robotics' [Basic ESC](https://bluerobotics.com/store/thrusters/speed-controllers/besc30-r3/)
And decided to put all information about it here

So here we go:

## The Software

The Basic ESC is using a firmware called BLHeli, you can find any information about it here : https://github.com/bitdump/BLHeli , turns out, this firmware is actually quite very famous and is used for lots of other ESCs in the market, Which is nice since it's open source with all the support and all
In the mentioned repo you can actually find a PC and even an android app for Atmel ESCs, pretty cool

The Basic ESC is using the SiLabs Version of this software, since the main micro is a SiLabs Busy Bee part 

### The EFM8BB21F16G-QFN20 Where:
> The EFM8 is for: Silicon Labs EFM8 Product Line
 The BB2 is the: Busy Bee 2 Family
> The "1" is for: Family Feature Set (the 1s Fam)
> The "F": Memory type (Flash)
> The "16": Memory size - 16KB
> The "G": Temperature Grade (-40 to +85)
And the QFN20 is for the physical component package type (I-I-It's a hardware thing, ya won'tget it)
And of course you can find alot more about it Here : [BB21F15](https://www.silabs.com/documents/public/data-sheets/efm8bb2-datasheet.pdf)


