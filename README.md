# herdr-zsa-lights

A [herdr](https://herdr.dev) plugin that shows agent status on a
[ZSA](https://www.zsa.io) keyboard's backlight, so you see which agent needs
you without looking at the screen.

- **Number key N** lights for workspace N — the same N as herdr's
  `ctrl+alt+N` — amber while its agent is blocked on you, green when it's done.
- **The top-right key** glows amber while any agent is blocked; bind it to the
  `next-agent` action and one press takes you there.

The rest of the board keeps your Oryx colours; only those keys change, and
everything is restored when the watcher stops or you toggle it off.

## Requirements

- herdr 0.9.3+
- [Keymapp](https://www.zsa.io/flash) running, with the **API enabled** in its
  config page
- [kontroll](https://github.com/zsa/kontroll) (`cargo install --git https://github.com/zsa/kontroll`
  on Linux; release binaries on macOS)
- `jq`, `bash`
- A Moonlander or Voyager (Linux also needs ZSA's udev rules, as for Keymapp)

## Install

```sh
herdr plugin install jwarykowski/herdr-zsa-lights
```

The watcher starts with each herdr session and exits when herdr stops.

## Actions

| action | does |
|---|---|
| `jwarykowski.herdr-zsa-lights.next-agent` | focus the next blocked agent, else the next done one; repeat to cycle |
| `jwarykowski.herdr-zsa-lights.toggle` | lights off/on |
| `jwarykowski.herdr-zsa-lights.restart` | restart the watcher, e.g. after a config change |

Bind them in herdr's `config.toml`, then point keyboard keys at those chords
in Oryx (e.g. the top-right key sends `ctrl+alt+a`):

```toml
[[keys.command]]
key = "ctrl+alt+a"
type = "plugin_action"
command = "jwarykowski.herdr-zsa-lights.next-agent"
description = "focus the agent that needs you"

[[keys.command]]
key = "ctrl+alt+shift+a"
type = "plugin_action"
command = "jwarykowski.herdr-zsa-lights.toggle"
description = "toggle keyboard agent lights"
```

## Configuration

Optional. Put overrides in `config.env` inside the plugin config directory
(`herdr plugin config-dir jwarykowski.herdr-zsa-lights`), then run the restart
action. See [`config.env.example`](config.env.example).

## Keyboard maps

[`maps/`](maps) holds each model's LED index for keys 1–9 and the top-right
key, derived from ZSA's QMK `rgb_matrix` layout. The Moonlander map is in daily
use; the Voyager map is untested — corrections welcome. A custom layout can
override the indices in `config.env`.

## License

MIT
