# Dotfiles

Personal configuration files managed with GNU Stow, supporting multiple machines.

## Structure

```
dotfiles/
├── common/                 # Shared across all machines
│   └── zsh/               # Zsh configuration
├── machines/
│   └── omarchy/           # Omarchy (Arch + Hyprland)
│       ├── hypr/          # Hyprland configs
│       ├── deskflow/      # Deskflow KVM client
│       ├── portals/       # XDG Desktop Portal routing
│       └── system/        # System files (require sudo)
└── README.md
```

## Quick Start

```bash
# Clone
git clone https://github.com/llkkdb/dotfiles.git ~/dotfiles
cd ~/dotfiles

# Install common configs (all machines)
stow -d common -t ~ zsh

# Install machine-specific configs
stow -d machines/omarchy -t ~ hypr deskflow portals
```

---

## Common Configs

Shared configurations that work across all machines.

| Package | Contents | Target |
|---------|----------|--------|
| `zsh` | `.zshrc` | `~/.zshrc` |

### Install Common

```bash
cd ~/dotfiles
stow -d common -t ~ zsh
```

---

## Machine: Omarchy (Arch + Hyprland)

### System Information

| Component | Version |
|-----------|---------|
| OS | Omarchy 3.2.3 |
| Kernel | Linux 6.17.9-arch1-1 |
| Desktop | Hyprland 0.52.2 (Wayland) |

### Packages

| Package | Contents | Target |
|---------|----------|--------|
| `hypr` | Hyprland configs | `~/.config/hypr/` |
| `deskflow` | Deskflow settings | `~/.config/Deskflow/` |
| `portals` | Portal routing | `~/.config/xdg-desktop-portal/` |
| `system` | System files | `/usr/share/...` (manual) |

### Install Omarchy Configs

```bash
cd ~/dotfiles

# User configs (via stow)
stow -d machines/omarchy -t ~ hypr deskflow portals

# System files (manual, requires sudo)
sudo cp machines/omarchy/system/usr/share/xdg-desktop-portal/portals/hypr-remote.portal \
    /usr/share/xdg-desktop-portal/portals/
sudo cp machines/omarchy/system/usr/share/dbus-1/services/org.freedesktop.impl.portal.desktop.hypr-remote.service \
    /usr/share/dbus-1/services/
```

### Deskflow Setup

Deskflow is configured as a **client** connecting to a Windows/Barrier server.

- **Server IP:** 192.168.8.206
- **Auto-start:** Enabled in `hypr/.config/hypr/autostart.conf`
- **Mode:** Headless client (`deskflow-core client`)

**Required:** Build and install [xdg-desktop-portal-hypr-remote](https://github.com/llkkdb/xdg-desktop-portal-hypr-remote) for Wayland support.

```bash
git clone https://github.com/llkkdb/xdg-desktop-portal-hypr-remote.git
cd xdg-desktop-portal-hypr-remote
mkdir build && cd build
cmake .. && make
sudo cp xdg-desktop-portal-hypr-remote /usr/lib/
```

### Hyprland Config Files

| File | Purpose |
|------|---------|
| `hyprland.conf` | Main config |
| `autostart.conf` | Auto-start apps (Deskflow) |
| `bindings.conf` | Keyboard shortcuts |
| `monitors.conf` | Display setup |
| `input.conf` | Input devices |
| `looknfeel.conf` | Appearance |
| `hypridle.conf` | Idle behavior |
| `hyprlock.conf` | Lock screen |
| `hyprsunset.conf` | Night light |
| `xdph.conf` | Desktop portal |

---

## Adding a New Machine

1. Create directory: `mkdir -p machines/<machine-name>`
2. Add stow packages with proper structure:
   ```
   machines/<machine-name>/
   └── <package>/
       └── .config/
           └── <app>/
               └── config-file
   ```
3. Install: `stow -d machines/<machine-name> -t ~ <package>`

---

## Syncing Changes

```bash
cd ~/dotfiles
git add -A
git commit -m "Update configs"
git push
```

## Uninstalling

```bash
cd ~/dotfiles

# Remove common
stow -D -d common -t ~ zsh

# Remove machine-specific
stow -D -d machines/omarchy -t ~ hypr deskflow portals
```

---

## Related Repositories

- [xdg-desktop-portal-hypr-remote](https://github.com/llkkdb/xdg-desktop-portal-hypr-remote) - RemoteDesktop portal for Hyprland (patched for sdbus-cpp 2.x)

## Last Updated

2025-12-16
