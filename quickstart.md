---
layout: page
title: Quick Start Guide
subtitle: Creating a Bootable ShredOS USB Drive & Using Built-In Tools
head-title: How to Download & Install ShredOS Bootable Disk Wipe USB
---

### 1. Choose the Right Image

Download the official release image from [GitHub Releases](https://github.com/PartialVolume/shredos.x86_64/releases) and verify its `SHA256` checksum:

* **`x86_64` (64-bit):** Recommended for virtually all PCs, laptops, and servers manufactured since 2006 (supports both UEFI and Legacy BIOS).
* **`i686` (32-bit):** For legacy 32-bit CPUs and older BIOS-only hardware.

---

### 2. Flash ShredOS to a USB Drive

#### Linux (`dd`)
Identify your USB device with `lsblk` (e.g., `/dev/sdX`) and write the image directly:

```bash
sudo dd if=shredos-*.img of=/dev/sdX bs=4M status=progress conv=fsync
```

#### Windows (Rufus or balenaEtcher)
1. Open **Rufus** or **balenaEtcher**.
2. Select the downloaded `shredos-*.img` file.
3. Select your target USB flash drive and click **Start / Flash**.

#### Ventoy (Multi-Boot USB)
ShredOS supports **Ventoy** out of the box. Copy the `.img` file directly onto your Ventoy data partition—no extraction or reflashing required. Note that when booted via Ventoy, PDF certificates and logs are saved inside `/etc/nwipe` in RAM rather than back to the Ventoy partition.

---

### 3. Booting & Keyboard Shortcuts

1. Insert the USB drive and open your system's boot menu (commonly `F12`, `F11`, `F8`, or `ESC` at power-on).
2. Select the USB device. ShredOS boots into RAM and launches **nwipe** automatically.
3. Use the arrow keys and `Space` in `nwipe` to select target drives, press `M` to choose your wipe method (or `H` for hardware-native sanitize), and press `F10` to start.

#### Virtual Consoles (`ALT + F1` through `ALT + F5`)

| Shortcut | Console / Utility |
| :--- | :--- |
| **`ALT + F1`** | Primary **nwipe** erasure interface |
| **`ALT + F2`** | Root shell (`smartctl`, `hdparm`, `nvme-cli`, `lsblk`, `dmesg`) |
| **`ALT + F3`** | System and kernel log output |
| **`ALT + F4`** | **`SmartDMT`** - Smart monitoring terminal |
| **`ALT + F5`** | **`shredos_hexviewer`** — interactive raw hexadecimal & ASCII sector inspector |

---

### 4. Unattended / Automated Wiping (`nwipe.conf`)

When flashed directly to USB, ShredOS creates a FAT32 boot partition containing `/boot/nwipe.conf` and `/etc/nwipe/`. You can customize:

* **Automated wipes (`autonuke`):** Configure `nwipe` to wipe all non-USB drives automatically on boot without keyboard interaction.
* **Custom PDF Branding:** Replace `/etc/nwipe/logo.jpg` on the USB stick to include your organization's logo on generated PDF erasure certificates.
* **Persistent Certificates:** By default, PDF erasure certificates and logs are written directly to the FAT32 partition on your ShredOS USB drive as each drive finishes.
