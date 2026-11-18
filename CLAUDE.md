# sdlami: the Amiga 500, 2000, 600 and 1200 on Linux and macOS through SDL 3

esp32ami's Amiga (`../esp32ami/components/amiga`, platform independent) on
the sdlemu runtime (`../sdlemu`, its CLAUDE.md: the workflow, the Pi, the
lessons), the 68000 / 68EC020 (Musashi: `M68K_RUNTIME`,
`M68K_NO_BUS_ERRORS`, `M68K_PROGRAM_READS`, `M68K_PREFETCH`, as esp32ami's)
esp32emu's, zlib for the RP9 packages (as esp32ami's host build). The
machine's own behaviour, lessons and tests are esp32ami's CLAUDE.md; this
project is the glue.

## Layout

| Path | What |
|---|---|
| `src/main.c` | The Amiga as an `emu_machine_t`: esp32ami's `main/main.c` through `../sdlemu/tools/port_glue.py` and `port_finish.py`, then by hand: the memory by `malloc()` (16-byte aligned), the ROM message, the Tab5 panel's width for super-hires frames (`BOARD_LCD_H_RES` 1280: shown one to one there, the same shape here), the web script built in, `shutdown()` saves the clock's offset and writes the floppies back, the core's host-build watch hook a no-op |
| `src/settings.c` | esp32ami's, over `emu/nvs.h` (section `ami`) |
| `src/rom_store.c` | A file in `~/sdlemu/amiga/roms/` (`emu_rom_dir()`) of the Kickstart's size and version (AROS's by "AROS ROM"/"AROS EXT"), any name |
| `CMakeLists.txt` | `sdlemu_embed()` builds in esp32ami's `main/web/amifiles.js` |

The opcode table copies stay (unlike the Macs' and the ST's): the Amiga
sizes them with `sizeof(void *)` and rounds the copy's cycle counts for its
chip bus slots.

Keep `src/main.c` in step with esp32ami's `main/main.c`.

## Build, run, check

```sh
../sdlemu/tools/pi.sh sdlami build
../sdlemu/tools/pi.sh sdlami headless     # or run: the window here
../sdlemu/tools/console.sh i c
../sdlemu/tools/pi.sh sdlami shot out.png
```

Console: the runtime's (`h`) and esp32ami's (esp32ami's CLAUDE.md): `k
<hex>`, `K <hex> 0|1`, `T <text>`, `m <dx> <dy> <btn>`, `j <hex>`,
`R <model> <kick> [chip KB] [slow KB] [fast KB]`.

Checked on the Pi (2026-11-15, headless): the A500 (Kickstart 1.3, 512 KB
+ 512 KB) at 100% to the insert-disk hand; the A1200 (Kickstart 3.1, 68EC020,
AGA) at 100% to its animated insert-disk screen after the IDE search (~frame
2500). Not yet: Workbench from a disk (none here), the window by hand,
sound heard, hard disks, RP9 packages, super-hires.
