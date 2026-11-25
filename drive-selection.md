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

The drive selection screen consists of 4 panels and a title bar. These values are user selectable via the shortcut keys which are displayed on the shortcut keys help bar on the bottom of the screen. These default values can also be set via the command line, e.g `sudo ./nwipe method=is5enh`. For a full list of command line options run `sudo ./nwipe --help`

**Top Left Panel `Options`**

This panel displays the currently active erasure settings (press the corresponding shortcut keys to modify them before starting a wipe):

* **Entropy:** The kernel randomness source used to seed the pseudo-random number generator (typically Linux `urandom` or `random`, or hardware RNG if available).
* **PRNG:** The Pseudo-Random Number Generator algorithm used to generate random data streams during random overwrite passes (such as Mersenne Twister, ISAAC, XORoshiro-256, SplitMix64, or AES-CTR).
* **Method:** The sanitization algorithm or hardware erase command applied to selected drives—ranging from software overwrite patterns (Zero Fill, One Fill, PRNG Stream, DoD 5220.22-M, HMG IS5, Gutmann) to hardware-native commands (ATA Secure Erase, NVMe Sanitize, SCSI Sanitize).
* **Verify:** Controls whether `nwipe` reads back the drive sectors after writing to confirm the pattern was written accurately:
    * `off`: Skips read-back verification (fastest).
    * `last`: Verifies only the final pass (recommended balance of speed and assurance).
    * `all`: Verifies every individual pass in multi-pass methods.
* **Rounds:** The number of times the selected erasure **Method** is repeated back-to-back on each drive (default is `1`).
* **Blanking:** Controls whether an additional final pass of all zeros (`0x00`) is written after the main erasure rounds finish (`ON` or `OFF`). Leaving the drive zeroed makes it simple to verify erasure with a hex viewer and prepares the disk cleanly for reuse.

---

**Top Right Panel `Statistics`**

This panel show information once the disk or disks being wiped have started.
  * **Runtime:** Total elapsed wall-clock time (`HH:MM:SS`) since the erasure session started.
  * **Remaining:** Estimated time left (`HH:MM:SS`) until all active drives finish their remaining passes and verification rounds, calculated dynamically from current average write/read speeds.
  * **Load Averages:** Standard Linux system load averages over the past **1**, **5**, and **15** minutes, reflecting overall CPU and I/O wait demand (higher values are normal when wiping multiple drives simultaneously using CPU-intensive PRNG algorithms).
  * **Throughput:** Combined real-time data transfer rate across all drives currently being wiped or verified (displayed in `MB/s` or `GB/s`).
  * **Retries/Errors:** Cumulative count of failed I/O operations encountered during the session—retried block writes/reads (typically caused by bad sectors or loose cables) and unrecoverable read/write or verification errors.
  
---

***Main Panel `Disks and Partitions`**

Each line in the drive list displays the following fields from left to right:

* **Cursor & Selection Status:**
    * <span style="background-color: #1589f0; color: #ffffff; font-family: monospace; font-weight: bold; white-space: pre; padding: 1px 5px; border-radius: 2px;">&gt;</span> indicates the currently highlighted row.
    * <span style="background-color: #1589f0; color: #ffffff; font-family: monospace; font-weight: bold; white-space: pre; padding: 1px 5px; border-radius: 2px;">&gt;[WIPE]</span> marks a drive selected for erasure.
    * <span style="background-color: #1589f0; color: #ffffff; font-family: monospace; font-weight: bold; white-space: pre; padding: 1px 5px; border-radius: 2px;">&gt;[&nbsp;&nbsp;&nbsp;&nbsp;]</span> indicates an unselected drive that will be left untouched.
    * <span style="background-color: #1589f0; color: #ffffff; font-family: monospace; font-weight: bold; white-space: pre; padding: 1px 5px; border-radius: 2px;">&gt;[&#45;&#45;&#45;&#45;]</span> indicates a locked/mounted drive that cannot be selected.
* **Device Node:** The Linux kernel device identifier (such as `/dev/sda`, `/dev/sdb`, or `nvme0n1`).
* **Bus / Drive Type:** The storage interface and media category detected by `nwipe` (such as `ATA`, `ATA-SSD`, `NVME`, `USB`, or `SAS`).
* **Capacity:** Total reported drive size in gigabytes (for example, `[  80 GB]` or `[1000 GB]`).
* **Temperature:** Live drive temperature in degrees Celsius read via SMART/NVMe sensors (such as `[32C]`).
* **Hidden Sectors <span style="background-color: #1589f0; color: #ffffff; font-family: monospace; font-weight: bold; white-space: pre; padding: 1px 5px; border-radius: 2px;">&gt;[HS? ...]</span>** Indicates whether a Host Protected Area (HPA) or Device Configuration Overlay (DCO) is present:
    * <span style="background-color: #1589f0; color: #ffffff; font-family: monospace; font-weight: bold; white-space: pre; padding: 1px 5px; border-radius: 2px;">&gt;[HS? NO ]</span> No hidden sectors detected; the full physical capacity is exposed.
    * <span style="background-color: #1589f0; color: #ffffff; font-family: monospace; font-weight: bold; white-space: pre; padding: 1px 5px; border-radius: 2px;">&gt;[HS? YES]</span> Hidden sectors detected (HPA/DCO active). Reset HPA/DCO using hdparm then restart nwipe.
    * <span style="background-color: #1589f0; color: #ffffff; font-family: monospace; font-weight: bold; white-space: pre; padding: 1px 5px; border-radius: 2px;">&gt;[HS? ???]</span> Hidden sector status could not be determined (common on older drives or USB bridges).
    * <span style="background-color: #1589f0; color: #ffffff; font-family: monospace; font-weight: bold; white-space: pre; padding: 1px 5px; border-radius: 2px;">&gt;[HS? N/A]</span> Not applicable to this interface type (such as NVMe).
* **In-Use Flag (`[IN USE]`):** Highlighted in red when the drive or one of its partitions is currently mounted (such as the booted ShredOS USB stick), preventing accidental erasure.
* **Model & Serial Number:** The manufacturer model string followed by the drive's unique hardware serial number (`Model/Serial`).
* **Hardware Sanitize Sub-Line (`└─ Secure Erase: Sanitize is available...`):** Appears directly beneath drives that support native firmware sanitization, letting you press the right arrow key (`→`) to configure hardware-level ATA Secure Erase or NVMe Sanitize.

### 3. Shortcut Keys

**Bottom Shortcut Bar**

Use the following keyboard shortcuts from the main drive selection screen to configure and launch an erasure session:

* **`S` (Start):** Begins wiping all drives marked with `[wipe]` using the currently selected parameters (capital `S` is required to prevent accidental starts).
* **`m` (Method):** Opens the wipe method selection menu to choose the erasure algorithm (such as Zero Fill, PRNG Stream, DoD 5220.22-M, or Gutmann).
* **`p` (PRNG):** Opens the Pseudo-Random Number Generator selection menu to choose the random stream algorithm (such as Mersenne Twister, ISAAC, XORoshiro-256, SplitMix64, or AES-CTR).
* **`v` (Verify):** Opens the verification mode menu to set post-write read-back verification (`off`, `last`, or `all`).
* **`r` (Rounds):** Prompts for the number of consecutive times the selected wipe method should be repeated on each drive.
* **`b` (Blanking):** Toggles the final zero-fill (`0x00`) blanking pass `ON` or `OFF` after the main erasure rounds finish.
* **`d` (Direction):** Toggles the wipe direction between **Forward** (start of disk to end) and **Reverse** (end of disk to start).
* **`e` (Secure Erase):** Opens the hardware-native sanitization menu (ATA Secure Erase / Enhanced Secure Erase or NVMe Sanitize) for the highlighted drive, if supported.
* **`t` (Topology):** Toggles the drive list view between flat device listing and physical bus/controller topology grouping.
* **`Space` (Select):** Toggles the wipe selection state (`[wipe]` or `[    ]`) for the currently highlighted drive.
* **`Ctrl+A` (All):** Selects all available (unlocked) drives in the list for erasure at once.
* **`c` (Config):** Opens the `nwipe` configuration menu to adjust PDF report settings, customer/organization details, and hardware options.
* **`CTRL+C` (Quit):** Aborts any active wipe (if running) and exits `nwipe` back to the ShredOS console.
