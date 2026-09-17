# kanata-config

Personal [kanata](https://github.com/jtroo/kanata) keyboard configuration.
Kanata is a cross-platform (Linux, macOS, Windows) tool for advanced keyboard
remapping — QMK/ZMK-style layers, tap-hold, caps-word and unicode output, but
implemented in software instead of firmware.

The goal of this layout is to keep every frequently-used key within the home
position: navigation, function keys, a numpad and Portuguese accented
characters are all reachable without moving your hands, by holding one key
with one hand and typing with the other.

## At a glance

Four keys are "gateways" — tap them for their normal character, hold them to
temporarily activate a layer:

```
   caps / esc  ──hold──▶  misc        numpad + symbols        (right hand)
   ;           ──hold──▶  cursor      arrows + navigation     (left hand)
   ret         ──hold──▶  function    F-keys + media          (left hand)
   '           ──hold──▶  accent      á à â ã é ê í ó ô õ ú ç (left hand)
```

The gateway always sits on the **opposite hand** from the keys it exposes, so
holding never fights with typing. Three of the four gateways (`;`, `'`, `ret`)
are under the right pinky, and each one lights up the same 3×4 block under the
left hand (`qwer` / `asdf` / `zxcv`). The left-hand gateway (`caps`/`esc`)
lights up a numpad under the right hand.

Anything not shown in a layer below is **transparent** — it keeps its normal
behaviour, so the layers are additive rather than replacements.

## Usage

```sh
kanata -c kanata.kbd
```

Validate the config without running it:

```sh
kanata -c kanata.kbd --check
```

On macOS, kanata needs Accessibility (and Input Monitoring, if using mouse
actions) permission in System Settings > Privacy & Security.

## Layers

In the diagrams below, the top half is the **physical key** you press and the
bottom half is what that key **does** on that layer. `·` means transparent
(unchanged). The bottom modifier row is omitted because it is unchanged on
every layer except `misc` (see its numpad note).

### base

The default layer. Only the four gateways and the modifier keys are touched;
everything else types what it says on the keycap.

```
 `      1     2     3     4     5     6     7     8     9     0     -     =     bspc
 tab    q     w     e     r     t     y     u     i     o     p     [     ]     \
 caps   a     s     d     f     g     h     j     k     l     ;     '     ret
 lsft   z     x     c     v     b     n     m     ,     .     /     rsft
 ───────────────────────────────── base (default) ─────────────────────────────────
 ·      ·     ·     ·     ·     ·     ·     ·     ·     ·     ·     ·     ·     ·
 ·      ·     ·     ·     ·     ·     ·     ·     ·     ·     ·     ·     ·     ·
 esc*   ·     ·     ·     ·     ·     ·     ·     ·     ·     ;*    '*    ret*
 ·      ·     ·     ·     ·     ·     ·     ·     ·     ·     ·     ·
```

`*` = dual-role key:

| Key | Tap | Hold |
|---|---|---|
| `caps` | `Esc` | `misc` layer |
| `esc` | `Esc` | `misc` layer |
| `;` | `;` | `cursor` layer |
| `'` | `'` | `accent` layer |
| `ret` | `Enter` | `function` layer |

Note that Caps Lock taps to `Esc` — it is not a Caps Lock key at all. Actual
Caps Lock lives on the `misc` layer, and `caps-word` (below) usually replaces
the need for it anyway.

The left/right Alt and Meta keys are also remapped, but only conditionally —
see [Platform differences](#platform-differences).

### cursor — hold `;`

Arrow keys and navigation on the left hand, with Ctrl and Alt available under
the right hand so word-wise and document-wise motions (`Ctrl+←`, `Alt+→`) work
without leaving the layer.

```
 `      1     2     3     4     5     6     7     8     9     0     -     =     bspc
 tab    q     w     e     r     t     y     u     i     o     p     [     ]     \
 caps   a     s     d     f     g     h     j     k     l     ;     '     ret
 lsft   z     x     c     v     b     n     m     ,     .     /     rsft
 ─────────────────────────────────── hold ; ───────────────────────────────────────
 ;      ·     ·     ·     ·     ·     ·     ·     ·     ·     ·     ·     ·     ·
 ·      home  ↑     end   pgup  ins   ·     ·     ·     ·     ·     ·     ·     ·
 ·      ←     ↓     →     pgdn  ·     ·     ·     ctrl  alt   HOLD  ·     ·
 ·      del   kp+   bspc  menu  ·     ·     ·     ·     ·     ·     ·
```

- `menu` is the context-menu key (`Shift+F10` on macOS, `cmp` elsewhere).
- `kp+` is the numpad plus key.
- The `` ` `` key emits `;`, so you can still type a literal semicolon without
  releasing the layer.

### misc — hold `caps` / `esc`

A numpad under the right hand, plus config and caps-word controls on the
number row.

```
 `      1     2     3     4     5     6     7     8     9     0     -     =     bspc
 tab    q     w     e     r     t     y     u     i     o     p     [     ]     \
 caps   a     s     d     f     g     h     j     k     l     ;     '     ret
 lsft   z     x     c     v     b     n     m     ,     .     /     rsft
 ────────────────────────────── hold caps  (or esc) ───────────────────────────────
 lrld   ·     ·     ·     ·     ·     ·     ·     nlk   prnt  caps  A_B   a_b   ·
 ·      ·     ·     ·     ·     ·     ·     ·     7     8     9     /     +     del
 HOLD   ·     ·     ·     ·     ·     ·     ·     4     5     6     -     ·
 ·      ·     ·     ·     ·     ·     ·     ·     1     2     3     ·
```

The right-hand block forms a real numpad shape:

```
   physical                      output
   i    o    p    [    ]         7    8    9    /    +
   k    l    ;    '              4    5    6    -
   ,    .    /                   1    2    3
   rmet ralt                     0    .
```

(`0` and `.` sit on `ralt` and `rctl` instead on Windows/Linux — see
[Platform differences](#platform-differences).)

Number-row keys:

| Key | Action |
|---|---|
| `` ` `` | `lrld` — live-reload the config file |
| `8` | Num Lock |
| `9` | Print Screen (`Shift+Cmd+4` screenshot on macOS) |
| `0` | Caps Lock (the real one) |
| `-` | `A_B` — caps-word: types `SCREAMING_SNAKE_CASE` |
| `=` | `a_b` — caps-word variant: types `snake_case` |
| `\` | Delete (forward delete) |

The two caps-word keys are the reason Caps Lock is rarely needed:

- `-` shifts letters and turns `-` into `_`, so typing `max-buffer-size`
  produces `MAX_BUFFER_SIZE`.
- `=` shifts **only** `-` into `_`, leaving letters alone, so the same
  keystrokes produce `max_buffer_size`.

Both end after 5 seconds idle, on a terminating key (such as space), or by
pressing the same key again.

### function — hold `ret`

F-keys on the same left-hand 3×4 block as the cursor layer, laid out
bottom-up like a numpad (F1–F9), with F10–F12 in the fourth column. Media and
brightness keys sit on the number row. Ctrl and Alt are under the right hand,
so shortcuts like `Alt+F4` stay a single-hand-plus-thumb affair.

```
 `      1     2     3     4     5     6     7     8     9     0     -     =     bspc
 tab    q     w     e     r     t     y     u     i     o     p     [     ]     \
 caps   a     s     d     f     g     h     j     k     l     ;     '     ret
 lsft   z     x     c     v     b     n     m     ,     .     /     rsft
 ────────────────────────────────── hold ret ──────────────────────────────────────
 [      brdn  brup  mute  vol-  vol+  bl-   bl+   ·     ·     ·     ·     ·     ·
 ·      f7    f8    f9    f10   ·     ·     ·     ·     ctrl  alt   ·     ·     ·
 ·      f4    f5    f6    f11   ·     ·     ·     ·     ·     ·     ·     HOLD
 ·      f1    f2    f3    f12   ·     ·     ·     ·     ·     ·     ·
```

- `brdn` / `brup` — screen brightness down/up.
- `bl-` / `bl+` — keyboard backlight down/up.
- `vol-` / `vol+` / `mute` — volume.

### accent — hold `'`

Portuguese diacritics on the left-hand block. Each accent is a `fork` on
Shift: hold either Shift for the uppercase form (`á` → `Á`). Because the
gateway is under the right pinky, `ret` is remapped to Right Shift on this
layer so you can reach uppercase without contorting.

```
 `      1     2     3     4     5     6     7     8     9     0     -     =     bspc
 tab    q     w     e     r     t     y     u     i     o     p     [     ]     \
 caps   a     s     d     f     g     h     j     k     l     ;     '     ret
 lsft   z     x     c     v     b     n     m     ,     .     /     rsft
 ─────────────────────────────────── hold ' ───────────────────────────────────────
 '      ·     ·     ·     ·     ·     ·     ·     ·     ·     ·     ·     ·     ·
 ·      ú     í     é     ê     ·     ·     ·     ·     ·     ·     ·     ·     ·
 ·      á     â     ó     ô     ·     ·     ·     ·     ·     ·     HOLD  shft
 ·      à     ã     ç     õ     ·     ·     ·     ·     ·     ·     ·
```

The twelve characters fill the block in rough frequency order, putting the
most-used ones (`á â ó ô`) on the home row under the strongest fingers:

| | `q`/`a`/`z` | `w`/`s`/`x` | `e`/`d`/`c` | `r`/`f`/`v` |
|---|---|---|---|---|
| upper | `ú` | `í` | `é` | `ê` |
| home | `á` | `â` | `ó` | `ô` |
| lower | `à` | `ã` | `ç` | `õ` |

The `` ` `` key emits `'`, so a literal apostrophe is still available without
releasing the layer.

### vimnav-next / vimnav-prev — defined but unbound

Two helper layers that prefix `]` or `[` before a key, producing
vim-unimpaired style motions (`]q` next quickfix item, `[b` previous buffer,
and so on) on the keys `q e t a s f l c b n spc`.

Nothing currently activates them. To use one, add a gateway to the `base`
layer, e.g.:

```lisp
(deflayermap (base)
  ]  (t! hold-layer vimnav-next)
  [  (t! hold-layer vimnav-prev)
)
```

## Tap-hold timing and behaviour

All the timing lives in the `defvar` block at the top of `kanata.kbd`. Every
gateway is a `tap-hold-press`, whose first two arguments are a *tap-repress
window* and a *hold timeout*:

| Variable | Value | Role | Applies to |
|---|---|---|---|
| `hold-timeout` (`$ht`) | 225 ms | How long to hold a gateway, with no other key pressed, before its layer activates. | all gateways |
| `tap-timeout` (`$tt`) | 40 ms | Tap-repress window — deliberately tiny, so fast typing never turns into a repeat. | `ret` |
| `dtap-timeout` (`$dtt`) | 200 ms | Same window, but wide enough for a deliberate double-tap-then-hold. | `;` `'` |
| — | 0 ms | No repress window at all. | `caps` `esc` |
| `macro-delay` (`$md`) | 10 ms | Currently unused — defined for future macros. | — |

Two behaviours are worth knowing:

**Double-tap-and-hold repeats the key.** Because `;` and `'` are held down to
reach a layer, they would otherwise lose auto-repeat. With the wider
`$dtt` window, tapping the key and then immediately pressing-and-holding it
again repeats the character (`;;;;;`) instead of activating the layer. A
single press-and-hold still activates the layer as usual.

**Gateways use `tap-hold-press`, not `tap-hold`.** Plain `tap-hold` only
decides tap-vs-hold from the timing of the gateway key itself, so a fast
hold-and-roll (hold `;`, quickly tap `d`) could resolve as a *tap* and emit a
literal `;d` instead of a right-arrow. `tap-hold-press` commits to the hold
action as soon as any other key is pressed, which makes fast rolls reliable.

## Platform differences

The config runs unmodified on macOS, Windows and Linux; differences are
handled with `(platform ...)` blocks.

| Behaviour | macOS | Windows / Linux |
|---|---|---|
| Context-menu key (`cursor` + `v`) | `Shift+F10` | `cmp` |
| Print Screen (`misc` + `9`) | `Shift+Cmd+4` | `prnt` |
| Numpad `0` (`misc`) | `rmet` | `ralt` |
| Numpad `.` (`misc`) | `ralt` | `rctl` |
| `release-keys` workaround | active | no-op |

**Modifier swapping for remote Windows sessions (macOS only).** While a
Microsoft Remote Desktop or Parallels console window is both frontmost **and**
fullscreen, `lalt`/`lmet` and `rmet`/`ralt` swap so that Windows modifier
muscle memory works inside the session, and `ralt` becomes the context-menu
key. Outside those conditions the keys behave normally.

The frontmost-app and fullscreen states are published as virtual keys by an
external helper, `kanata-vk-agent`; without it running, the modifiers simply
stay in their normal macOS positions. The condition is evaluated on the
modifier key itself rather than on the virtual key, because a
`defvirtualkeys` action is evaluated once when the agent presses it —
whichever of the app/fullscreen keys arrived first would otherwise decide the
outcome before the other one existed.

**macOS key-repeat workaround.** With the OS repeat delay set to its shortest
setting, starting a tap-hold layer change immediately after typing a normal
character can leave that character repeating after release. The `release-keys`
template in `release-keys.kbd` force-releases every alpha key (plus space,
comma and period) before the layer action runs. It wraps the `;` and `'`
gateways and is a no-op on Windows and Linux.

## Files

| File | Purpose |
|---|---|
| `kanata.kbd` | Main entry point: `defcfg`, timing variables, templates, platform-aware aliases and every layer. Includes `release-keys.kbd` and `accents-unicode.kbd`. |
| `release-keys.kbd` | The macOS `release-keys` workaround described above; no-op elsewhere. |
| `accents-unicode.kbd` | Accent aliases via the `unicode` action. **This is the variant currently included.** |
| `accents.kbd` | Alternative combining Unicode Hex Input, macOS `Option` dead keys and plain unicode per platform, with commented-out drafts of a per-input-source `switch`. |
| `accents-hexinput.kbd` | Alternative using the macOS "Unicode Hex Input" input source (`Option`+hex digits). |
| `accents-macos.kbd` | Alternative using macOS's built-in `Option` dead keys (e.g. `Option+e` then a vowel). |
| `nop.kbd` | Minimal do-nothing config, useful as a placeholder or sanity check. |

All four accent files define the same alias names (`@á`, `@ç`, …), so they are
interchangeable — swap the `(include "...")` line near the bottom of
`kanata.kbd` to pick the mechanism that works best for your platform and
active input source.

## Reference

Full syntax and semantics for every action used here (`tap-hold-press`,
`layer-toggle`, `deflayermap`, `deftemplate`, `defvirtualkeys`, `switch`,
`fork`, `caps-word-custom-toggle`, `unicode`, `platform`, …) is documented in
the upstream configuration guide:
[`docs/config.adoc`](https://github.com/jtroo/kanata/blob/main/docs/config.adoc)
in the [jtroo/kanata](https://github.com/jtroo/kanata) repo, rendered at
<https://jtroo.github.io/config.html>. Platform caveats are collected in
[`docs/platform-known-issues.adoc`](https://github.com/jtroo/kanata/blob/main/docs/platform-known-issues.adoc).
