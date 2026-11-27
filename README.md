# fbnmtz.audio

Omarchy shell bar-widget plugin: volume slider, output picker, per-app mixer.

A personal clone of Omarchy's `omarchy.audio`, renamed to `fbnmtz.audio`,
with one change: output volume can go up to **200%** (slider maximum `2`,
`setOutputVolume` clamps to `2` instead of `1`). Everything else matches
upstream Omarchy `omarchy.audio`.

- **Plugin id:** `fbnmtz.audio`
- **Kind:** `bar-widget`
- **Entry point:** `Panel.qml` (+ local `Model.js`)
- **Cloned from:** `omarchy.audio` (`omarchy.clonedFrom`; calls made to
  `omarchy.audio` are routed to this clone while it is enabled)
- **Author:** fbnmtz

## What it does

- Output volume slider (0–200%) with mute toggle; click speaker to mute,
  right-click to toggle mute, mouse wheel to change volume (OSD shown).
- Keyboard navigation inside the panel (`h`/`j`/`k`/`l`, arrows, `m`, `Enter`,
  `Tab`, `Esc`).
- Output device picker and input (microphone) picker, input level meter.
- Per-application playback mixer (volume + mute per stream).
- DSP-aware volume: resolves the real sink behind speaker tunings / EasyEffects.
- Hides platform-internal audio nodes; virtual sources via `wpctl`.
- Localized through Omarchy's built-in `audio.*` translations (50 languages).

## Requirements

- **Omarchy** (Quickshell-based `omarchy-shell`, Hyprland), tested on
  Omarchy **4.5.0**; `omarchy.*` core plugins `omarchy.osd` and `omarchy.media`
  enabled (stock).
- **PipeWire + WirePlumber** (`wpctl`); Quickshell's Pipewire/Mpris services.
- Omarchy CLI helpers shipped with Omarchy, on `PATH`:
  `omarchy-audio-sink-availability`, `omarchy-audio-output-sink`,
  `omarchy-audio-output-set-default`, `omarchy-audio-input-set-default`,
  `omarchy-shell`.
- Optional: **EasyEffects** (reads `$XDG_CONFIG_HOME/easyeffects/db/easyeffectsrc`
  if present; harmless when absent) and Omarchy speaker tuning.
- **No network access. No extra packages. No root.** Nothing is written to disk
  by this plugin; volume changes go to PipeWire. Note that changing volume
  above 100% is allowed by this plugin but depends on your hardware/DAC.

Everything the plugin needs inside the plugin folder is self-contained:
`manifest.json`, `Panel.qml`, `Model.js`. QML modules `qs.Ui`, `qs.Commons`
and Quickshell come from Omarchy itself.

## Install

```sh
omarchy plugin add https://github.com/fbnmtz/fbnmtz.audio.git --enable
```

Or manually:

```sh
git clone https://github.com/fbnmtz/fbnmtz.audio.git \
  ~/.config/omarchy/plugins/fbnmtz.audio
omarchy-shell shell rescanPlugins
omarchy plugin enable fbnmtz.audio --section right   # choose bar section
```

Validate the folder at any time:

```sh
omarchy plugin validate ~/.config/omarchy/plugins/fbnmtz.audio
```

Enabled state lives in `~/.config/omarchy/shell.json` (a `bar.layout` entry
with `"id": "fbnmtz.audio"`). While `omarchy.clonedFrom` names `omarchy.audio`,
the stock widget is replaced by this clone.

## Usage

- Click to open panel; right-click bar icon to toggle master mute.
- Wheel on bar icon adjusts output volume (+OSD).
- Settings: none declared in `barWidget.schema`.
- IPC target `omarchy.audio` (also reachable as `fbnmtz.audio`); calls made to
  `omarchy.audio` are routed to this clone, e.g.
  `omarchy-shell omarchy.audio toggle`. `omarchy-shell shell rescanPlugins`
  reloads.

## Remove

```sh
omarchy plugin remove fbnmtz.audio
```

This disables the plugin, removes the checkout, and restores stock
`omarchy.audio`. Or remove `"id": "fbnmtz.audio"` from `shell.json` and delete
`~/.config/omarchy/plugins/fbnmtz.audio`, then `omarchy-shell shell rescanPlugins`.

## Origin & license

Derived from Omarchy's `omarchy.audio` (Omarchy is MIT licensed,
© David Heinemeier Hansson). See `LICENSE`. Omarchy's own copyright notice is
preserved in `LICENSE` as required by the MIT license.
