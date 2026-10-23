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
- **ASCII art.** A preset gallery (hostname in block letters, the Bannarr wordmark, Tux, a
  server rack, a welcome box, a divider line), a text-to-ASCII generator with a built-in block
  font (plus figlet's fonts if figlet is already installed), pasting, and editing in `$EDITOR`.
  Art can be one color or a per-line gradient.
- **Art size.** Block-letter art can be drawn at 1×, 2× or 3×. The default, `auto`, uses 2×
  when it fits a normal terminal.
- **Info items.** 19 of them, including hostname, IP, OS, kernel, uptime, load, CPU, memory,
  swap, disk and disk mounts. Each can be shown or hidden, reordered, relabelled, recolored and
  given an icon. The icon sets are unicode and emoji.
- **Usage bars.** For CPU, memory, swap, disk and mounts, with green, yellow and red usage
  colors and a dim dotted track. They can be switched on or off for all items or one at a time.
- **Zero dependencies.** Bannarr never installs packages, needs no special fonts, and works
  without a UTF-8 locale.
- **Text and layout.** Static text lines, and `list`, `columns` and `inline` layouts.
- **Install method.** Bannarr writes its own `/etc/profile.d/00_bannarr.sh` on any box. On
  community-scripts containers it switches off `00_lxc-details.sh` by renaming it, never by
  editing it.
- **Community-scripts title and credit.** On first run, Bannarr reads the original banner's
  title (such as "Plex LXC Container") and its community-scripts credit line, and asks whether
  to keep each one.
- **Backups.** Named backups, a restore with a preview first, and automatic backups before
  every apply (the last 5 are kept).
- **Quiet login** (opt-in). Hides the other login messages (`/etc/motd`, `update-motd.d`,
  `/etc/motd.d`), with an exact restore.
- **Undo.** `--revert` returns the box to its original login, and `--uninstall` removes
  Bannarr's settings as well.
- **Command-line options** for preview, apply, backup, list, restore, quiet, revert and
  uninstall.
- **Logo and wordmark** set in Null Pointer by Tom Murphy 7.
