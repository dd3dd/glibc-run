# glibc-run

> ⚠️ **DISCLAIMER:** This is my personal script written for a specific setup (Gentoo Musl + LLVM). Paths like `/mnt/glibc` are hardcoded for my system. This script is shared purely for reference or as a starting point for your own custom solutions. I DO NOT guarantee that it will work on Void, Alpine, or Chimera without manually tweaking the paths. No complaints accepted, but pull requests are welcome!

A collection of lightweight, high-performance solutions designed to run Glibc binaries (including Steam, modern games, and clients like AyuGram) seamlessly on Musl-based Linux distributions (such as Gentoo, Void, or Alpine Linux) without relying on heavy Flatpaks.

---

## 🛠️ Choose Your Flavor

This repository offers two independent implementations depending on your philosophy and setup:

### 1. `bwrap/` (Bubblewrap Sandbox)
* **How it works:** Leverages `bubblewrap` to create an unprivileged user namespace sandbox.
* **Pros:** Highly secure, runs entirely from a normal user (`no root/doas required`), protects host from malicious binaries.
* **Best for:** Gaming (Steam/Proton), web browsers, and untrusted closed-source apps.

### 2. `pure-sh/` (Pure POSIX sh + Namespaces)
* **How it works:** Uses standard Linux kernel namespaces via `unshare -m -i` combined with a classic `chroot`.
* **Pros:** **Zero external dependencies**, pure Unix-way. Uses the kernel's native Mount Namespace, meaning all mounts automatically disappear when the app closes (no host pollution). 
* **Best for:** Minimalist purists, suckless fans, and trusted apps built from source.

---

## 🌟 Shared Features

* **Isolated Home Directory:** Both versions automatically isolate application data inside a dedicated directory (`~/.local/share/glibc_sandbox/` or `~/.local/share/glibc-box-home/`) to keep your host `$HOME` clean.
* **Hardware Acceleration:** Full 3D/DRM performance via direct forwarding of `/dev/dri`, `/dev/shm`, and inputs.
* **Display Server Support:** Seamless X11 and Wayland socket forwarding for GUI applications.
* **Isolated D-Bus:** Launches an independent D-Bus session inside the container environment to prevent sandbox escapes while ensuring app stability.

---

 ## 🧪 Verified Applications

## This setup has been successfully tested on Gentoo (Musl/LLVM) for:

    Steam

    Discord

    Obsidian

    Prism Launcher

Note: Since this tool is environment-agnostic, it should work with any compatible glibc-based rootfs.
⚠️ Status & Contributing

This project is currently my daily driver.

##    Tested on: Gentoo (Musl/LLVM).

##    Other distributions: It should work on any Linux distribution with bwrap/unshare. If you encounter issues on your distro, feel free to open an issue or submit a Pull Request!

Feedback and PRs are very welcome.

---

## 📂 Repository Structure

```text
glibc-run/
├── bwrap/
│   └── glibc-run        # Bubblewrap-based runner (No root)
├── pure-sh/
│   └── glibc-run        # unshare + chroot runner (Zero dependencies)
└── README.md

🚀 Installation & Usage
1. Prerequisites

    A working Glibc root environment at /mnt/glibc (e.g., a minimal Gentoo Glibc stage3, Arch Linux bootstrap, or Debian chroot).

    For the bwrap version: bubblewrap installed on the host.

    For the pure-sh version: doas or sudo configured on the host.

2. Installing the Bubblewrap variant:

cp bwrap/glibc-run /usr/local/bin/glibc-run
chmod +x /usr/local/bin/glibc-run

3. Installing the Pure Namespace variant:

cp pure-sh/glibc-run /usr/local/bin/glibc-run-pure
chmod +x /usr/local/bin/glibc-run-pure

4. Running a binary:

glibc-run /path/to/glibc/binary [arguments]
# OR #
glibc-run-pure /path/to/glibc/binary [arguments]
