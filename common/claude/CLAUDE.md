# Claude Context - Ryan's Environment

This file provides context for Claude Code sessions.

## System Overview

| Property | Value |
|----------|-------|
| **OS** | Omarchy 3.2.3 (Arch Linux based) |
| **Kernel** | Linux 6.17.9-arch1-1 |
| **Desktop** | Hyprland 0.52.2 (Wayland) |
| **Shell** | zsh |
| **User** | ryanh |
| **Hostname** | aimaxomarchy |

## Hardware

- **Device:** MS-S1 MAX
- **CPU:** AMD RYZEN AI MAX+ 395 (32 cores)
- **GPU:** AMD Radeon 8060S (Integrated)
- **RAM:** ~124GB
- **Storage:** 1.86TB btrfs

## Key Directories

| Path | Purpose |
|------|---------|
| `~/dotfiles/` | GNU Stow managed configs ([GitHub](https://github.com/llkkdb/dotfiles)) |
| `~/xdg-desktop-portal-hypr-remote/` | Custom RemoteDesktop portal ([GitHub](https://github.com/llkkdb/xdg-desktop-portal-hypr-remote)) |
| `~/.config/hypr/` | Hyprland configs (symlinked from dotfiles) |

## Dotfiles Structure

```
~/dotfiles/
├── common/
│   └── zsh/                    # Shared zshrc
└── machines/
    └── omarchy/                # This machine
        ├── hypr/               # Hyprland configs
        ├── deskflow/           # KVM client settings
        ├── portals/            # XDG portal routing
        └── system/             # System files (sudo required)
```

**Stow commands:**
```bash
# Common configs
stow -d ~/dotfiles/common -t ~ zsh

# Omarchy-specific
stow -d ~/dotfiles/machines/omarchy -t ~ hypr deskflow portals
```

## Custom Setup: Deskflow on Wayland

This machine runs Deskflow as a **client** connecting to a Windows/Barrier server (192.168.8.206).

**Components:**
- `xdg-desktop-portal-hypr-remote` - Custom portal providing `org.freedesktop.portal.RemoteDesktop`
- Binary: `/usr/lib/xdg-desktop-portal-hypr-remote`
- Portal config: `/usr/share/xdg-desktop-portal/portals/hypr-remote.portal`
- D-Bus service: `/usr/share/dbus-1/services/org.freedesktop.impl.portal.desktop.hypr-remote.service`

**Auto-start:** `deskflow-core client` via `~/.config/hypr/autostart.conf`

**Note:** Running Deskflow as a SERVER on Hyprland is NOT supported (requires InputCapture portal which Hyprland doesn't implement yet).

## Package Manager

- **Primary:** pacman
- **AUR:** yay
- **Snapshots:** snapper (btrfs)

## GitHub

- **Username:** llkkdb
- **CLI:** gh (authenticated)

## Repositories

| Repo | Purpose |
|------|---------|
| [llkkdb/dotfiles](https://github.com/llkkdb/dotfiles) | System configurations |
| [llkkdb/xdg-desktop-portal-hypr-remote](https://github.com/llkkdb/xdg-desktop-portal-hypr-remote) | RemoteDesktop portal (sdbus-cpp 2.x patched) |

## Key Dependencies (Omarchy)

| Package | Version | Notes |
|---------|---------|-------|
| sdbus-cpp | 2.2.1 | D-Bus library (2.x API) |
| libei | 1.5.0 | Input emulation |
| hyprland | 0.52.2 | Wayland compositor |
| xdg-desktop-portal | 1.20.3 | Portal frontend |
| xdg-desktop-portal-hyprland | 1.3.11 | Hyprland portal backend |
| deskflow | 1.25.0 | KVM software |
| stow | - | Dotfile symlink manager |

## Notes for Future Sessions

1. **Wayland limitations:** No X11/Xwayland for input capture - use portals
2. **sdbus-cpp 2.x:** Requires strong types (`ServiceName{}`, `ObjectPath{}`) and `addVTable()` pattern
3. **Deskflow server mode:** Not possible on Hyprland until InputCapture portal is implemented
4. **Config changes:** Edit files in `~/dotfiles/`, symlinks auto-update

## Last Updated

2025-12-16
