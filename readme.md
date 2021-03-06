# Nomenclature
In qmk, the two first-class citizens are the `keyboard` and the `keymap`.
## Keyboard
The keyboard is the set of `rules.mk, config.h, keyboard.c, keyboard.h`, where usually instead of `keyboard` it's the keyboard's name. It contains the `LAYOUT` definition of the keyboard, as well as the row/col size, whether the keyboard is row or column driven, and other implementation for the hardware.

The `LAYOUT` macro has two parts, the first is a single dimension array which I think just declares the key variables. The second part organizes them with {} into rows and columns. It's good to note that the first section *SHOULD NOT* include vars for empty spaces. If your matrix is not perfectly rectangular there will be gaps in some of the rows, and this is fine. Just name your variables something descriptive like `K<row><col> (K41)` and skip the ones which aren't in your matrix.
The second part will need to include those spaces as `KC_NO` so that the board knows to not expect inputs from those spaces.

For example. A keyboard with two keys on the top row and 3 on the bottom row could be created like so:
_keyboard.h_
```
#define LAYOUT_3x3( \
  k00,      k02, \
  k10, k11, k12 \
) \
{ \
  { k00, KC_NO, k02 }, \
  { k10, k11, k12 } \
}
```

_config.h_
```
#define MATRIX_ROWS = 2
#define MATRIX_COLS = 3

#define MATRIX_ROW_PINS { ... }
#define MATRIX_COL_PINS { ... }
```

## Keymap
The keymap lives in a folder next to the keyboard. The mapping's name is the foldername, and the files inside are `config.h` and `keymap.c`. The map contains the keycode assignment for the switches in the `LAYOUT`.

Going along with our example above, we can make a key mapping as follows:

_keymap.c_
```
enum layer_names {
  _BASE
};



const uint16_t PROGMEM keymaps[][MATRIX_ROWS][MATRIX_COLS] = {
    /* Base */
    [_BASE] = LAYOUT(
      KC_A,               KC_B,
      KC_LSFT,  KC_SPC,   KC_ENT
    ),
};
```
This layout assigns the letter A and B to the top row of the keyboard,
and left shift, space, and enter to the bottom row. Note that it is a flat array, like the first chunk of the `keyboard::LAYOUT`, not a two-dimensional one. In addition, we do not need to add `KC_NO` no-ops to this mapping, they layout handles it for us.

A full list of keycodes can be found in the [QMK documentation](https://docs.qmk.fm/#/keycodes)

# Pin mappings
The Arduino Pro Micro has an ATMega34u chip on it. The breakout on the board doesn't match up with the internal naming of the pins, so I've made a reference of what I used here.
Used a combination of this [Arduino chip reference](https://www.arduino.cc/en/Hacking/PinMapping32u4) and [Sparkfun Pro Micro](https://cdn.sparkfun.com/assets/9/c/3/c/4/523a1765757b7f5c6e8b4567.png) to create the my keyboard. Sparkfun has a better pin reference [here](https://cdn.sparkfun.com/datasheets/Dev/Arduino/Boards/ProMicro16MHzv1.pdf). Specifically, the pins are organized into groups called ports, generally split by functionality. Analogue pin 3 (A3 on the sparkfun board reference) is read in the code as Port F pin 4 (PF4 in the arduino reference). 

## RHS:
### Rows:
| Board Pin | Pin In Code |
|----:|----|
| TX1 | D3 |
| 4 | D4 |
| 5 | C6 |
| 6 | D7 |
| 7 | E6 |
| 8 | B4 |

### Cols:
| Board Pin | Pin In Code |
|----:|----|
| A3 | F4 |
| A2 | F5 |
| A1 | F6 |
| A0 | F7 |
| 15 | B1 |
| 14 | B3 |
| 16 | B2 |
| 10 | B6 |

I organized my board as COL2ROW. As long as all the diodes are pointing the same direction, it won't matter which format you use, swapping COL2ROW/ROW2COL is as simple as changing `#define DIODE_DIRECTION [COL2ROW|ROW2COL].

