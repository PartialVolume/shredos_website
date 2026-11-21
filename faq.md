---
layout: page
title: FAQ & Hardware Compatibility
subtitle: Common Questions About NVMe, SSDs, RAID Controllers, and Erasure Verification
---

### Is `shredos.net` affiliated with ShredOS?
**No.** `shredos.net` is an unofficial third-party site that scrapes release notes and serves advertisements. The only official ShredOS resources are **`https://shredos.com`** and **`https://github.com/PartialVolume/shredos.x86_64`**.

---

### How do I wipe NVMe and SATA SSDs properly?
Modern SSDs and NVMe drives use wear-leveling and over-provisioning, meaning standard block-by-block software overwrites may not reach retired flash blocks. ShredOS supports **Hardware-Native Sanitize** directly inside `nwipe`, triggering the drive controller's own firmware sanitization routines:

* **NVMe Sanitize / Format NVM:** Block Erase and Cryptographic Erase.
* **ATA Sanitize / Enhanced Secure Erase:** Controller-level flash block erase and cryptographic key destruction.

You can also run a software pass (such as Zero Fill or PRNG Stream) or inspect raw sectors afterward using **`shredos_hexviewer`** on `ALT + F5`.

---

### Why isn't my drive showing up in `nwipe`?
1. **RAID Mode / Intel VMD:** Many laptops and workstations ship with SATA/NVMe controllers set to **RAID** or **Intel RST / VMD** in the BIOS/UEFI. Switch the storage controller mode to **AHCI** (or disable VMD) so the Linux kernel can address each physical drive directly.
2. **Hardware RAID Cards:** Enterprise RAID controllers (such as Dell PERC or HP Smart Array) hide individual disks behind virtual volumes unless flashed to **IT Mode (HBA)** or configured for single-disk JBOD passthrough.
3. **USB Bridges:** Some cheap USB-to-SATA enclosures block SMART and ATA passthrough commands. For hardware-native Secure Erase, connect drives directly to motherboard SATA/NVMe ports or an HBA.

---

### What about Host Protected Areas (HPA) and Device Configuration Overlays (DCO)?
`nwipe` automatically detects and reports HPA/DCO hidden sectors on ATA/SATA drives. You can also inspect or manage HPA/DCO settings manually from the root shell on `ALT + F2` using `hdparm -N /dev/sdX`.

---

### Can I boot ShredOS over the network (PXE)?
Yes. ShredOS runs entirely in RAM from a single `bzImage` kernel with an embedded initramfs. Extract `bzImage` from the `.img` boot partition and serve it via **iPXE**, **PXELINUX**, or **netboot.xyz**—no NFS or HTTP root filesystem mount is required.
