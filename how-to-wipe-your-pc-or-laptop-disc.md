---
layout: page
title: When to Wipe a Disc
subtitle: Deleting vs. Formatting vs. Permanently Wiping Your PC or Laptop Drive
seo_title: How Do I Wipe My PC or Laptop Disc? | ShredOS Guid
---

# How Do I Wipe My PC or Laptop Disc?

If you are planning to sell, donate, recycle, or reinstall a fresh operating system on your computer, you have probably wondered: **"How do I wipe my PC or laptop disc?"**

Simply dragging files to the Recycle Bin or running a standard format does **not** permanently erase your personal data. To make sure your private documents, passwords, browser history, and old operating system are completely unrecoverable, you need to **wipe** the physical drive.

Below is a clear breakdown of the differences between deleting, formatting, and wiping, followed by a step-by-step guide on how **ShredOS** wipes your drive completely clean.

---

## Deleting vs. Formatting vs. Wiping: What’s the Difference?

Many people assume these three terms mean the same thing, but they handle your data in completely different ways:

| Action | What It Actually Does | Is Data Recoverable? |
| :--- | :--- | :--- |
| **Deleting Files** | Removes the file's entry from the file system index (like tearing a page out of a book's table of contents). The actual data remains untouched on the disc until new files eventually overwrite those sectors. | **Yes** — easily recovered using standard file-recovery software. |
| **Formatting a Disc** | Builds a fresh, empty file system (such as NTFS, ext4, or APFS) over the partition. A **Quick Format** only clears the index table; even a full format in modern OS installers may leave unallocated areas or SSD blocks intact. | **Yes** — partition recovery and forensic tools can still extract raw data. |
| **Wiping a Disc** | Overwrites every single sector across the entire drive—including hidden partitions, boot sectors, and free space—with zeroes, random data, or industry-standard patterns, or triggers the drive's built-in hardware cryptographic/block erase. | **No** — once overwritten or hardware-sanitized, the original data is permanently destroyed. |

---

## Why Use ShredOS to Wipe Your Drive?

You cannot completely erase a hard drive or SSD while the operating system installed on it is actively running—Windows, Linux, and macOS lock the system files they need to stay booted.

**ShredOS** solves this by running entirely from your computer's **RAM (memory)** via a bootable USB flash drive. Because ShredOS never mounts or depends on your internal hard drive or SSD:

* **Complete Access:** It bypasses operating system file locks, hidden recovery partitions, and OS restrictions.
* **Erases Everything:** Powered by `nwipe`, ShredOS wipes the entire physical block device—your personal files, user profiles, bootloaders, partition tables, and the operating system itself.
* **Supports Modern & Legacy Hardware:** Works across NVMe SSDs, SATA SSDs, traditional spinning hard drives (HDDs), eMMC storage, and USB drives.
* **Built-In Hardware Sanitize & Verification:** Supports multi-pass software overwrites (such as Zero Fill, PRNG Stream, DoD 5220.22-M, and HMG IS5) as well as interactive hardware-native **ATA/NVMe Sanitize** commands directly from the interface, followed by optional read-back verification and PDF erasure certificates.

---

## Step-by-Step: How to Wipe Your PC or Laptop with ShredOS

> **Warning:** Wiping a drive is permanent and irreversible. Back up any photos, documents, or license keys you want to keep before starting, and unplug any external USB backup drives so you do not accidentally select them.

### 1. Download ShredOS
Download the latest 64-bit (`x86_64`) `.img`  or `.iso` release of ShredOS from the [ShredOS Releases page](https://github.com/PartialVolume/shredos.x86_64/releases) (or the older 32-bit `i686` build if you are wiping vintage hardware).

### 2. Create a Bootable ShredOS USB Drive
Flash the ShredOS `.img` or `.iso` file onto a spare USB stick using a standard imaging tool:
* **Windows:** Use [Rufus](https://rufus.ie/) or [balenaEtcher](https://etcher.balena.io/).
* **macOS / Linux:** Use balenaEtcher, Raspberry Pi Imager, or `dd` from the terminal:
  ```bash
  sudo dd if=shredos.img of=/dev/sdX bs=4M status=progress conv=fsync
  ```
  *(Replace `/dev/sdX` with your actual USB stick device path).*
* **Ventoy:** Alternatively, copy the .iso or.img file to a [Ventoy](https://www.ventoy.net/) USB drive

### 3. Boot Your PC or Laptop from the USB Drive
1. Insert the ShredOS USB drive into the computer you want to wipe.
2. Power on or restart the machine and press your manufacturer's **Boot Menu key** (commonly `F12`, `F11`, `F8`, `F2`, `Esc`, or `Option ⌥` on Intel Macs) before the operating system loads.
3. **Note on UEFI & Secure Boot:** ShredOS supports both Legacy BIOS and modern UEFI booting, **but Secure Boot must be disabled** in your BIOS/UEFI settings before booting from the USB stick.
4. Select your USB drive from the boot menu. ShredOS will load into memory and launch `nwipe` automatically.

### 4. Select Your Drive and Choose a Wipe Method
1. Use the **Up / Down arrow keys** to highlight the internal drive(s) you want to wipe, and press **`Space`** to select them (a `WIPE` tag will appear next to selected drives). Double-check the model name, serial number, and capacity so you do not select your ShredOS USB stick.
2. Press **`M`** to choose your wipe method:
   * **Zero Fill (`zero`)** or **PRNG Stream (`prng`)**: A single pass of zeroes or cryptographically random data is fast, thorough, and recommended for modern drives.
   * **Hardware Sanitize (`e` or right arrow (`→`) key / Sanitize menu)**: Ideal for modern SATA and NVMe SSDs, instructing the drive's controller to perform a native block erase or cryptographic scramble across all flash cells, including wear-leveling spare blocks.
3. Press **`V`** if you want to adjust post-wipe verification (verifying the last pass or all passes) or disable blanking. See [Device Selection](https://shredos.com/drive-selection.html) for detailed device selection user interface and user selectable options.

### 5. Start the Wipe
Press **`Shift s (capital S)`** to begin wiping the selected drive(s).

ShredOS will display real-time throughput speed, drive temperature, elapsed time, and estimated time remaining. Depending on drive capacity and interface speed, a wipe can take anywhere from a few seconds (using NVMe Sanitize) to several hours (for multi-terabyte mechanical hard drives).

---

## What Happens After the Wipe? Ready for Sale or Reinstallation

Once ShredOS finishes and reports that the wipe succeeded, your disc is completely blank—there is no partition table, no file system, and no operating system left on the drive.

* **If you are selling, donating, or recycling the PC:** Simply power off the machine and remove your ShredOS USB drive. If the next owner turns it on, they will see a *"No bootable device found"* message, confirming the drive is completely empty.
* **If you are reinstalling Windows, Linux, or macOS:** Boot your computer from your Windows USB installer, Linux Live USB, or macOS Recovery media. Because the drive is completely unallocated, the OS installer will automatically partition, format, and install a clean copy of the operating system onto the freshly wiped disc.
