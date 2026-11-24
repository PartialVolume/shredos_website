---
layout: page
title: Quick Start Guide
subtitle: Creating a Bootable ShredOS USB Drive & Using Built-In Tools
seo_title: How to Download & Install ShredOS Bootable Disk Wipe USB
---

### 1. Choose the Right Image

Download the official release image from [GitHub Releases](https://github.com/PartialVolume/shredos.x86_64/releases) and verify its `SHA256` checksum:

* **`x86_64` (64-bit):** Recommended for virtually all PCs, laptops, and servers manufactured since 2006 (supports both UEFI and Legacy BIOS).
* **`i686` (32-bit):** For legacy 32-bit CPUs and older BIOS-only hardware.
* **`Lite` versions:** will boot on a system with only 512MB CPU RAM. 

---

### 2. Flash ShredOS to a USB Drive

#### Linux (`dd`)
Identify your USB device with `lsblk` (e.g., `/dev/sdX`) and write the image directly:

```
sudo dd status=progress if=shredos-*.img of=/dev/sdX
```

#### Windows (Rufus or balenaEtcher)
1. Open **Rufus** or **balenaEtcher**.
2. Select the downloaded `shredos-*.img` file.
3. Select your target USB flash drive and click **Start / Flash**.

#### Ventoy (Multi-Boot USB)
ShredOS supports **Ventoy** out of the box. Copy the `.img` or `.iso` file directly onto your Ventoy data partition—no extraction or reflashing required. As with flashed versions of .iso and .img files, the PDF files and logs are saved to the root `/` of the Ventoy Flash drive.

---

### 3. Booting & Keyboard Shortcuts

1. Insert the USB drive and open your system's boot menu (commonly `F12`, `F11`, `F8`, or `ESC` at power-on).
2. Select the USB device. ShredOS boots into RAM and launches **nwipe** automatically.
3. Use the <kbd>↑</kbd> and <kbd>↓</kbd> arrow keys and <kbd>Space</kbd> in `nwipe` to select target drives, press `M` to choose your wipe method, and press `S` to start.

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

When flashed directly to USB, ShredOS creates a FAT32 boot partition containing `/etc/nwipe/nwipe.conf`. You can customize:

* **Automated wipes (`autonuke`):** Configure `nwipe` to wipe all drives automatically on boot without keyboard interaction.
* **Custom PDF Branding:** Replace `/etc/nwipe/logo.jpg` on the USB stick to include your organization's logo on generated PDF erasure certificates.
* **Persistent Certificates:** By default, PDF erasure certificates and logs are written directly to the FAT32 partition on your ShredOS USB drive when all drives have finished and you have pressed the return key to generate the reports. 
