---
name: kanata-config-guide
description: Use this skill whenever you are about to write, edit, or review any kanata configuration in this repo — .kbd files (kanata.kbd, accents*.kbd, release-keys.kbd, nop.kbd, or any new .kbd file) — or whenever the user asks to add/change a layer, alias, template, defvar, defcfg option, deflayermap/defsrc entry, defvirtualkeys, tap-hold or other action. Make sure to use this skill even for small edits like tweaking a timeout or adding one key remap — kanata's syntax has sharp edges (argument order, platform-specific behavior, known bugs) that are easy to get subtly wrong by pattern-matching existing code alone.
---

# Kanata config guide lookup

This repo's `.kbd` files are kanata configurations — s-expression syntax with
strict per-action argument order and platform-dependent behavior. Before
writing or editing any kanata syntax, confirm it against the upstream
configuration guide rather than relying purely on memory or copying nearby
code, since existing code in this repo may itself be using a workaround or
outdated form.

## Where the guide lives

The guide is a sibling checkout of the kanata source repo:

```
../kanata/docs/config.adoc
```

(relative to this config repo's root, i.e.
`/Users/esousa/workspace/kanata/docs/config.adoc`).

If that path doesn't exist, tell the user the sibling `kanata` checkout is
missing before proceeding, since you cannot otherwise verify current syntax.

## How to use it

The guide is long (2000+ lines) — don't read it front to back. Instead:

1. Identify the construct you're about to touch (e.g. `tap-hold`, `defalias`,
   `deflayermap`, `defvirtualkeys`, `one-shot`, `switch`, `fork`,
   `process-unmapped-keys`, a `defcfg` option, etc.).
2. Grep the guide for that construct's section anchor or heading, e.g.:
   ```
   grep -n -i "tap-hold" ../kanata/docs/config.adoc
   ```
   Section headings use `[[anchor-name]]` markers right before `===`/`====`
   headers — search for the construct name to jump straight to its
   **Reference** (exact syntax/argument order) and **Description** (behavior,
   caveats, platform quirks) subsections.
3. Read enough of that section to confirm:
   - exact argument order and types (numbers vs strings vs nested lists)
   - default variant name vs explicitly-named variants (e.g. `one-shot` is an
     alias for `one-shot-press`)
   - platform-specific caveats or known issues (e.g. the `tap-hold` Linux
     repeat-of-antecedent-key issue, Windows-only quirks, macOS permission
     requirements)
4. Only then write or edit the config.

If you're touching a construct already used elsewhere in this repo's `.kbd`
files (e.g. the `release-keys` workaround in `release-keys.kbd`, or the
`switch`/`defvirtualkeys` remote-desktop logic in `kanata.kbd`), still check
the guide — existing usage may encode a platform-specific workaround whose
reasoning you need to understand before extending or copying it elsewhere.

## After editing

If you changed something with platform-specific behavior, double check via
the guide whether the change needs to be mirrored per-platform (this repo
uses `(platform (...) ...)` blocks extensively for macOS vs Windows/Linux
differences).
