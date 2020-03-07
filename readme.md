#Pin mappings
The Arduino Pro Micro has an ATMega34u chip on it. The breakout on the board doesn't match up with the internal naming of the pins, so I've made a reference of what I used here. Used a combination of this chip reference https://www.arduino.cc/en/Hacking/PinMapping32u4 and this board reference https://cdn.sparkfun.com/assets/9/c/3/c/4/523a1765757b7f5c6e8b4567.png. Specifically, the pins are organized into groups called ports, generally split by functionality. Analogue pin 3 (A3 on the sparkfun board reference) is read in the code as Port F pin 4 (PF4 in the arduino reference). 

RHS:
Rows:
TX1 - D3
4 - D4
5 - C6
6 - D7
7 - E6
8 - B4

Cols:
A3 - F4
A2 - F5
A1 - F6
A0 - F7
15 - B1
14 - B3
16 - B2
10 - B6

I organized my board as COL2ROW. As long as all the diodes are pointing the same direction, it won't matter which format you use, swapping COL2ROW/ROW2COL is simple.

There are 3 steps to mapping the board. FIrst you have to create the keyboard layout. This is in the top level directory lookingglass/ as `lookingglass.h`. Inside there's a `#define` which creates the layout. The macro takes two parts, the first is a single dimension array which I think just declares the key variables. The second part organizes them with {} into rows and columns. It's good to note that the first section *SHOULD NOT* include vars for empty spaces. If your matrix is not perfectly rectangular there will be gaps in some of the rows, and this is fine. Just name your variables something descriptive like `K<row><col> (K41)` and skip the ones which aren't in your matrix.
The second part will need to include those spaces as `KC_NO` so that the board knows to not expect inputs from those spaces.
