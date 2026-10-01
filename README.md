<p align="center">  
    <img src="c8.png" alt="chip8 logo" width="160">  
</p>

<br />
<br />
<br />
<br />


# PiLang CHIP-8

A baseline CHIP-8 emulator written in [PiLang](https://pilang.netlify.app). It
runs standard raw `.ch8` ROMs loaded at address `0x200` and implements the
64 by 32 display, built-in font sprites, delay and sound timers, the 16-key
keypad, and the original CHIP-8 opcode set.

## Running

From the repository root:

```powershell
pilang run chip8.pi <file-name.ch8>
```

The `roms/` folder contains games and demos you can run right away, including a
maze generator, snake, and a Sierpinski triangle:

```powershell
pilang run chip8.pi roms/maze.ch8
```

Any compatible ROM works; replace the path with your own `.ch8` file. ROMs can
be at most 3584 bytes (4096 minus the 512 bytes reserved below `0x200`);
larger files are rejected with an error.

Close the window or press Escape to quit.

## Controls

The 16 CHIP-8 keys are mapped to the left-hand block of the keyboard, in order.
The hex value of each CHIP-8 key is shown on the right:

```
Keyboard        CHIP-8
1 2 3 4         0 1 2 3
Q W E R   ->    4 5 6 7
A S D F         8 9 A B
Z X C V         C D E F
```

Arrow keys are also supported as aliases, so games that use `5`/`7`/`8`/`9`
for movement are playable without reaching for the letter keys:

| Arrow | CHIP-8 key | Keyboard |
| ----- | ---------- | -------- |
| Left  | `7`        | `R`      |
| Right | `9`        | `S`      |
| Up    | `5`        | `W`      |
| Down  | `8`        | `A`      |

## Details

| Item         | Value                                                  |
| ------------ | ------------------------------------------------------ |
| Memory       | 4096 bytes, ROM loaded at `0x200`, font at `0x50`      |
| Display      | 64 x 32 monochrome, drawn at 12x scale (768 x 384)     |
| Speed        | Up to 20 instructions per frame, timers tick at 60 Hz  |
| Registers    | 16 8-bit registers `V0`-`VF`, 16-entry call stack      |
| Random seed  | Fixed (`1`), so `CXNN` output is the same on every run |

## Behaviour notes

- Drawing a sprite (`DXYN`) ends the current frame, like the original
  interpreter waiting for vertical blank. Sprites wrap around the screen edges.
- `8XY6` and `8XYE` shift `VX` in place and ignore `VY`.
- `FX55` and `FX65` leave `I` unchanged.
- `BNNN` jumps to `NNN + V0`.
- The sound timer counts down, but no audio is played.
- Only the original CHIP-8 instruction set is implemented. SUPER-CHIP and
  XO-CHIP extensions are not supported.