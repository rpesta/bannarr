# Changelog

All notable changes to Bannarr are recorded here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and versions follow
[Semantic Versioning](https://semver.org/).

## [Unreleased]

The first release: an interactive editor for the SSH login banner on any Linux box, with
special handling for containers made with community-scripts.

### Added

- **Editor.** A terminal editor with a live preview that runs the exact banner script it will
  install, using live values from the machine. When the terminal is big enough, it shows the
  gradient Bannarr wordmark.
- **Keyboard menus.** ↑/↓ to move, Space to check several items, Enter to choose, Esc to go back.
  The preview follows the highlight in the art gallery, font list and color picker, and stays
  visible throughout.
- **ASCII art.** A preset gallery (hostname in block letters, the Bannarr wordmark, Tux, a
  server rack, a welcome box, a divider line), a text-to-ASCII generator with a built-in block
  font (plus figlet's fonts if figlet is already installed), pasting, and editing in `$EDITOR`.
  Art can be one color or one of 18 gradients (ocean, fire, forest, sunset, purple, rainbow,
  mono, aurora, candy, lava, ice, mint, peach, neon, dusk, matrix, gold, steel) or a custom
  color list. Each is spread over the art's lines from first color to last.
- **Columns layout.** Two items per row. The left column is sized to its longest value, so
  long values like OS names or CPU models don't push the right column out of line.
- **Built-in font.** A readable 5×7 pixel font, 4 rows tall, for the hostname and the text
  generator.
- **Art size.** Block-letter art can be drawn at 1×, 2× or 3×. The default, `auto`, doubles
  only small art that still fits a normal terminal.
- **Info items.** 19 of them, including hostname, IP, OS, kernel, uptime, load, CPU, memory,
  swap, disk and disk mounts. Each can be shown or hidden, reordered, relabelled, recolored and
  given an icon. The icon sets are unicode (symbols every terminal font has) and emoji.
- **Usage bars.** For CPU, memory, swap, disk and mounts, with green, yellow and red usage
  colors and a dim dotted track. They can be switched on or off for all items or one at a time.
- **Zero dependencies.** Bannarr never installs packages, needs no special fonts, and works
  without a UTF-8 locale.
- **Text and layout.** Static text lines, and `list`, `columns` and `inline` layouts.
- **Install method.** Bannarr writes its own `/etc/profile.d/00_bannarr.sh` on any box. On
  community-scripts containers it switches off `00_lxc-details.sh` by renaming it, never by
  editing it.
- **Banner title.** A title line under the art with its own color, editable from the main
  menu, with the current title filled in for editing.
- **Community-scripts detection.** On first run, Bannarr says it detected a community-scripts
  container and shows the original banner's Banner Title (such as "Plex LXC Container") and
  Credits in a checklist, both kept unless unticked. It also offers to hide the plain
  title-and-OS copy that community-scripts writes to `/etc/motd`, using the reversible quiet
  mode.
- **openSUSE support.** The installed banner is executable, because openSUSE only loads
  executable `profile.d` scripts for some shells.
- **Copy to other servers.** Main menu **c** and `--export` produce a one-line command with a
  share code that recreates the design on another server. `--import` applies a share code
  without questions, or menu **c** imports one with a preview first.
  - **Adapts to each server:** hostname art, the community-scripts title and credits, and
    mounts.
  - **Safe:** codes are parsed and checked, never run.
- **Saving.** Pressing **s** asks Save or Cancel. When a saved banner would be replaced, it
  offers to back it up first, keeping exactly one backup. Bannarr then prints where the backup
  went and the command that undoes the save. `--restore` and the Restore menu bring the backup
  back, with a preview in the editor.
- **Quiet login** (opt-in). Hides the other login messages (`/etc/motd`, `update-motd.d`,
  `/etc/motd.d`), with an exact restore.
- **Undo.** `--revert` returns the box to its original login, and `--uninstall` removes
  Bannarr's settings as well.
- **Command-line options** for preview, apply, restore, quiet, revert and uninstall.
- **Logo and wordmark** set in Null Pointer by Tom Murphy 7.
