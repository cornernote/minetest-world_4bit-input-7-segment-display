# Minetest World - 4bit-input 7-segment-display

Here is a machine I would like to share with those who do not know how a 4-bit decoder works, or have not yet seen one in minetest.

Winner of the [1st Mesecons Machine Competition!](https://forum.minetest.net/viewtopic.php?id=2810).


## Description


### What is it?

```
4-bit button input 
-into> binary coded decimal (using only 24 microcontrollers)
-into> 7-segment display encoder (using 112 microcontrollers)
-into> 7-segment piston display output
```

### What's it do?

The 4 buttons make an LED looking screen display a number from 0-9.


### Features

* Fits into 18 wide x 32 high x a tiny 28 deep including the glass housing
* 2-bit and 1-bit decoder output on both sides
* only 24 microcontrollers to make the 4bit decoder
* all gates are labeled with signs for a nice "feature tour"
* used the last few bits to encode "cnote" =)


### How does it work?

First the user presses a combination of buttons.  There are 16 combinations in total.  Each button relates to a number. +8, +4, +2, +1.  By pressing the buttons you can add to 15 (not 16, because 0 is a number too).

The 4bit input is fed into a 2bit decoder, which has 2 sets of 4 outputs (1st [set of 8] mcs after the buttons). There will always be two, and only two outputs on in this set (one red and one green).

The 2bit decoder output is fed into a 1bit decoder to make a binary coded decimal/hexadecimal which has 16 outputs (2nd [set of 16] mcs after the button).  You will notice each of these 16 outputs has a light.  There will always be one, and only one light on in this set.  The light represents the sum of the buttons.  The first light means it adds to 0, the second means it adds to 1, the third means it adds to 2, etc, to 15.

The binary coded decimal is fed into the display-encoder (the rows of 7 mcs behind the display).  Each mc will pass the bit through to the light on the other side to indicate it is connecting through all the mcs.  Each mc will also pass the bit from the next mc further away.  Depending on if the piston should be in or out the mc will pass the bit from the binary coded decimal output.

If it seems tricky to understand then download the map, load and load it up.  Have a look at the code in the diagrams below and the code in the mcs, and follow the logic along the tracks.


### What's next?

This is as far as I will take this build for now, however if you are keen to have a play...

The 4-bit inputs to could be linked to another machine that outputs a base-10 number with multiple digits (handles the carryover).

Once you have a few screens working together, they can be used as an output for a binary calculator.

You could also use it to track your score in a mini-game.


## Screenshots

![screenshot_299439193](https://cloud.githubusercontent.com/assets/51875/26443977/a40ce106-4179-11e7-9375-dcf93284d450.png)

![screenshot_298940729](https://cloud.githubusercontent.com/assets/51875/26443979/a4393df0-4179-11e7-882a-ea1333bf3fbd.png)

![screenshot_299265942](https://cloud.githubusercontent.com/assets/51875/26443984/a46fdb08-4179-11e7-92f7-bf04d784228c.png)

![screenshot_299203214](https://cloud.githubusercontent.com/assets/51875/26443987/a47509ca-4179-11e7-9f79-162e93fd9a13.png)



## Smaller Version

### 4-bit Decoder

![screenshot_352044499](https://cloud.githubusercontent.com/assets/51875/26443973/a4052dc6-4179-11e7-8e55-8ed98352b49d.png)

Mesecon Microcontroller Programming

* `A`=input from button
* `B`=input from larger bit
* `C`=output to larger digit
* `D`=output to current digit

** In addition to the table below, all have: `sbi(C,A)`

```
---------------------------------------------------------------------------
bit 4 (+8)    bit 3 (+4)    bit 2 (+2)    bit 1 (+1)     << indicator lights
---------------------------------------------------------------------------
sbi(D,A)    sbi(D,A&B)    sbi(D,A&B)    sbi(D,A&B)     |    F    |
sbi(D,A)    sbi(D,A&B)    sbi(D,A&B)    sbi(D,!A&B)    |    E    |
sbi(D,A)    sbi(D,A&B)    sbi(D,!A&B)   sbi(D,A&B)     |    D    |  O
sbi(D,A)    sbi(D,A&B)    sbi(D,!A&B)   sbi(D,!A&B)    |    C    |  U
sbi(D,A)    sbi(D,!A&B)   sbi(D,A&B)    sbi(D,A&B)     |    B    |  T
sbi(D,A)    sbi(D,!A&B)   sbi(D,A&B)    sbi(D,!A&B)    |    A    |  P
sbi(D,A)    sbi(D,!A&B)   sbi(D,!A&B)   sbi(D,A&B)     |    9    |  U
sbi(D,A)    sbi(D,!A&B)   sbi(D,!A&B)   sbi(D,!A&B)    |    8    |  T
sbi(D,A)    sbi(D,A&!B)   sbi(D,A&B)    sbi(D,A&B)     |    7    |
sbi(D,A)    sbi(D,A&!B)   sbi(D,A&B)    sbi(D,B&!A)    |    6    |
sbi(D,A)    sbi(D,A&!B)   sbi(D,B&!A)   sbi(D,A&B)     |    5    |
sbi(D,A)    sbi(D,A&!B)   sbi(D,B&!A)   sbi(D,B&!A)    |    4    |
sbi(D,A)    sbi(D,A|B)    sbi(D,A&!B)   sbi(D,A&B)     |    3    |
sbi(D,A)    sbi(D,A|B)    sbi(D,A&!B)   sbi(D,B&!A)    |    2    |
sbi(D,A)    sbi(D,A|B)    sbi(D,A|B)    sbi(D,A&!B)    |    1    |
sbi(D,A)    sbi(D,A|B)    sbi(D,A|B)    sbi(D,!A&!B)   |    0    |  << the row closest in the image
---------------------------------------------------------------------------
bit 4 (+8)    bit 3 (+4)    bit 2 (+2)    bit 1 (+1)     << input buttons
---------------------------------------------------------------------------
 ^^ the column closest in the image
 ```
 
### 7 Segment Display Decoder

Next I added the 7 segment display decoder on top.  I was able to get the connection to the upper level using only 2 nodes with inverters.  The whole 4-bit input to 7-segment driver is only 10x17x3, including the input buttons and indicator lights.

![screenshot_359116322](https://cloud.githubusercontent.com/assets/51875/26443976/a4086180-4179-11e7-96d5-e8220421d550.png)

The whole 7x16 grid of microcontrollers is programmed in one of two ways:

* POWER ON - (piston out) will hide on 7 segment display - `sbi(B,D)sbi(A,C|!D)`
* POWER OFF - (piston in) will display on 7 segment display - `sbi(B,D)sbi(A,C&D)`

