# herdr-zsa-lights

A [herdr](https://herdr.dev) plugin that shows agent status on a
[ZSA](https://www.zsa.io) keyboard's backlight, so you see which agent needs
you without looking at the screen.

- **Number key N** lights for workspace N — the same N as herdr's
  `ctrl+alt+N` — amber while its agent is blocked on you, green when it's done.
- **The top-right key** glows amber while any agent is blocked; bind it to the
  `next-agent` action and one press takes you there.
- **The Q–P row** is a progress bar for
  [herdr-pomodoro](https://github.com/jwarykowski/herdr-pomodoro): red keys
  drain through a work round, blue through a break.
- **Music bars**: the `music` action starts
  [keybars](https://github.com/jwarykowski/keybars), a music equaliser on the
  rows below the number row; the agent keys stay on top, and the pomodoro bar
  steps aside until the music stops.
- **Party lights**: the `party` action cycles the whole board through a
  rainbow for a few seconds. Hook it to a merged PR or a finished pomodoro.

While any of these keys is lit, the rest of the board is dark: once Keymapp
drives a key, the firmware shows only what its API sets (there is no per-key
"show the layout here"). With nothing to show, the board keeps your Oryx
colours, and they come back when the watcher stops or you toggle it off.

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
| `jwarykowski.herdr-zsa-lights.party` | party lights for a few seconds |
| `jwarykowski.herdr-zsa-lights.music` | music bars on/off (needs [keybars](https://github.com/jwarykowski/keybars): `go install github.com/jwarykowski/keybars@latest`, plus `cava`) |
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

## Party on events

`party` is made for hooks. With
[herdr-gh](https://github.com/jwarykowski/herdr-gh), a merged PR throws a
party from `$(herdr plugin config-dir jwarykowski.herdr-gh)/hooks/pr.merged`:

```sh
#!/bin/sh
herdr plugin action invoke party --plugin jwarykowski.herdr-zsa-lights
```

The same line in herdr-pomodoro's `hooks/pomodoro.work_done` celebrates a
finished round.

## Configuration

Optional. Put overrides in `config.env` inside the plugin config directory
(`herdr plugin config-dir jwarykowski.herdr-zsa-lights`), then run the restart
action. See [`config.env.example`](config.env.example).

## Keyboard maps

[`maps/`](maps) holds each model's key index for keys 1–9, the top-right key
and the Q–P row, in Keymapp's order: each half numbered on its own, row by row
from the left, inner column included (not the firmware's `rgb_matrix` LED
order). The Moonlander map is checked on a real board; the Voyager map is
untested — corrections welcome. A custom layout can
override the indices in `config.env`.

## Support

herdr-zsa-lights is free and MIT licensed. If your keyboard tells you when an agent needs you, you can
[sponsor its development](https://github.com/sponsors/jwarykowski).

## License

MIT
