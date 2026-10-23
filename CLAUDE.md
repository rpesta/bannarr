# Bannarr

An interactive editor for the SSH login banner on any Linux box. One bash script,
`bannarr.sh`, with no build step and no dependencies beyond bash 4.3+, coreutils or busybox,
and `/proc`.

## Commands

```bash
bash -n bannarr.sh                                    # syntax-check the editor
BANNARR_ETC_DIR=/tmp/bn/etc bash bannarr.sh --apply   # run against a fake /etc, no root needed
sh -n /tmp/bn/etc/profile.d/00_bannarr.sh             # syntax-check a generated banner
shellcheck -s sh /tmp/bn/etc/profile.d/00_bannarr.sh  # if installed
```

The longer notes (how it works, the files it touches, the settings and function references,
testing, the community-scripts submission checklist) live in `docs/DEVELOPMENT.md`. They are
deliberately not in the README, which stays about installing and using Bannarr, in plain
language for non-developers.

## Architecture

`bannarr.sh`, top to bottom:

- **Paths and constants:** `ETC_DIR` (overridable with `BANNARR_ETC_DIR`), the logo, color
  lists and presets.
- **`emit_renderer`:** the POSIX `sh` renderer (`_bn_*`). It's embedded verbatim in the
  generated `/etc/profile.d/00_bannarr.sh`, and also `eval`'d into the editor.
- **State:** `set_defaults`, `load_state`, `write_conf` and `build_profile`. Settings are
  `BN_*` variables.
- **ASCII art:** presets, the built-in block font and figlet.
- **TUI helpers and menus:** `draw` redraws the live preview on every loop.
- **System changes:** `save_apply`, `park`/`unpark`, `quiet_motd`/`unquiet_motd`, backups,
  `revert_default` and `uninstall`.
- **`main`:** command-line options, then the main menu.

## Rules

- **Zero dependencies.** Bannarr must work on a stock box with nothing added: never install
  packages, never require special fonts (no Nerd Fonts), and never assume a UTF-8 locale. No
  `${s:i:1}` on strings that may hold multi-byte characters. See "Zero dependencies" in
  `docs/DEVELOPMENT.md`.
- **The renderer must stay POSIX `sh`:** no arrays, `[[ ]]`, `local`, `$'...'` or `${var^^}`.
  Write unusual glyphs as `printf` octal escapes.
- **Never edit or delete a file Bannarr didn't create.** Switch it off with `park` and restore
  it with `unpark`, and record anything else you change so it can be undone exactly.
- **Every function gets a short doc comment** above it, in the style of the existing ones.
- **The README stays user-facing.** Technical detail goes in `docs/DEVELOPMENT.md`.
- **Record user-visible changes** under `[Unreleased]` in `CHANGELOG.md`.
- **Never commit font files.** The logo font, Null Pointer, is used only as outlines.
- **LF line endings only**, enforced by `.gitattributes`.
