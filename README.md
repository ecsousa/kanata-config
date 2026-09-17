# kanata-config

Personal [kanata](https://github.com/jtroo/kanata) keyboard configuration.
Kanata is a cross-platform (Linux, macOS, Windows) tool for advanced keyboard
remapping — think QMK/ZMK-style layers, tap-hold, one-shot keys, and unicode
input, but implemented in software instead of firmware.

This repo maps a physical US-QWERTY-ish keyboard to a layer-based layout with
a Home Row Mod / dual-role `Caps Lock`/`Esc`, a cursor layer, a symbol/misc
layer, a function-key layer, an accented-character layer (for typing
Portuguese diacritics), and Windows-remote-desktop-aware modifier swapping on
macOS.

## Files

| File | Purpose |
|---|---|
| `kanata.kbd` | Main entry point. Defines `defcfg`, templates, the platform-aware modifier aliases, and the layers: `base`, `cursor`, `misc`, `function`, `accent`, `vimnav-next`/`vimnav-prev`. Includes `release-keys.kbd` and `accents-unicode.kbd`. |
| `release-keys.kbd` | A `release-keys` template used as a macOS-only workaround: force-releases all alpha/space/comma/period keys before a tap-hold layer action, to avoid stuck key-repeat when the OS repeat delay is set very short. No-op on other platforms. |
| `accents-unicode.kbd` | Defines the accented-character aliases (á, é, í, ó, ú, â, ê, ô, ã, õ, à, ç + uppercase) using the `unicode` action. This is the variant currently `include`d by `kanata.kbd`. |
| `accents.kbd` | Alternate/experimental accents implementation combining macOS Unicode Hex Input, macOS US-layout `Option` dead-key macros, and plain unicode output depending on platform, with commented-out drafts for a virtual-key-based per-input-source switch. Not currently included by `kanata.kbd`. |
| `accents-hexinput.kbd` | Alternate accents implementation using macOS's "Unicode Hex Input" input source (`Option`+hex digits) via `macro`. Not currently included. |
| `accents-macos.kbd` | Alternate accents implementation using macOS's built-in US-International-style `Option` dead keys (e.g. `Option+e` then a vowel) via `macro`. Not currently included. |
| `nop.kbd` | Minimal no-op config (empty `defsrc`/`deflayermap`) — useful as a placeholder/sanity-check config. |

Only one accents implementation is active at a time, selected via the
`(include "...")` line near the bottom of `kanata.kbd`; the other three are
kept side-by-side as alternatives for different platforms/input-source setups.

## Layers (defined in `kanata.kbd`)

- **base** — the default layer. Notable remaps:
  - `caps`/`esc` swapped, and each is a tap-hold: tap for Esc, hold to
    activate the `misc` layer.
  - `ret` (Enter) is tap-hold: tap for Enter, hold to activate the
    `function` layer.
  - `'` is tap-hold: tap for `'`, hold to activate the `accent` layer.
  - `;` is a hold-layer for the `cursor` layer.
  - Left/right Alt/Meta are swapped on macOS only while a Windows Remote
    Desktop / Parallels window is the frontmost app **and** fullscreen
    (via `defvirtualkeys` + `switch`), so Windows-style modifier muscle
    memory works inside a remote session.
- **cursor** — vim-style arrow keys on `hjkl`-adjacent keys (`asdf`), plus
  Home/End/PgUp/PgDn, Delete, Backspace, Insert, and a context-menu key.
- **misc** — numpad-style digits, symbols, `caps-word`, Num Lock, Print
  Screen, Delete, and platform-specific right modifier keys.
- **function** — F1–F12, media/brightness keys, and Ctrl/Alt.
- **accent** — Portuguese accented vowels/ç (tap for lowercase, hold Shift
  for uppercase), via the included accents file.
- **vimnav-next** / **vimnav-prev** — helper layers for macro-based
  "next match" / "previous match" style navigation.

## Reference

Full syntax and behavior for every action used here (`tap-hold`,
`layer-while-held`, `deflayermap`, `defvirtualkeys`, `switch`, `fork`,
`one-shot`, `caps-word`, `unicode`, etc.) is documented in the upstream
kanata configuration guide:
[`docs/config.adoc`](https://github.com/jtroo/kanata/blob/main/docs/config.adoc)
in the [jtroo/kanata](https://github.com/jtroo/kanata) source repo, also
rendered at https://jtroo.github.io/config.html.

## Usage

Run kanata pointing at the main config file:

```sh
kanata -c kanata.kbd
```

On macOS, kanata needs Accessibility (and, if using mouse actions, Input
Monitoring) permission in System Settings > Privacy & Security. See
[`docs/platform-known-issues.adoc`](https://github.com/jtroo/kanata/blob/main/docs/platform-known-issues.adoc)
in the kanata repo for platform-specific caveats.
