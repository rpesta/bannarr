<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/logo-light.svg">
    <img src="assets/logo.svg" alt="Bannarr logo" width="96">
  </picture>
</p>

<h1 align="center"><img src="assets/wordmark.svg" alt="bannarr" width="360"></h1>

The editor greets you with the same wordmark in the terminal:

```
██       ████████ ██▄▄▄▄▄▄ ██▄▄▄▄▄▄ ████████ ██▄▄▄▄▄▄ ██▄▄▄▄▄▄
██▄▄▄▄▄▄ ▄▄▄▄▄▄██ ████████ ████████ ▄▄▄▄▄▄██ ████████ ████████
██▀▀▀▀██ ██▀▀▀▀██ ████████ ████████ ██▀▀▀▀██ ████████ ████████
██    ██ ██    ██ ██    ██ ██    ██ ██    ██ ██    ▀▀ ██    ▀▀
████████ ████████ ██    ██ ██    ██ ████████ ██       ██
████████ ████████ ██    ██ ██    ██ ████████ ██       ██
```

Interactive editor for the SSH login banner on any Linux box: LXC containers, VMs or bare
metal. Built with [community-scripts](https://community-scripts.org) (Proxmox VE
Helper-Scripts) in mind.

You design the banner in a terminal UI with a live preview that redraws after every change.
Bannarr installs it as its own file, `/etc/profile.d/00_bannarr.sh`. On containers created by
community-scripts, it also switches off their stock banner (`/etc/profile.d/00_lxc-details.sh`)
by renaming it, never by editing it. Going back to the stock banner is just a rename back
(see [Coexisting with community-scripts](#coexisting-with-community-scripts)).

> **Status: early draft.** Syntax checks pass and the banner renderer has been exercised, but
> the interactive menus have not yet been tested on real Debian, Alpine, RHEL or Arch guests.

```
  █▀▄ █▀█ █▀▀ █▄▀ █▀▀ █▀█
  █▄▀ █▄█ █▄▄ █ █ ██▄ █▀▄

  Provided by: community-scripts ORG | GitHub: https://github.com/community-scripts/ProxmoxVE
  ◆ OS        : Debian GNU/Linux 12 (bookworm)
  ⌂ Hostname  : docker
  ⇄ IPv4      : 192.168.1.42
  ↑ Uptime    : 3d 4h 12m
  ▣ CPU       : █░░░░░░░░░ 12% of 4 cores
  ▤ Memory    : ███░░░░░░░ 1.2 / 4.0 GiB (31%)
  ◫ Disk (/)  : ██████░░░░ 11.8 / 20.0 GiB (59%)
  ◫ /mnt/media: █████████░ 82.1 GiB free of 1.8 TiB
```

## Features

- **ASCII art**: preset gallery (hostname in block letters, the Bannarr wordmark, Tux, server rack, welcome box,
  divider line), a text-to-ASCII generator (built-in block font, or every
  [figlet](http://www.figlet.org/) font if figlet is installed), paste your own, or edit it in
  `$EDITOR`.
- **Art colors**: any single color, or a per-line gradient (ocean, fire, forest, sunset,
  purple, rainbow, mono, or your own list of 256-color numbers).
- **19 info items** (hostname, IP, OS, CPU, memory, disk, mounts...). You can turn each one on
  or off, reorder, rename, recolor and give it its own icon.
- **Disk mounts**: one row per mounted disk with free space, auto-detected or hand-picked.
- **Usage bars** for CPU, memory, swap, disk and mounts, optionally colored green, yellow or
  red as usage rises.
- **Icons**: `unicode` (safe everywhere), `emoji`, or `nerd` (needs a
  [Nerd Font](https://www.nerdfonts.com/) in your SSH client), or a custom symbol per item.
- **Static text lines** placed anywhere in the order, each with its own color.
- **Layouts**: `list`, `columns` (two per row) or `inline` (one line).
- **Live preview**: the preview runs the exact script that will be installed, using live
  values from the machine.
- **Safe to try**: the original banner is never edited or deleted. `--revert` brings it back
  at any time.
- **Quiet other login messages** (opt-in): hide `/etc/motd`, Ubuntu/Debian's `update-motd.d`
  scripts and `/etc/motd.d` entries, so your banner is the only thing a login shows. Every
  change is recorded and undone exactly.
- **Backups**: save named snapshots of a design and restore any of them with a preview first.
  The design is also auto-saved before every apply (last 5 kept), so every change can be undone.

## Usage

Run it as root on the box whose banner you want to change: a container, a VM, or a Proxmox
host itself. There's nothing to install. This one-liner fetches and runs the latest
version:

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/rpesta/bannarr/main/bannarr.sh)"
```

Use `bash -c "$(curl ...)"` exactly as shown. `curl ... | bash` won't work, because the editor
reads your keypresses from stdin and the pipe would take it over. Without `curl`, use
`wget -qO-` in its place. To pass options, add them after the closing quote, for example
`bash -c "$(curl -fsSL …/bannarr.sh)" -- --revert`.

From a local copy:

```bash
sudo bash bannarr.sh
```

| Command | What it does |
| --- | --- |
| `bannarr.sh` | Interactive editor with live preview |
| `bannarr.sh --preview` | Print the banner from the saved settings |
| `bannarr.sh --apply` | Build and install the banner from saved settings (e.g. after a hostname change). Also turns off a community-scripts banner that has come back |
| `bannarr.sh --backup [label]` | Back up the current saved design |
| `bannarr.sh --list-backups` | List backups, newest first |
| `bannarr.sh --restore NAME` | Restore and apply a backup |
| `bannarr.sh --quiet-motd` | Quiet other login messages (see [Quieting other login messages](#quieting-other-login-messages)) |
| `bannarr.sh --restore-motd` | Show them again, undoing exactly what `--quiet-motd` changed |
| `bannarr.sh --revert` | Turn Bannarr off and bring back the default login: the default banner, plus other login messages if they were quieted. Settings and backups are kept |
| `bannarr.sh --uninstall` | Revert, then optionally delete all settings and backups |
| `bannarr.sh --help` | Show help |

Changes only take effect when you press **s** (Save & apply) in the main menu. The new banner
appears on your next SSH login. In the main menu:
- **6** shows which other login messages this box has and lets you quiet or restore them.
- **r** opens **Backups & restore**: back up, restore (with a preview), revert to the
  default login, or uninstall.

### Keys in list menus

Info items: `<n>` toggle · `u <n>` / `d <n>` move · `l <n>` rename · `c <n>` color ·
`i <n>` icon · `m` pick mounts · `b` back.
Static text: `a` add · `e <n>` edit · `x <n>` delete · `t <n>` show/hide · `c <n>` color ·
`u/d <n>` move.

## Compatibility

| | Requirement |
| --- | --- |
| Editor (`bannarr.sh`) | bash 4.3+, coreutils or busybox, `/proc` |
| Generated banner | any POSIX `sh`: bash, dash, busybox ash |
| Tested targets (planned) | Debian, Ubuntu, Alpine, Fedora, Rocky/Alma, Arch, openSUSE |

The banner is shown by login shells that read `/etc/profile.d/` (sh, bash, ash). zsh and fish
don't read it by default.

## Coexisting with community-scripts

Nothing calls `00_lxc-details.sh` by name. At login, `/etc/profile` runs every file in
`/etc/profile.d/` whose name ends in `.sh`. Bannarr uses that:

| | Community-scripts box | Any other box |
| --- | --- | --- |
| **Save & apply** | Writes `00_bannarr.sh` and renames `00_lxc-details.sh` to `00_lxc-details.sh.bannarr-disabled`. The renamed file no longer matches `*.sh`, so it's skipped | Writes `00_bannarr.sh` |
| **Revert** (`--revert`) | Deletes `00_bannarr.sh` and renames the original back | Deletes `00_bannarr.sh` |
| **Default design** | Includes the "Provided by: community-scripts ORG" credit line | No credit line |

The original file is never edited. To undo by hand, run:
`mv /etc/profile.d/00_lxc-details.sh.bannarr-disabled /etc/profile.d/00_lxc-details.sh`.

If a community-scripts installer creates `00_lxc-details.sh` again, you'll see two banners
until you run `bannarr.sh --apply`. That moves the newer copy aside as
`00_lxc-details.sh.bannarr-disabled.<timestamp>`, so `--revert` still restores the original.

## Quieting other login messages

On many systems a login prints more than the profile.d banners, for example Ubuntu's news
and help links or Debian's legal notice. Quieting is **opt-in**: use main menu **6** or
`--quiet-motd`. Each change is recorded in `/etc/bannarr/quiet/` so it can be undone
exactly:

| Source | Shown by | Quiet does | Restore does |
| --- | --- | --- | --- |
| `/etc/update-motd.d/*` (Debian/Ubuntu dynamic MOTD) | `pam_motd` via `run-parts` | `chmod a-x` on each executable script and records its original mode. This is what community-scripts does too. Renaming isn't safe here, because `run-parts --lsbsysinit` still runs some names that contain a dot | Sets each recorded script back to its exact original mode. Scripts that were already off stay off |
| `/etc/motd` | `pam_motd` or sshd `PrintMotd` | Renames it to `/etc/motd.bannarr-disabled` | Renames it back |
| `/etc/motd.d/*` (e.g. Cockpit's hint on RHEL/Fedora) | `pam_motd` | Moves the entries to `/etc/bannarr/quiet/motd.d/`, because `pam_motd` shows every file in that directory whatever its name | Moves them back |

- **Package upgrades:** an upgrade can make an `update-motd.d` script executable again or
  recreate `/etc/motd`. While quiet mode is on, every `--apply` (or Save & apply) quiets
  them again.
- **Name clashes on restore:** if something new has taken an original's name, the restore
  leaves it alone and says so. The stored original stays in `/etc/bannarr/quiet/`, and
  `--uninstall` never deletes that folder while it holds anything.
- **Revert includes this:** `--revert` and `--uninstall` also restore login messages.
- **Not changed:** the `Last login: ...` line comes from sshd (`PrintLastLog` in
  `sshd_config`), and Bannarr doesn't touch it.

## How it works

```
                 ┌──────────── bannarr.sh (bash TUI) ─────────────┐
 /etc/bannarr/   │                                                │
  bannarr.conf ──┼─► load_state ─► BN_* vars + ART_LINES          │
  banner.txt   ──┘                     │                          │
                                       ▼                          │
                              build_profile  ── prints ──►  settings + renderer + art
                                       │                          │
                    draw (every menu loop): sh <tmpfile>  ◄── live preview
                    save_apply:           sh -n, then write ──► /etc/profile.d/00_bannarr.sh
                                          rename 00_lxc-details.sh ──► *.bannarr-disabled
                 └────────────────────────────────────────────────┘
```

1. **Settings are shell variables.** All settings are `BN_*` variables (see
   [Settings reference](#settings-reference)). The menus edit them in memory, and `write_conf`
   prints them as safely quoted `sh` assignments.
2. **One renderer, two uses.** `emit_renderer` prints a block of POSIX `sh` functions
   (`_bn_*`) that collect values and draw the banner. That block is
   - copied word for word into the generated profile script, and
   - `eval`'d into the bash editor, so the menus reuse the same labels, icons and colors.
3. **The generated profile script is self-contained.** `build_profile` writes:

   ```sh
   # Generated by Bannarr ...
   if [ -t 1 ] && [ -z "${BANNARR_SHOWN:-}" ]; then (
     BN_ITEMS='text1 os hostname ...'   # settings (write_conf)
     _bn_c() { ... }                    # renderer (emit_renderer)
     ...
     _bn_art() { cat <<'__BANNARR_ART__'
     ...art lines...
     __BANNARR_ART__
     }
     _bn_show
   ); fi
   BANNARR_SHOWN=1; export BANNARR_SHOWN
   ```

   - `[ -t 1 ]` keeps non-interactive sessions (`scp`, `rsync`, `ssh host cmd`) silent.
   - The `( ... )` subshell keeps Bannarr's variables and functions out of your login shell.
     The one exception is `BANNARR_SHOWN`, which is exported so the banner doesn't repeat in
     nested shells, tmux or screen. RHEL-likes run `profile.d` for every interactive shell,
     so without it you'd see the banner each time.
   - Login never reads `/etc/bannarr/`, so the banner keeps working even if that folder is
     deleted.
4. **The live preview runs the real thing.** Every menu loop calls `draw`, which runs
   `build_profile` into a temp file and executes it with `sh`. What you see is exactly what
   SSH will show.
5. **Saving is guarded.** `save_apply` writes the new script next to the live one and runs
   `sh -n` on it. Nothing changes unless that passes. If the saved design differs from the
   one being applied, it's auto-saved as a backup first. Only then does `save_apply` disable
   the community-scripts banner and swap in the new file.

### Files

| Path | Purpose |
| --- | --- |
| `/etc/profile.d/00_bannarr.sh` | The generated banner, run at login |
| `/etc/profile.d/00_lxc-details.sh.bannarr-disabled` | The community-scripts banner, switched off but unchanged |
| `/etc/bannarr/bannarr.conf` | Saved settings (`BN_*` assignments, mode 600) |
| `/etc/bannarr/banner.txt` | Saved ASCII art, one line per line |
| `/etc/bannarr/backups/<timestamp>[-label]/` | Backups: `bannarr.conf` + `banner.txt`. Auto-saves end in `-autosave`; only the last 5 are kept |
| `/etc/motd.bannarr-disabled` | `/etc/motd` while login messages are quiet |
| `/etc/bannarr/quiet/` | Record of quieted login messages: `enabled` marker, `update-motd.d.list` (original modes), `motd.d/` (moved entries) |

## Settings reference

All settings live in `bannarr.conf` and are embedded in the profile script.

| Variable | Default | Meaning |
| --- | --- | --- |
| `BN_ITEMS` | `os hostname ip uptime cpu memory disk` (with `text1` first on community-scripts boxes) | Space-separated display order: item keys and `textN` static lines |
| `BN_TEXT_<n>` | community-scripts credit line (`n`=1, community-scripts boxes only) | Static text line `n` |
| `BN_TC_<n>` | *(empty)* | Color override for text line `n` |
| `BN_LAYOUT` | `list` | `list`, `columns` or `inline` |
| `BN_INDENT` | `2` | Spaces before every line |
| `BN_LABEL_W` | `10` | Label column width (`list`/`columns`) |
| `BN_VALUE_W` | `26` | Value width in `columns` layout |
| `BN_SEP` | `: ` | Text between label and value |
| `BN_BAR` | `yes` | Show usage bars |
| `BN_BAR_W` | `10` | Bar width in cells |
| `BN_BAR_FULL` / `BN_BAR_EMPTY` | `█` / `░` | Bar characters |
| `BN_THRESH` | `yes` | Color bars (or values, when bars are off) green/yellow/red by usage |
| `BN_ICONS` | `unicode` | Icon set: `off`, `unicode`, `emoji`, `nerd` |
| `BN_MOUNTS` | *(empty = auto)* | `\|`-separated mount points for the `mounts` item |
| `BN_ART_COLOR` | ocean gradient | One color, or a space-separated list cycled per art line |
| `BN_ART_SRC` | `hostname` | `hostname` (art rebuilt on every load) or `custom` (from `banner.txt`) |
| `BN_C_LABEL` / `BN_C_VALUE` | `yellow` / `bgreen` | Label and value colors |
| `BN_C_SEP` / `BN_C_TEXT` / `BN_C_ICON` | `gray` / `gray` / `cyan` | Separator, static text and icon colors |
| `BN_L_<key>` | *(empty)* | Label override for an item |
| `BN_VC_<key>` | *(empty)* | Value color override for an item |
| `BN_I_<key>` | *(empty)* | Icon override for an item |

**Color specs** (`_bn_c`): `none`, `black`, `red`, `green`, `yellow`, `blue`, `magenta`,
`cyan`, `white`, `gray`, a `b` prefix for bold (`bred`, `bcyan`...), or a 256-color number
`0`–`255`.

## Info items

| Key | Label | Source (first one that works) | Bar |
| --- | --- | --- | --- |
| `hostname` | Hostname | `/proc/sys/kernel/hostname` → `hostname` | |
| `fqdn` | FQDN | `hostname -f` | |
| `ip` | IPv4 | `ip -4 addr` (global) → `hostname -I` → `ifconfig` | |
| `ipv6` | IPv6 | `ip -6 addr` (global) | |
| `os` | OS | `/etc/os-release` `PRETTY_NAME` → `NAME` → `uname -s` | |
| `kernel` / `arch` | Kernel / Arch | `uname -r` / `uname -m` | |
| `uptime` | Uptime | `/proc/uptime` | |
| `load` | Load | `/proc/loadavg` | |
| `cpu` | CPU | `/proc/stat` sampled over 0.2 s | ✓ |
| `cpu_model` / `cores` | CPU Model / Cores | `/proc/cpuinfo` | |
| `memory` | Memory | `/proc/meminfo` `MemAvailable` (older kernels: Free+Buffers+Cached) | ✓ |
| `swap` | Swap | `/proc/meminfo` | ✓ |
| `disk` | Disk (/) | `df -Pk /` | ✓ |
| `mounts` | *(mount path)* | `/proc/mounts` + `df -Pk`, one row per mount | ✓ |
| `procs` | Processes | `/proc/[0-9]*` | |
| `users` | Users | `who` | |
| `date` | Date | `date` | |

The `mounts` item skips pseudo filesystems, `/proc` `/sys` `/dev` `/run`, and the single-file
bind mounts LXC adds (`/etc/hosts`, etc.). In auto mode it also skips `/` while `disk` is
enabled, so the root disk isn't shown twice.

## Function reference

Every function has a doc comment in `bannarr.sh`. Summary:

### Renderer (POSIX sh, embedded in the banner)

| Function | Role |
| --- | --- |
| `_bn_show` | Entry point: art, then each `BN_ITEMS` entry |
| `_bn_item` | Print one item in the current layout |
| `_bn_cell` | Format icon + label + separator + bar + value |
| `_bn_val` | Collect an item's live value (`_v`) and usage percent (`_p`) |
| `_bn_label` / `_bn_icon` | Resolve label / icon (override → default) |
| `_bn_bar` | Build a usage bar |
| `_bn_mounts_all` / `_bn_mounts` | Detect mounts / choose which to show |
| `_bn_c` / `_bn_thc` | Color spec → ANSI escape / usage → threshold color |

### Editor (bash)

| Function | Role |
| --- | --- |
| `main`, `usage`, `need_root` | Entry point and argument handling |
| `is_cs_box`, `set_defaults`, `load_state`, `write_conf` | Settings in and out (`load_state` also reads a backup directory) |
| `emit_renderer`, `build_profile` | Generate the banner script |
| `save_apply`, `disable_cs_banner` | Install the banner and switch off the community-scripts one |
| `park`, `unpark` | Rename a file to / from `*.bannarr-disabled`, never overwriting a newer copy |
| `is_quiet`, `quiet_motd`, `unquiet_motd`, `motd_sources` | Quiet / restore other login messages and list what's there |
| `revert_default`, `uninstall`, `banner_status` | Go back to the default login / remove Bannarr / status line |
| `make_backup`, `list_backups`, `restore_backup`, `prune_autosaves` | Backups |
| `draw`, `rule`, `pause` | Screen redraw with live preview |
| `main_menu`, `art_menu`, `preset_menu`, `generate_menu`, `items_menu`, `mounts_menu`, `text_menu`, `style_menu`, `colors_menu`, `motd_menu`, `backups_menu`, `restore_menu` | Menus |
| `pick_color`, `ask_num`, `swatch`, `grad_swatch` | Input widgets |
| `preset_art`, `block_text`, `paste_art`, `edit_art`, `trim_art` | ASCII art sources |
| `move_item`, `text_ids` | List helpers |
| `pkg_install`, `sq`, `hn` | Utilities |

## Development

### Rules for the renderer

Everything inside `emit_renderer`'s heredoc runs under dash and busybox ash at login, so it
must stay **POSIX sh**: no arrays, `[[ ]]`, `local`, `$'...'` or `${var^^}`. Store unusual
characters (e.g. Nerd Font glyphs) as `printf` octal escapes.

### Testing without touching `/etc`

`BANNARR_ETC_DIR` stands in for `/etc`, so every file Bannarr touches (`profile.d`, `motd`,
`motd.d`, `update-motd.d`, and `bannarr/` for settings) lands in a test folder. It also
skips the root check. `BANNARR_DIR` can move just the settings folder. To simulate a
community-scripts box with an Ubuntu-style MOTD:

```bash
export BANNARR_ETC_DIR=/tmp/bn/etc E=/tmp/bn/etc
mkdir -p "$E/profile.d" "$E/update-motd.d" && echo 'echo STOCK' > "$E/profile.d/00_lxc-details.sh"
echo 'Welcome!' > "$E/motd" && printf '#!/bin/sh\necho hi\n' > "$E/update-motd.d/10-help" && chmod 755 "$E/update-motd.d/10-help"
bash bannarr.sh --apply && bash bannarr.sh --quiet-motd
ls "$E" "$E/profile.d"; stat -c '%a %n' "$E"/update-motd.d/*   # parked + 644
BANNARR_SHOWN= sh "$E/profile.d/00_bannarr.sh"                  # needs a terminal
bash bannarr.sh --revert && ls "$E" "$E/profile.d"               # everything back, 755 again
```

Before submitting changes, run `bash -n bannarr.sh`, then generate a banner and check it with
`sh -n`, and with `shellcheck -s sh` if available.

### Submitting to community-scripts

According to the [docs](https://community-scripts.org/docs/tools/addon/readme), add-ons live
at `tools/addon/<name>.sh` in
[community-scripts/ProxmoxVE](https://github.com/community-scripts/ProxmoxVE). Before opening a
PR:

- [ ] Source the shared helpers (`core.func`, `tools.func`, `error_handler.func`) like the
      other add-ons, and drop the stand-in `msg_*` functions (see `TODO(PR)` in the script).
- [ ] Add the website/JSON metadata entry the repo expects for add-ons.
- [ ] Decide on run mode: the docs say add-ons run on the Proxmox host and target a chosen
      container, while Bannarr runs on the box it customises. A host wrapper could
      `pct push` the script and `pct exec` it. The editor is interactive, so it would need
      `pct enter` or a TTY.
- [ ] Test on Debian, Ubuntu, Alpine, Fedora/Rocky, Arch and openSUSE guests.

## Known limitations / roadmap

- In LXC, `/proc/stat` usually shows the host's CPU usage rather than the container's
  (memory and disk are per-container via LXCFS).
- zsh and fish login shells don't read `/etc/profile.d/`.
- Mount points containing `|` aren't supported in `BN_MOUNTS`.
- The `Last login:` line (sshd `PrintLastLog`) still appears above the banner. Hiding it
  means editing `sshd_config` or adding a per-user `~/.hushlogin`, so it's left alone for now.
- Quieting doesn't cover `/run/motd.d` or `/usr/lib/motd.d`. These are generated at runtime
  or owned by packages, and are rarely used.
- Ideas: `{hostname}`-style placeholders in static text, custom command items, centered
  alignment, and install as `/usr/local/bin/bannarr`.

## License

MIT, matching community-scripts:
<https://github.com/community-scripts/ProxmoxVE/raw/main/LICENSE>

### Logo and wordmark font

The logo, the wordmark and the terminal wordmark are set in **Null Pointer** by
Tom Murphy 7 ([Divide By Zero fonts](http://fonts.tom7.com/)). They're used under the
font's free license, which permits graphics and design work, including commercial work.

- **No font file here:** the repository contains only the outlines and the block-art
  wordmark, never the font file.
- **Merchandise:** the designer asks that anyone selling physical products made with his
  fonts (T-shirts, mugs, books) send him a sample, or at least an email saying where the
  font is used.
