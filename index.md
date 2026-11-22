---
layout: home
title: ShredOS
subtitle: Minimal, Open-Source Bootable Disc Wipe Linux Distribution for Secure Disk Erasure and Sanitation
---

**ShredOS** is a lightweight, fast-booting Linux distribution built specifically to securely erase hard drives, SSDs, and NVMe storage devices. Designed as a modern, actively maintained successor to DBAN, ShredOS boots directly into **nwipe** in seconds from a USB flash drive or PXE network environment—supporting both legacy BIOS and modern UEFI systems.

> **Official Source Notice:** `shredos.com` and [github.com/PartialVolume/shredos.x86_64](https://github.com/PartialVolume/shredos.x86_64) are the only official homes of ShredOS. Always verify SHA256 checksums and download images exclusively from the official GitHub Releases page.

[Download Latest Release on GitHub](https://github.com/PartialVolume/shredos.x86_64/releases){: .btn .btn-success .btn-lg}
[View Source Code](https://github.com/PartialVolume/shredos.x86_64){: .btn .btn-default .btn-lg}

---

### Why Choose ShredOS Disk Wipe Software??

| Feature | Details |
| :--- | :--- |
| **Wipe Engine** | Boots directly into **nwipe** with an interactive ncurses interface or fully unattended `nwipe.conf` automation. |
| **Erasure Standards** | DoD 5220.22-M, Gutmann, PRNG Stream, Random, Zero Fill, Verify Only, plus **Hardware-Native Sanitize** (ATA Secure Erase & NVMe Block/Crypto Erase). |
| **Broad Hardware Support** | Full support for NVMe, SATA, PATA/IDE, SAS, SCSI, USB enclosures, and enterprise HBAs / Intel VMD controllers. |
| **Audit-Ready Certificates** | Automatically writes detailed PDF erasure certificates (with speed and temperature graphs) and JSON/text logs back to the boot USB drive. |
| **Built-In Diagnostics** | Multi-console tools (`ALT+F1` to `ALT+F5`) including raw hex disk inspection (`shredos_hexviewer`), `smartctl`, and `hdparm` HPA/DCO checks. |
| **100% Free & Open Source** | Licensed under GPLv2. No ads, no telemetry, no nagware, and no artificial drive size limits. |

---

### Quick Start in 3 Steps

1. **Download the Image:** Grab the latest `.img` file (64-bit `x86_64` or legacy 32-bit `i686`) from the [GitHub Releases](https://github.com/PartialVolume/shredos.x86_64/releases) page.
2. **Flash to USB:** Write the image directly to a USB stick using `dd`, Rufus, balenaEtcher, or copy it onto a **Ventoy** drive.
3. **Boot & Wipe:** Boot the target machine from USB. ShredOS loads into RAM and launches `nwipe` automatically—you can even unplug the USB stick once booted if you don't need logs saved back to it.

---

### Contribute to This Site

Notice outdated documentation or want to add hardware compatibility notes? Every page on `shredos.com` is open source—click **"Edit this page"** below to submit a pull request on GitHub.
