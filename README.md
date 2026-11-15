# rpi5ami

A Commodore Amiga 500, 2000, 600 or 1200 for the Raspberry Pi 5, in a
window of its own: [esp32ami](https://github.com/rlucencic/esp32ami)'s
Amiga (with esp32emu's 68000 and 68EC020, Musashi) on the
[rpi5emu](https://github.com/rlucencic/rpi5emu) runtime.

- A 68000 at 7.09 MHz with the OCS and ECS chipsets (Agnus, Denise, Paula,
  two 8520 CIAs), or the A1200's 68EC020 with AGA (Alice, Lisa), Kickstart
  1.2 to 3.1 or the free AROS, floppy (ADF) and hard disk (HDF) images,
  Amiga Forever's RP9 packages, the A600's and A1200's IDE port, fast RAM.
- The setup screen (model, Kickstart, chip, slow and fast RAM, video, sound,
  the drives), the web page for the disk images (DMS archives and gzipped
  images unpacked in the browser), the console, as the rest of the family.

## Building

See [rpi5emu](https://github.com/rlucencic/rpi5emu#building): esp32emu,
rpi5emu, esp32ami and rpi5ami cloned side by side, and zlib
(`zlib1g-dev`), then

```sh
cd rpi5ami && cmake -S . -B build -G Ninja && cmake --build build
```

## ROMs

Kickstarts in the data folder's `roms/` (`~/rpi5emu/roms`), under any name:
each is found by its version (esp32ami's are FS-UAE's names,
`amiga-os-130.rom` ...). Not part of this repository; AROS's two ROMs are
free.

## Running

```sh
build/rpi5ami                                       # the setup screen first
build/rpi5ami -S --choice Model=A1200 --choice Kickstart=3.1
build/rpi5ami game.adf                              # with a disk in DF0:
```

Disk images and RP9 packages live in `~/rpi5emu/floppies/` (shared with
rpi5ste and rpi5arc). The keyboard by position: F11 Help, the Windows keys
the Amiga keys; F12 makes the cursor keys and the numeric keypad the
joystick in port 2 (Alt fire); the mouse in port 1 after a click in the
window.

## Licence

MIT.
