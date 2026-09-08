# Plan

Steam stays running in the background. On kernels older than Linux 7.3, `hid-steam` does not bind the 2025/2026 Steam Controller, so Steam is the driver.

Herdr is the long-term product. Omarchy/Hyprland + Voxtype is the first slice you can feel.

Steam Input Desktop Layout binds **keyboard / mouse / gamepad**, not shell commands. Dictation and Hyprland actions therefore go through keys the compositor already owns.

## Phase 1 — Voxtype (no code)

1. Plug the Steam Controller in (USB puck or BLE).
2. Start Steam and leave it in the tray.
3. Steam → Settings → Controller → Desktop Layout → Edit Layout.
4. Bind one button to keyboard **Super+Ctrl+X** (Omarchy default: `voxtype record toggle`).
   - Right trigger is a natural dictation slot.
   - Left grip is a spare if the trigger is analog-awkward.
5. Press it. Voxtype should toggle.

Optional: bind hold-to-talk to **F9** (Omarchy starts recording on press, stops on release). Steam activators can do hold vs tap.

If Super+Ctrl+X does not fire on Wayland, bind the same button to a plain key (`F13`) and add a Hyprland bind later. Do not add that bind until the pad is in hand.

Do not enable a full desktop-mouse layout until we know whether [steam-for-linux#13185](https://github.com/ValveSoftware/steam-for-linux/issues/13185) applies. Trackpad-as-cursor may die the moment Steam starts. Keyboard injection is what Phase 1 needs.

Suggested first map:

| Control | Action |
|---|---|
| Right trigger or Y | Voxtype toggle (`Super+Ctrl+X`) |
| Steam + Y | Voxtype cancel — needs a Hyprland bind; not there by default |

Leave A/B/D-pad alone until Herdr.

## Phase 2 — Omarchy / Hyprland from the same layout

Same chain: Steam button → key → existing Hyprland bind.

Useful Omarchy defaults:

- Super+Space — launcher
- Super+Ctrl+E — emoji overlay
- workspace next/prev, focus direction

Steam **Guide Button Chord Layout** (hold Steam + another) is the prefix layer. Keep destructive stuff there.

## Phase 3 — Herdr

Steam cannot talk to Herdr's Unix socket. Two ways:

**A. Steam stays the mapper.** Unused keys (F13–F24 or Super+Ctrl+letter) in Desktop Layout; Hyprland binds or small scripts call Herdr. Closest to "no daemon." Weak for analog sticks, focused-pane substitution, and rumble on agent status.

**B. Small Linux daemon, Steam still running.** Steam turns the pad into a normal gamepad (Xbox-shaped evdev). The daemon reads that with SDL/evdev and talks to Herdr over the socket. Pads, grips, and gyro only exist if Steam duplicates them onto that virtual pad, or if the daemon later speaks Steam Input.

Do A for a couple of Herdr actions. Do B for a full supervisor layout (next blocked agent, rumble, prefix).

## Phase 4 — Omarchy plugin listing

When there is something to ship:

- Git repo with `manifest.json` at the **repo root**
- Unique namespaced plugin id (not `omarchy.*`)
- `omarchy plugin validate ./`
- QML is UI only (status, learn-mode overlay, bar widget). HID / Steam / Herdr socket stay in a daemon or in Steam itself
- README + license (MIT is the marketplace example)
- List at [plugins.omarchy.org](https://plugins.omarchy.org/publish.html)

A QML-only plugin cannot own HID, uinput, or rumble. Follow the usual Omarchy split: QML under `~/.config/omarchy/plugins/<id>/`, helpers outside it.
