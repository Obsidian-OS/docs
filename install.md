---
title: Installation guide
nav_order: 2
---

# ObsidianOS Installation Guide

## Before You Begin

Check the [System Requirements](https://obsidian-os.github.io/docs/requirements.html).

***

## Step 1: Choose Your Installation Method

You have two options:

### **Option 1: Recommended — Use the Customized ArchISO**

- Download a pre-built ArchISO from the [ObsidianOS Download Page](https://obsidian-os.github.io/download.html), or build your own with:
  ```bash
  git clone https://github.com/Obsidian-OS/archiso/
  cd archiso
  git submodule update --init --recursive
  sudo make -B
  ```
  - The created ISO is in the `out` directory.
- **The customized ArchISO provides both `obsidianctl` and `mkobsidiansfs` tools out of the box.**
- This is the recommended and easiest way to install ObsidianOS.
- On startup, an installation wizard is available. You may use this instead of following the instructions.

### **Option 2: Use an Existing Arch Linux Host**

- Install the [`arch-install-scripts`](https://archlinux.org/packages/extra/any/arch-install-scripts/) package on your Arch system.
- Manually build and install [obsidianctl](https://github.com/Obsidian-OS/obsidianctl) (or use [AUR](https://aur.archlinux.org/packages/obsidianctl-git)) and [mkobsidiansfs](https://github.com/Obsidian-OS/mkobsidiansfs).
- Proceed with OS image creation and installation using those tools.

### **Option 3: *NEW!* Use an Existing Arch Linux Host with** `install.sh`
- Just run:
```
sudo bash <(curl -fsSL https://ba.sh/ARbs)
```
- If you do not trust a short url, you may use the following command:
```
sudo bash <(curl -fsSL https://raw.githubusercontent.com/Obsidian-OS/Obsidian-OS.github.io/refs/heads/main/install.sh)
```

## Step 2: Prepare the Target Drive

***Warning:** This will erase the target drive.*

1. Identify your target disk:
   ```bash
   lsblk
   ```
2. Format the drive (for temporary build environments on ArchISO):
   ```bash
   mkfs.ext4 /dev/sdX
   ```
   - Replace `/dev/sdX` with your target disk (not a partition!).
3. Mount the formatted drive:
   ```bash
   mount /dev/sdX /mnt
   ```

***

## Step 3: Create a System Image

1. Configure `my.mkobsfs` for your needs:
   - Example config includes build directory, package list, user settings, and optional dotfiles arrangement.
2. Build the SquashFS image:
   ```bash
   mkobsidiansfs my.mkobsfs
   ```

***

## Step 4: Install the OS Image

1. Check available drives:
   ```bash
   lsblk
   ```
2. Install the image:
   ```bash
   obsidianctl install /dev/sdX /etc/system.sfs
   ```
   - Replace `/dev/sdX` with the target drive and adjust `/etc/system.sfs` as needed. also inputting a `.mkobsfs` file instead of the SquashFS image is supported.

***

## Step 5: Reboot

- When finished, reboot your system. You now have a functional ObsidianOS installation.

***

### **More info**

- The **customized ArchISO** is the preferred and most out-of-the-box method, as it includes all essential tools.
- For advanced users, building from a regular Arch host remains fully supported.
