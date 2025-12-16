# Omarchy Hyprland Dotfiles

Personal configuration files for Omarchy (Arch Linux) with Hyprland.

## System Information

| Component | Version |
|-----------|---------|
| OS | Omarchy 3.2.3 |
| Kernel | Linux 6.17.9-arch1-1 |
| Desktop | Hyprland 0.52.2 (Wayland) |
| Shell | zsh |

## What's Included

### User Configurations (managed with GNU Stow)

| Package | Description |
|---------|-------------|
| `hypr` | Hyprland window manager configs |
| `deskflow` | Deskflow KVM client settings |
| `portals` | XDG Desktop Portal routing |

### System Files (require sudo to install)

| File | Purpose |
|------|---------|
| `system/usr/share/xdg-desktop-portal/portals/hypr-remote.portal` | Portal config for RemoteDesktop |
| `system/usr/share/dbus-1/services/org.freedesktop.impl.portal.desktop.hypr-remote.service` | D-Bus activation service |

## Prerequisites

```bash
# Install GNU Stow
sudo pacman -S stow

# Install Deskflow
sudo pacman -S deskflow

# Build xdg-desktop-portal-hypr-remote (see below)
```

## Installation

### 1. Clone this repository

```bash
git clone https://github.com/llkkdb/dotfiles.git ~/dotfiles
cd ~/dotfiles
```

### 2. Install user configs with Stow

```bash
# Install all packages
stow hypr deskflow portals

# Or install individually
stow hypr      # Hyprland configs
stow deskflow  # Deskflow settings
stow portals   # XDG portal routing
```

### 3. Install system files (requires sudo)

```bash
# Install portal config
sudo cp system/usr/share/xdg-desktop-portal/portals/hypr-remote.portal \
    /usr/share/xdg-desktop-portal/portals/

# Install D-Bus service
sudo cp system/usr/share/dbus-1/services/org.freedesktop.impl.portal.desktop.hypr-remote.service \
    /usr/share/dbus-1/services/
```

### 4. Build and install xdg-desktop-portal-hypr-remote

This is required for Deskflow to work on Wayland/Hyprland.

```bash
# Clone the patched repo (with sdbus-cpp 2.x compatibility)
git clone https://github.com/llkkdb/xdg-desktop-portal-hypr-remote.git
cd xdg-desktop-portal-hypr-remote

# Build
mkdir -p build && cd build
cmake ..
make

# Install
sudo cp xdg-desktop-portal-hypr-remote /usr/lib/

# Restart portal
pkill xdg-desktop-portal
systemctl --user restart xdg-desktop-portal
```

### 5. Verify installation

```bash
# Check RemoteDesktop portal is available
gdbus introspect --session --dest org.freedesktop.portal.Desktop \
    --object-path /org/freedesktop/portal/desktop | grep RemoteDesktop
```

## Deskflow Setup

This configuration uses Deskflow as a **client** connecting to a Windows/Barrier server.

- **Server IP:** 192.168.8.206 (configured in Deskflow.conf)
- **Auto-start:** Enabled via `hypr/.config/hypr/autostart.conf`
- **Mode:** Client (headless, auto-connect)

### Changing the server IP

Edit `~/.config/Deskflow/Deskflow.conf`:
```ini
[client]
remoteHost=YOUR_SERVER_IP
```

## Hyprland Configs

| File | Purpose |
|------|---------|
| `hyprland.conf` | Main config (sources other files) |
| `autostart.conf` | Auto-start applications (Deskflow) |
| `bindings.conf` | Keyboard shortcuts |
| `monitors.conf` | Display configuration |
| `input.conf` | Keyboard/mouse settings |
| `looknfeel.conf` | Appearance settings |
| `hypridle.conf` | Idle behavior |
| `hyprlock.conf` | Lock screen settings |
| `hyprsunset.conf` | Night light settings |
| `xdph.conf` | Desktop portal settings |

## Uninstalling

```bash
# Remove stow symlinks
cd ~/dotfiles
stow -D hypr deskflow portals

# Remove system files
sudo rm /usr/share/xdg-desktop-portal/portals/hypr-remote.portal
sudo rm /usr/share/dbus-1/services/org.freedesktop.impl.portal.desktop.hypr-remote.service
sudo rm /usr/lib/xdg-desktop-portal-hypr-remote
```

## Notes

- Deskflow TLS certificates are NOT included (regenerated on first run)
- The `xdg-desktop-portal-hypr-remote` binary must be built from source due to sdbus-cpp 2.x API changes
- Running Deskflow as a **server** on Hyprland is NOT supported (requires InputCapture portal)

## Related Repositories

- [xdg-desktop-portal-hypr-remote (patched)](https://github.com/llkkdb/xdg-desktop-portal-hypr-remote) - RemoteDesktop portal for Hyprland

## Last Updated

2025-12-16
