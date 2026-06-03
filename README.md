# glibc-run

A lightweight, highly secure Bubblewrap-based sandbox designed to run Glibc binaries (including Steam and modern games) seamlessly on Musl-based Linux distributions (like Gentoo or Void Linux).

Instead of relying on heavy Flatpaks or insecure chroots, this script leverages Linux Namespaces to create a fully isolated environment for applications that hardcode Glibc dependencies.

## Features //

* **Strict Isolation:** Uses Linux Namespaces (`--unshare-all`) and strips all root privileges (`--cap-drop ALL`).
* **Sandbox Home:** Automatically creates and isolates separate home directories for each app under `~/.local/share/glibc_sandbox/` to keep your host clean.
* **Hardware Acceleration:** Native 3D performance via direct forwarding of `/dev/dri`, `/dev/shm`, and input devices.
* **Display Server Support:** Seamlessly forwards both X11 and Wayland sockets for GUI applications.
* **Steam & Proton Ready:** Robust enough to handle nested namespaces used by Steam's internal container runtime (*Pressure-Vessel*).
* **Isolated D-Bus:** Launches an independent D-Bus session inside the container to prevent sandbox escapes while ensuring app stability.

## Prerequisites //

1. **Bubblewrap:** Make sure `bwrap` is installed on your host system.
2. **Glibc Environment:** A working Glibc root environment at `/mnt/glibc` (e.g., a minimal Gentoo Glibc stage3 or Arch Linux bootstrap).

## Installation & Usage // 

1. Copy the script to your local path:
```bash
   cp glibc-run /usr/local/bin/glibc-run
   chmod +x /usr/local/bin/glibc-run

