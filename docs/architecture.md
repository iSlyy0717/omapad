# Architecture

Omapad is a Linux/Omarchy product: a Steam Controller drives the desktop and, later, Herdr agents.

## Layers

```
Steam Controller (2025/2026)
        │
        ▼
Steam Input  (owns hidraw; Desktop Layout + optional Xbox 360 evdev)
        │
        ├─ virtual keyboard/mouse  →  Hyprland binds  →  voxtype / omarchy-shell / hyprctl
        │
        └─ virtual gamepad evdev   →  future omapad daemon  →  herdr.sock
                                                              omarchy-shell IPC
                                                              haptics via Steam Input
        │
        ▼
omarchy-shell QML plugin (optional)
  service + overlay/bar-widget
  status, learn-mode UI, summon/hide
  does not open hidraw or /dev/uinput
```

## Why Steam is in the path

`hid-steam` in Linux 7.3 is where first-party kernel support for the new pad landed. On older kernels the module typically only aliases:

- `28de:1102` 2015 wired
- `28de:1142` 2015 dongle
- `28de:1205` Steam Deck

It does **not** bind `1302`–`1305` (Ibex / Proteus / Nereid). Until 7.3 (or a distro backport), Steam, SDL HIDAPI, or sc-controller is how userspace sees a gamepad.

Steam Input Desktop Layout is a GUI mapper. That is Phase 1. A daemon that reads Steam's virtual pad is Phase 3B.

## What cannot live in QML

Omarchy plugins are QML + `manifest.json` inside the long-lived `omarchy-shell` process. Kinds: `service`, `bar-widget`, `overlay`, `panel`, `menu`, `bar`.

The shell injects `omarchyPath`, `shell`, `manifest`, registries. IPC: `omarchy-shell shell summon|hide|toggle|call`.

Quickshell has `Process` / `Socket` / `IpcHandler`. It does not have HID or uinput types. On-screen keyboard plugins in this ecosystem keep QML for UI and put uinput/`wtype` in a helper process.

Do not link ydotool into plugin code (AGPLv3). Call `wtype` (MIT) or `ydotool` as a process if needed. Prefer Hyprland binds so Steam's virtual keyboard is enough.

## Herdr socket

Herdr on Linux exposes a newline-delimited JSON Unix socket (default `~/.config/herdr/herdr.sock`, overridable with `HERDR_SOCKET_PATH`):

- `pane.split`, `pane.send_keys` / `pane.send_input`
- workspace / tab / agent methods
- `plugin.action.invoke`
- `agent.list` for blocked / done / idle — polling about every 500 ms is enough; per-pane event subscribe has to track panes as they come and go

A Herdr plugin can own agent RPC and still needs a HID/injection backend on Linux.

## Intended config shape

- W3C standard-gamepad names (`a`, `lt`, `dpad_up`, `share`, sticks)
- sections for compositor actions, injected keys, raw socket binds, and haptics
- prefix hold or tap
- focused-pane substitution for socket methods
- learn / setup writing a profile

## Names already taken

Omagotchi, Omaland, OmaVibes, Omarchist. Do not use **Omacon** (Omacom Foundation). Third-party ids cannot use `omarchy.*`.
