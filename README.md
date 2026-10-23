<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/logo-light.svg">
  <img src="assets/logo.svg" width="96" height="96" alt="">
</picture>

# Bannarr

### SSH banners, your way

**Design the banner your servers greet you with when you SSH in.**

Pick ASCII art, choose which system stats appear and how they look, and watch a live preview
as you go. The original banner stays safe, so you can put it back whenever you like.

</div>

> [!WARNING]
> **Not yet released: Bannarr is still under construction.** Expect bugs and breaking
> changes. It hasn't been tested on real Debian, Ubuntu, Alpine, RHEL or Arch systems yet, so
> only try it on a test box or a snapshotted container.

<!-- Screenshots will replace this sample once Bannarr has been run on real systems. -->

```
  █▀▀▄  ▄▀▀▀▄ ▄▀▀▀▄ █  ▄▀ █▀▀▀▀ █▀▀▀▄
  █   █ █   █ █     █▄▀   █▄▄▄  █▄▄▄▀
  █  ▄▀ █   █ █   ▄ █ ▀▄  █     █ ▀▄
  ▀▀▀    ▀▀▀   ▀▀▀  ▀   ▀ ▀▀▀▀▀ ▀   ▀

  ♦ OS        : Debian GNU/Linux 12 (bookworm)
  ⌂ Hostname  : docker
  ↔ IPv4      : 192.168.1.42
  ↑ Uptime    : 3d 4h 12m
  ■ CPU       : █········· 12% of 4 cores
  ▬ Memory    : ███······· 1.2 / 4.0 GiB (31%)
  ◘ Disk (/)  : █████····· 11.8 / 20.0 GiB (59%)
  ◘ /mnt/media: █████████· 82.1 GiB free of 1.8 TiB
```

Bannarr runs on the box whose banner you want to change: a container, a virtual machine, a
physical server, or a Proxmox host itself. There is nothing to install and nothing left
running in the background. Your banner shows up the next time you log in.

> [!NOTE]
> **Bannarr is an independent project. It is not part of, or endorsed by, community-scripts
> (Proxmox VE Helper-Scripts).** It works alongside the banner their containers come with, and
> puts that banner back when you're done.

## Why this exists

Containers made with [community-scripts](https://community-scripts.org) greet you with a
short banner showing the app name, OS, hostname and IP. A stock Ubuntu server greets you with
news, help links and update notices instead. Changing either one means editing system files by
hand, on every box, and remembering how to undo it. Bannarr turns that into an editor with a
live preview and an undo button.

## Features

- **ASCII art.** Pick from a gallery (your hostname in big letters, Tux, a server rack, a
  welcome box and more), type any text and turn it into block letters, or paste your own.
- **Art size.** Make block-letter art small, medium or large, or let Bannarr pick the biggest
  size that fits a normal terminal.
- **Colors.** Color the art with a single color or a gradient such as ocean, fire, sunset or
  rainbow.
- **19 system details.** Hostname, IP address, OS, uptime, CPU, memory, disk space and more.
  Show the ones you want, in the order you want, with your own labels and colors.
- **Every disk.** List each attached drive with how much space is free, or pick just the ones
  you care about.
- **Usage bars.** CPU, memory and disk can show a bar that turns from green to yellow to red as
  they fill up. Turn the bars on or off for all of them, or one at a time.
- **Icons.** Simple symbols or emoji, both of which work in any terminal.
- **Banner title.** A title line under the art, such as "Plex LXC Container", with its own
  color. On community-scripts containers it starts as their title, and you can edit it any
  time.
- **Your own text.** Add lines like a welcome message or a warning anywhere in the banner.
- **Layouts.** One detail per line, two columns, or everything on a single line.
- **Live preview.** See exactly what your login will look like, with real values from the
  machine, while you edit.
- **Safe to try.** Your original banner is never deleted, and one command brings it back.
- **Copy your design to other servers.** Happy with a banner? Bannarr gives you one command to
  paste on any other server to get the same design there. Hostname art, the community-scripts
  title and credits, and disk mounts adapt to each server automatically.
- **A backup when you want one.** When you save over a banner, Bannarr asks whether to back up
  the old one first, then tells you where it went and the one command that brings it back.
- **A clean login** (optional). Hide the other messages some systems show at login, such as
  Ubuntu's news and tips, so your banner is the only thing you see. They can be brought back
  exactly as they were.

## Requirements

- **A Linux box you can log in to as root.** Debian, Ubuntu, Alpine, Fedora, Rocky, Alma,
  Arch and openSUSE are all supported.
- **Nothing to install.** Bannarr needs only bash and curl, which almost every system already
  has, community-scripts containers included. A bare-bones Alpine image may need
  `apk add bash curl` first.
- **The banner appears for bash and sh users.** People whose login shell is zsh or fish won't
  see it.

## Install

There's nothing to install. Log in to the box as root and run:

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/rpesta/bannarr/main/bannarr.sh)"
```

- **Copy the command exactly as shown.** The common `curl … | bash` shortcut won't work with
  Bannarr's menus.
- **No curl?** Use `wget -qO-` in its place.
- **Adding an option** (see [Commands](#commands)): put it at the end, after `--`. For example,
  to go back to your original banner:

  ```bash
  bash -c "$(curl -fsSL https://raw.githubusercontent.com/rpesta/bannarr/main/bannarr.sh)" -- --revert
  ```

You can also download it and run your own copy:

```bash
git clone https://github.com/rpesta/bannarr.git
sudo bash bannarr/bannarr.sh
```

## Using it

Bannarr opens with a live preview of your banner at the top and a menu underneath. Nothing
changes on the machine until you save:
1. Press **s**, then choose **Save** or **Cancel**.
2. If a banner was already saved, Bannarr asks whether to back it up first. There is only ever
   one backup, the banner from before your last save.
3. Bannarr says where any backup went, and the command to undo the save.

Your new banner appears the next time you log in.

| Key | What it opens |
| :-: | ------------- |
| **1** | Banner art: choose, generate, paste, resize or color the ASCII art |
| **2** | Banner title: the line under the art, such as "Plex LXC Container". Edit, recolor or remove it, and show or hide the Credits |
| **3** | Info items: choose which system details to show, and how, including each one's bar |
| **4** | Static text: add your own lines |
| **5** | Layout & style: layout, spacing, usage bars and icons |
| **6** | Colors |
| **7** | Other login messages: hide or bring back the extra messages some systems show |
| **c** | Copy to other servers: show the command that recreates this design elsewhere, or import one |
| **s** | Save or cancel your changes, with the option to back up the banner they replace |
| **r** | Restore: bring back your backup, go back to the original banner, or uninstall |

Every menu works from the keyboard, like other community-scripts menus:
- **↑ / ↓** move the highlight.
- **Space** checks or unchecks the highlighted row in lists like Info items and Mounts, so you
  can tick several in a row.
- **Enter** chooses, or finishes a list.
- **Esc** goes back.
- **Other keys:** each screen lists the rest at the bottom, such as `u`/`d` to reorder or
  `c` for color. The number and letter shortcuts shown next to each option still work.

While you move through the art gallery, the font list or the color picker, the live preview
shows the highlighted choice, and Esc puts back what you had.

### Commands

You can also skip the menus and do common jobs directly:

| Command | What it does |
| ------- | ------------ |
| `--preview` | Show your saved banner |
| `--apply` | Install your saved banner again, for example after renaming the box |
| `--restore` | Bring back the banner you backed up when you last saved |
| `--export` | Print the command that copies your saved design to another server |
| `--import CODE` | Apply a design copied from another server (the command `--export` prints does this) |
| `--quiet-motd` | Hide the other login messages |
| `--restore-motd` | Bring the other login messages back |
| `--revert` | Turn Bannarr off and bring back the original login. Your design is kept |
| `--uninstall` | Turn Bannarr off and, if you choose, delete your design and backup too |
| `--help` | Show all of this |

### On community-scripts containers

Containers made with community-scripts come with their own banner:
- **First run:** Bannarr says it detected a community-scripts container and lists the two
  parts of its banner, both ticked:
  - **Banner Title**, such as "Plex LXC Container".
  - **Credits**, "Provided by: community-scripts ORG".

  Untick either to remove it, and the preview updates as you go. Both can be changed later
  under **2** (Banner title). Bannarr then asks whether to hide the plain copy of the title
  and OS that these containers print above the banner.
- **Saving:** Bannarr switches the original banner off so you don't see two, without deleting
  or changing it.
- **Going back** to the original (**r** in the menu, or `--revert`) switches it on again.

### Copying a design to other servers

Press **c**, then choose the first option. Bannarr shows a single command and saves a copy
in `/etc/bannarr/share-command.txt`. Run it as root on each server that should get the same
design:

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/rpesta/bannarr/main/bannarr.sh)" -- --import 'bannarr1:H4sIAAAA…'
```

- **No questions asked:** it applies the design straight away and prints the command to undo
  it, so it suits setting up several servers in a row.
- **Adapts to each server:**
  - **Hostname art** shows that server's name.
  - **On community-scripts containers,** the banner title and credits are that container's
    own.
  - **Disks** that server doesn't have are left out.
- **Preview first:** to look at a design before using it, open Bannarr on the other server,
  press **c**, and paste the command or just the code.
- **Safe to paste:** a share code is only text. Nothing in it is ever run, and Bannarr rejects
  any code that isn't a design it could have made itself.

## Privacy

Bannarr has no server, no analytics and no tracking. Everything happens on your machine, and
your designs are stored there. The only time it goes online is to download itself from GitHub
when you run the install command. It never installs anything. A share code holds your design
settings and nothing else, and it's only ever in the command you copy. Nothing is uploaded
anywhere.

## Limitations

- **Containers show the host's CPU usage.** Inside a container, the CPU figure usually
  reflects the whole host rather than the container. Memory and disk are the container's own.
- **zsh and fish users don't see the banner.**
- **The "Last login" line stays.** It comes from the SSH server, and Bannarr doesn't change your
  SSH settings.
- **Hostname art doesn't update by itself.** After renaming a box, run Bannarr with `--apply` to
  redraw it.

## License

Copyright 2026 Ryan Pesta.

Bannarr is licensed under the [MIT License](LICENSE). You may use, modify and share it freely,
as long as the copyright notice comes with it.

The logo and wordmark are set in **Null Pointer** by Tom Murphy 7
([Divide By Zero fonts](http://fonts.tom7.com/)), used under that font's free license. The
font file itself isn't included here.

The community-scripts name is used only to say which banner Bannarr works with. Bannarr does
not use or adapt the community-scripts logo.
