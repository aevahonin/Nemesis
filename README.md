# Nemesis

A local music player. Native UI on [iced](https://iced.rs/), no embedded browser. Version **0.1.1-beta** (Free): download, pick a folder, play. Search and playlists are not in the UI yet. No macOS.

Русский: [README.ru.md](README.ru.md)

Releases: [github.com/aevahonin/Nemesis/releases](https://github.com/aevahonin/Nemesis/releases)

<p align="center">
  <img src="media/grid.gif" alt="Nemesis" />
</p>

<p align="center">
  <img src="media/1.png" width="49%" alt="" />
  <img src="media/2.png" width="49%" alt="" />
  <img src="media/3.png" width="49%" alt="" />
  <img src="media/4.png" width="49%" alt="" />
  <img src="media/5.png" width="49%" alt="" />
  <img src="media/6.png" width="49%" alt="" />
</p>


---

## What it does

On-disk library (mp3, flac, wav, ogg/Vorbis, m4a, aac), tabs for queue / albums / artists / tracks / folders, queue, shuffle, repeat, crossfade, seek, spectrum, covers. Frameless window, dark and light themes, Ru/En.

**Native format:** the app outputs the file’s sample rate and bit depth and does not resample itself. PipeWire or shared WASAPI may still resample downstream. Exclusive mode, DSD, and PEQ are not this release.

Data: Linux `~/.nemesis/`, Windows `%USERPROFILE%\.nemesis\` (library, cover cache, window, settings).

---

## Linux and Windows

Windows x86_64 smoke (2026-09-03): window, scan, play/next/stop, SMTC, theme.

| | Linux (x86_64) | Windows 10/11 (x86_64) |
|---|---|---|
| Scan, queue, shuffle, repeat, crossfade | yes | yes |
| Native PCM format | yes | no (soon) |
| Volume in native format | `pactl` (sink-input) | WASAPI session volume |
| Media keys without focus | MPRIS (`nemesis` on D-Bus) | SMTC |
| OS theme / accent | portal, GNOME, KDE | registry |
| Frameless, transparent window | yes | yes |
| Install | `.deb` / `.rpm` / `.pkg.tar.zst` / tar.gz | **portable zip**, no installer (installer soon) |

---

## Install

Use the **x86_64** artifact on a typical PC. Filenames are on the release (tag matches Cargo: `v0.1.1-beta`).

### Debian / Ubuntu (12+ / 22.04+)

```bash
sudo dpkg -i nemesis_*_amd64.deb
sudo apt-get install -f   # if dependencies are missing
```

You need PipeWire or PulseAudio and a GPU driver (Vulkan/OpenGL).

### Fedora (39+)

```bash
sudo dnf install nemesis-*.x86_64.rpm
```

### Arch / Manjaro / EndeavourOS

```bash
sudo pacman -U nemesis-*-x86_64.pkg.tar.zst
```

No AUR package yet.

### Linux without a distro package

Extract `nemesis-*-linux-x86_64.tar.gz` at `/` (it contains `usr/bin/nemesis` and a `.desktop` file). Or run the binary from `usr/bin/` on a system with a matching glibc.

### Windows

Unzip `nemesis-*-windows-x86_64-portable.zip` and run `nemesis.exe`. No extra DLLs. It does not add itself to the Start menu.

---

## Support matrix

| Build | Status |
|---|---|
| Linux x86_64, glibc ≥ 2.36 (Debian 12, Ubuntu 22.04, Fedora 39+, current Arch) | primary |
| Windows 10/11 x86_64 | primary, portable |
| Linux aarch64, Windows ARM64 | **experimental** |

Experimental builds have **not been run anywhere**: the package may not install, the window or audio may not start. For daily use, stick to x86_64.

Still untested (reports welcome if you try):

- a DE / compositor other than GNOME/KDE
- HiDPI, multi-monitor, Windows snap
- headset hardware keys, SMTC/MPRIS from the OS UI
- large libraries, CUE, odd tags
- native format 24-bit / 96 kHz on a given device
- Windows ARM / Linux ARM, if you attempt it

---

## Stack

Rust, [iced](https://iced.rs/) 0.14, wgpu, rodio/cpal, Symphonia, Lofty, rusqlite (SQLite linked in), rfd (folder picker). Audio: ALSA → PipeWire/Pulse on Linux, WASAPI on Windows.

---

## Feedback

[Issues](https://github.com/aevahonin/Nemesis/issues) on this repo.

Please include:

1. OS and version (e.g. Arch, Ubuntu 24.04, Windows 11 23H2), desktop (GNOME / KDE / other).
2. Architecture: x86_64 or ARM.
3. Exact release filename (`nemesis_…deb`, zip, …).
4. Version from About.
5. What you did, what you expected, what happened. A screenshot for UI bugs.
6. Native format on/off, problem file format (flac 16/44.1, etc.).

Windows release has no console, so there is no cmd log. On Linux, paste terminal output if you launched it from a terminal.

Do not attach the whole library or `library.db`. One file that reproduces the bug (if you can share it) plus steps is enough.
