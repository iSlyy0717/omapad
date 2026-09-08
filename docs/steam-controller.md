# Steam Controller (2025/2026)

Valve's new Steam Controller (announced/shipped 2025–2026; kernel and SDL say "2026" / Ibex). This is **not** the 2015 pad.

People and sources disagree on exact product IDs. Treat the family as `28de:1302`–`1305` until `lsusb` on the target machine settles it.

| Hardware | USB ID | Notes |
|---|---|---|
| 2015 wired | `28de:1102` | hid-steam on kernels before 7.3 |
| 2015 wireless dongle | `28de:1142` | hid-steam on kernels before 7.3 |
| Steam Deck | `28de:1205` | hid-steam on kernels before 7.3 |
| 2026 Ibex wired | `28de:1302` | kernel/SDL; needs Linux 7.3 hid-steam |
| 2026 Ibex BLE | `28de:1303` | BLE; needs Linux 7.3 hid-steam |
| Proteus puck / "Steam Controller Puck" | `28de:1304` | SDL Proteus; some issues call this the 2026 USB controller |
| Nereid Steam Machine receiver | `28de:1305` | SDL |

Issue [ValveSoftware/steam-devices#80](https://github.com/ValveSoftware/steam-devices/issues/80) called `28de:1304` the 2026 USB controller and `28de:1007` the bootloader (`cdc_acm` can steal the flash device). Kernel/SDL name 1304 as the Proteus puck. Record whatever `lsusb` prints; do not assume.

## Kernel vs Steam

On Linux 7.2 and older, `hid-steam` typically aliases only `1102`, `1142`, and `1205`. Linux 7.3 is where Ibex support landed.

Without Steam on those older kernels the new pad is lizard mode: HID mouse + keyboard + vendor collection. Trackpad may move the system cursor. It is not a gamepad.

With Steam, Steam claims hidraw, turns lizard off, and tries to publish virtual mouse/keyboard plus an emulated Xbox 360 pad / Steam Input device.

`steam-devices` udev already grants `uaccess` to all vendor `28de` USB/hidraw. No extra udev for normal use. Bootloader `1007` is a firmware-flash edge case.

## Hyprland desktop mouse bug

[steam-for-linux#13185](https://github.com/ValveSoftware/steam-for-linux/issues/13185) (Arch-family + Hyprland, USB id `28de:1304`):

- Before Steam: lizard-mode trackpad moves the system cursor
- After Steam: Steam grabs hidraw, fails Deck-style registration (`Deck Controller PCB Serial# invalid: NA`), never creates a uinput mouse
- Games still work; desktop cursor does not

Phase 1 only needs Steam to inject a **keyboard** chord. If Super+Ctrl+X never arrives, try a plain function key. If the cursor dies when Steam starts, that is this bug, not Omapad.

## Steam Input as mapper

Desktop Layout: Steam → Settings → Controller → Desktop Layout.

Guide Button Chord Layout: hold Steam + another button. That is the prefix layer.

Binds: gamepad, keyboard, mouse, some system actions. Not arbitrary shell. Not Herdr's socket.

Configs live under `~/.steam/steam/userdata/<id>/241100/remote/`.

When Steam Input is on, do not also read the kernel hidraw pad. Steam unregisters hid-steam nodes (when hid-steam is in play) and owns the device.

## Haptics

The 2015 pad has no rumble motors (haptic pulses on the pads). The 2026 Ibex/Deck path has kernel `FF_RUMBLE` and output reports `0x80` / `0x81` in mainline hid-steam (Linux 7.3).

With Steam running, rumble/haptics should go through Steam Input (`TriggerHapticPulse` / `TriggerVibration`), not vendor HID until hidraw is proven free.

## Do not copy Xbox 1:1

New pad extras vs a Series controller: dual pads and/or dual sticks, extra grips (`BTN_GRIPL2` / `GRIPR2` on Ibex), gyro, Start-hold as gamepad vs desktop on Deck/Ibex firmware.

Map Omapad actions onto those controls. Do not pretend the hardware is an Xbox pad.
