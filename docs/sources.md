# Sources

Public references used while planning Omapad.

## Omarchy / Hyprland

- [Shell Plugins](https://omarchy.org/manual/shell-plugins/)
- [Develop a Plugin](https://plugins.omarchy.org/develop.html)
- [Publish a Plugin](https://plugins.omarchy.org/publish.html)
- [Hyprland dispatchers](https://wiki.hypr.land/configuring/core/dispatchers/)

## Herdr

- [Herdr socket API](https://herdr.dev/docs/socket-api/)

## Steam Controller / Steam Input

- Linux `hid-steam.c`, `hid-ids.h` (torvalds/master)
- [Phoronix: Linux 7.3 hid-steam for 2026 Steam Controller](https://www.phoronix.com/news/Linux-7.3-HID)
- [ArchWiki: Steam Input Configurator](https://wiki.archlinux.org/title/Steam_Input_Configurator)
- [ArchWiki: Steam](https://wiki.archlinux.org/title/Steam)
- [steam-devices 60-steam-input.rules](https://github.com/ValveSoftware/steam-devices/blob/master/60-steam-input.rules)
- [steam-devices#80](https://github.com/ValveSoftware/steam-devices/issues/80) — 2026 pad `28de:1304` vs bootloader `1007`
- [steam-for-linux#13185](https://github.com/ValveSoftware/steam-for-linux/issues/13185) — desktop mouse on Hyprland
- [XDA: new Steam Controller without Steam](https://www.xda-developers.com/valve-steam-controller-best-gamepad-used-stops-being-one-close-steam/) — BLE `28de:1303` lizard collections
- [ISteamInput](https://partner.steamgames.com/doc/api/isteaminput)

## Related tools (call as processes, do not vendor)

- [sc-controller](https://github.com/C0rn3j/sc-controller) GPL-2.0
- AntiMicroX GPL-3, input-remapper GPL-3
- ydotool AGPLv3; wtype MIT
