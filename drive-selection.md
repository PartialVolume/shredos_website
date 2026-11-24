---
layout: page
title: Drive Selection
subtitle: ShredOS drive selection and shortcut keys
---

### 1. Overview
Use the drive selection screen to choose which disks to wipe and configure your erasure settings. If you are unsure which settings to pick, leave the defaults as they are—these work well for modern SATA, IDE, and SAS drives (post-2000, non-MFM). SSDs and NVMe drives are covered separately, as they have specific requirements that must be addressed before running a traditional block erasure. Preferably, use a hardware sanitize (purge) method on SSD and NVMe drives, as detailed later on this page..

Enabling a specific drive is achieved by using the <kbd>↑</kbd> and <kbd>↓</kbd> arrow keys and then press <kbd>Space</kbd> to enable the drive for wiping. <span style="background-color: #1589f0; color: #ffffff; font-family: monospace; font-weight: bold; padding: 1px 5px; border-radius: 2px;">&gt;[wipe]</span> will be displayed at the start of the drive line which indicates this specific drive will be wiped upon starting the wipe by using shift-s (capital S). If the drive supports native sanitize and you wish to sanitize (purge) the drive use the up/down arrow keys to highlight the drive then press the right arrow key. As of nwipe v0.43 only one drive can be sanitized at a time unlike traditional block writing where multiple drives can be simultaneously erased.

- <span style="background-color: #1589f0; color: #ffffff; font-family: monospace; font-weight: bold; white-space: pre; padding: 1px 5px; border-radius: 2px;">&gt;[wipe]</span> = Drive will be wiped.
- <span style="background-color: #1589f0; color: #ffffff; font-family: monospace; font-weight: bold; white-space: pre; padding: 1px 5px; border-radius: 2px;">&gt;[&nbsp;&nbsp;&nbsp;&nbsp;]</span> = Drive will not be wiped.
- <span style="background-color: #1589f0; color: #ffffff; font-family: monospace; font-weight: bold; white-space: pre; padding: 1px 5px; border-radius: 2px;">&gt;[&#45;&#45;&#45;&#45;]</span> = Drive is mounted and can't be selected for wiping.

Drives currently mounted by the operating system are flagged as <span style="background-color: #ffffff; color: #e01b24; font-family: monospace; font-weight: bold; padding: 1px 5px; border: 1px solid #cccccc; border-radius: 2px;">[IN USE]</span> and cannot be wiped until unmounted. This is to avoid accidently overwriting a mounted drive when nwipe is being run on a linux distribution. You normally won't see this warning as ShredOS operates in RAM as a disc less system and no drives are mounted at boot. If you are using nwipe on a linux distro you can overide this prevention measure by using the --force option when executing nwipe. e.g. `sudo ./nwipe --force`

![nwipe drive selection screen in ShredOS](/assets/img/nwipe-drive-selection.png)

### 2. Drive Selection Panel Descriptions

The drive selection screen consists of 4 panels and a title bar.
* **Top Left Panel `Options`** This panel contains the currently selected settings for `entropy` type, `PRNG` type (psuedo random number generator), `Method` type, `Verify` off/last/all, number of `rounds` and whether final `blanking` (writing zeros) is switched on or off. Each of these will be described below. These values are user selectable via the shortcut keys which are displayed on the shortcut keys help bar on the bottom of the screen. These default values can also be set via the command line, e.g `sudo ./nwipe method=is5enh`. For a full list of command line options run `sudo ./nwipe --help`

* **Top Right Panel `Statistics`** This panel show information once the disk or disks being wiped have started.

### 3. Shortcut Keys

