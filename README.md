# Omapad

Omakase for your hands: drive Omarchy and Herdr from a Steam Controller.

This repo is research and planning only. No plugin code yet.

| | |
|---|---|
| Display name | Omapad |
| Plugin id | namespaced at publish; must not use the reserved `omarchy.*` prefix |
| Target | Omarchy (Arch + Hyprland + omarchy-shell) |
| Hardware | 2025/2026 Steam Controller (not the 2015 pad) |

OMA in Omarchy is omakase — chef's choice. Omapad is the chef's choice of how you drive the machine: a Steam Controller on the desk, dictation on a trigger, coding agents on the face buttons.

## Docs

- [docs/plan.md](docs/plan.md) — staged build, what ships first
- [docs/architecture.md](docs/architecture.md) — daemon vs QML vs Steam Input
- [docs/steam-controller.md](docs/steam-controller.md) — 2025/2026 pad on Linux
- [docs/sources.md](docs/sources.md) — public references

## License

[MIT](LICENSE)

## Non-goals (for now)

- Do not edit `/usr/share/omarchy/`
- Do not vendor GPL remappers (sc-controller, AntiMicroX) into an MIT plugin
- Do not treat this pad as an Xbox clone
- Do not write QML until Phase 1 dictation is proven on hardware
