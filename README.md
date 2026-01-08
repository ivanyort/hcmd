# Header Change Mask Decoder

This project explains, step by step, how to interpret the `header__change_mask`
field produced by Qlik data integration products. The goal is to show which
columns in the change table (`__ct` tables) were updated compared to the source
table.

## What the program does

1. Reads the hexadecimal input (`header__change_mask`).
2. Normalizes the input (removes `0x` and left-pads with zero to keep an even
   number of characters).
3. Converts each byte to binary (8 bits per byte).
4. Applies the Event Hubs target rule:
   - `yes`: use all bits (discard 0).
   - `no` : discard the last N bits (default 7).
5. Removes leading zeros (null-trim).
6. Maps bits from right to left to data columns:
   - bit 1 = updated column
   - bit 0 = unchanged column

## Important rules

- Bit position follows the column ordinal in the change table.
- Bits map only to data columns; `header__...` fields are not part of the mask.
- Bytes are little-endian and may be left-trimmed for efficiency.
- The discarded-bit count matches the number of `header__` fields stored in `__ct`.
  Default is 7, but it can change based on configuration. It becomes 0 when the
  target is Event Hubs.

## How to use

1. Open `index.html` or https://ivanyort.github.io/hcmd/ in a browser
2. Enter the hexadecimal value of `header__change_mask`.
3. Choose whether the target is Event Hubs (`n` or `y`).
4. Set how many rightmost bits to discard (default 7).
5. Click "Decode" to see the result and the step-by-step output.

## Author

Ivan Yort <ivan.yort@qlik.com>

## Repository

https://github.com/ivanyort/hcmd

