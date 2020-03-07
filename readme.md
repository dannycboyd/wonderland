# Nomenclature
In qmk, the two first-class citizens are the `keyboard` and the `keymap`.
## Keyboard
The keyboard is the set of `rules.mk, config.h, keyboard.c, keyboard.h`, where usually instead of `keyboard` it's the keyboard's name. It contains the `LAYOUT` definition of the keyboard, as well as the row/col size, whether the keyboard is row or column driven, and other implementation for the hardware.

The `LAYOUT` macro has two parts, the first is a single dimension array which I think just declares the key variables. The second part organizes them with {} into rows and columns. It's good to note that the first section *SHOULD NOT* include vars for empty spaces. If your matrix is not perfectly rectangular there will be gaps in some of the rows, and this is fine. Just name your variables something descriptive like `K<row><col> (K41)` and skip the ones which aren't in your matrix.
The second part will need to include those spaces as `KC_NO` so that the board knows to not expect inputs from those spaces.

For example. A keyboard with two keys on the top row and 3 on the bottom row could be created like so:
```
#define LAYOUT_3x3( \
  k00,      k02, \
  k10, k11, k12 \
) \
{ \
  { k00, k02 }, \
  { k10, k11, k12 } \
}
```

## Keymap
The keymap lives in a folder next to the keyboard. The mapping's name is the foldername, and the files inside are `config.h` and `keymap.c`. The map contains the keycode assignment for the switches in the `LAYOUT`.

# Pin mappings
The Arduino Pro Micro has an ATMega34u chip on it. The breakout on the board doesn't match up with the internal naming of the pins, so I've made a reference of what I used here.
Used a combination of this chip reference https://www.arduino.cc/en/Hacking/PinMapping32u4 and this board reference https://cdn.sparkfun.com/assets/9/c/3/c/4/523a1765757b7f5c6e8b4567.png to create the my keyboard. Specifically, the pins are organized into groups called ports, generally split by functionality. Analogue pin 3 (A3 on the sparkfun board reference) is read in the code as Port F pin 4 (PF4 in the arduino reference). 

## RHS:
### Rows:
| Board Pin | Pin In Code |
| TX1 | D3 |
| 4 | D4 |
| 5 | C6 |
| 6 | D7 |
| 7 | E6 |
| 8 | B4 |

### Cols:
| Board Pin | Pin In Code |
| A3 | F4 |
| A2 | F5 |
| A1 | F6 |
| A0 | F7 |
| 15 | B1 |
| 14 | B3 |
| 16 | B2 |
| 10 | B6 |

I organized my board as COL2ROW. As long as all the diodes are pointing the same direction, it won't matter which format you use, swapping COL2ROW/ROW2COL is as simple as changing `#define DIODE_DIRECTION [COL2ROW|ROW2COL].

