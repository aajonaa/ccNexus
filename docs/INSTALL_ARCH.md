# Installing ccNexus on Arch Linux

This guide documents the steps to build and install ccNexus desktop application on Arch Linux.

## Prerequisites

### 1. Go (Golang)

Ensure Go is installed:

```bash
sudo pacman -S go
```

Verify installation:

```bash
go version
```

### 2. System Dependencies

ccNexus uses Wails v2 which requires GTK and WebKit2GTK:

```bash
sudo pacman -S gtk3 webkit2gtk
```

> **Note:** The package `webkit2gtk` provides `webkit2gtk-4.0` which is required by Wails. Do not confuse with `webkit2gtk-4.1` which is a different package.

### 3. Wails CLI

Install the Wails CLI tool:

```bash
go install github.com/wailsapp/wails/v2/cmd/wails@latest
```

Add Go bin to your PATH (add to `~/.bashrc` or `~/.zshrc`):

```bash
export PATH=$PATH:$(go env GOPATH)/bin
```

## Building ccNexus

### 1. Clone the Repository

```bash
git clone https://github.com/lich0821/ccNexus.git
cd ccNexus
```

### 2. Install Frontend Dependencies

```bash
cd cmd/desktop/frontend
npm install
cd ..
```

### 3. Build the Application

```bash
cd cmd/desktop
wails build
```

The built binary will be located at:

```
cmd/desktop/build/bin/ccNexus
```

## Installing to Application Menu

To make ccNexus appear in your application launcher, create a desktop entry:

### 1. Create Desktop Entry

Create the file `~/.local/share/applications/ccNexus.desktop`:

```ini
[Desktop Entry]
Name=ccNexus
Comment=A smart API endpoint rotation proxy for Claude Code
Exec=/path/to/ccNexus/cmd/desktop/build/bin/ccNexus
Icon=/path/to/ccNexus/cmd/desktop/build/appicon.png
Type=Application
Categories=Development;Utility;
Terminal=false
StartupWMClass=ccNexus
```

> **Note:** Replace `/path/to/ccNexus` with your actual installation path.

### 2. Update Desktop Database

```bash
update-desktop-database ~/.local/share/applications/
```

### 3. Refresh Application Menu

Either:
- Log out and log back in
- Restart your desktop shell (GNOME: `Alt+F2` then type `r`)
- Wait for the menu to auto-refresh

## Troubleshooting

### Package webkit2gtk-4.0 not found

If you see this error:

```
Package 'webkit2gtk-4.0' not found
```

Install the correct package:

```bash
sudo pacman -S webkit2gtk
```

### Mirror sync issues

If package download fails with 404 errors, sync the package database:

```bash
sudo pacman -Syyu webkit2gtk
```

### Wails CLI not found

Ensure Go bin directory is in your PATH:

```bash
export PATH=$PATH:$(go env GOPATH)/bin
```

## Uninstalling

1. Remove the desktop entry:

```bash
rm ~/.local/share/applications/ccNexus.desktop
update-desktop-database ~/.local/share/applications/
```

2. Remove the built binary and source (optional):

```bash
rm -rf /path/to/ccNexus
```
