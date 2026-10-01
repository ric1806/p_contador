<!---

This file is used to generate your project datasheet. Please fill in the information below and delete any unused
sections.

You can also include images in this folder and reference them in the markdown. Each image must be less than
512 kb in size, and the combined size of all images must be less than 1 MB.
-->

## How it works

This is a 4-bit binary counter that shows its value in hexadecimal on a 7-segment display.
It is built only from logic gates in Wokwi and has three blocks:

1. **Register (Q3..Q0):** four D flip-flops hold the count. They all share the `clk` clock.
   When `rst_n` is 0, all four are cleared to 0 (active-low reset).
2. **+1 adder:** one NOT, three XOR and two AND gates compute `Q + 1`. On every rising
   clock edge this value is loaded into the register. After F (15) it wraps back to 0.
3. **4-to-16 decoder and segment logic:** a decoder made of AND gates turns on exactly one line
   for each value from 0 to F. For each segment, OR gates and a final NOR gate turn it on for
   the values where it must be lit. The outputs are active high, for a common-cathode display.

The display shows `0 1 2 3 4 5 6 7 8 9 A b C d E F` and then starts again.
The decimal point (`uo[7]`) is a direct copy of input `ui[7]`.

| Pin | Signal |
|-----|--------|
| `uo[0]`..`uo[6]` | segments a..g |
| `uo[7]` | decimal point (same as `ui[7]`) |
| `ui[7]` | decimal point input |

## How to test

1. Set `rst_n` to 0 and then back to 1: the display shows `0`.
2. Apply clock pulses, one at a time or at 1 Hz so you can see it. Each rising edge adds 1:
   `0, 1, 2, ... 9, A, b, C, d, E, F, 0, ...`
3. Set `ui[7]` to 1 and the decimal point turns on. Set it to 0 and it turns off.

In the Wokwi simulation, the slide switch selects between the 1 Hz clock and the **Paso** (step)
button (S key), and the **REINICIAR** (reset) button (R key) clears the counter to 0.

## External hardware

The 7-segment display on the Tiny Tapeout demo board, connected to `uo[0]`..`uo[7]`.
