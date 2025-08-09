---
title: Installation guide
---

# ObsidianOS Installation Guide
## Getting ready
You may either:
1. Boot off the [customized archiso](https://github.com/Obsidian-OS/archiso/). or,
2. Install the [arch-install-scripts](https://archlinux.org/packages/extra/any/arch-install-scripts/) package on an arch system, build and install [obsidianctl](https://github.com/Obsidian-OS/obsidianctl) and [mkobsidiansfs](https://github.com/Obsidian-OS/mkobsidiansfs).
## Guided installer
This installer is available on the customized archiso and helps install ObsidianOS. It is possible to use this on an normal system by [installing it on an arch host](https://github.com/Obsidian-OS/guidedobsidian). To use it, just type in `guidedobsidian`.
Please note that if you are not on the archiso then the prebuilt option will **not work**.
## Manual install
### Creating a system image
Modify the following example to suit your preferences:
```
# --- System creation ---
BUILD_DIR="obsidian_rootfs" # SquashFS generation directory
PACKAGES="base linux linux-firmware networkmanager sudo vim nano efibootmgr python squashfs-tools arch-install-scripts base-devel" # Packages to install
OUTPUT_SFS="system.sfs" # Output SquashFS
TIMEZONE="America/New_York" # Olson Timezone
HOSTNAME="obsidian" # Hostname
CUSTOM_SCRIPTS_DIR="" # Path to a directory containing custom shell scripts, including a main.sh script.
# If provided, these scripts will be copied into the chroot environment, made executable, and main.sh will be executed within the chroot.
# This allows for custom configurations or installations to be performed inside the SquashFS image.
# --- User Creation ---
ADMIN_USER="neoapps" # Creates an user with the wheel group and adds the wheel group to the sudoers file.
ADMIN_DOTFILES="" # A git repo with dotfiles that will copy to your home directory
ADMIN_DOTFILES_TYPE="" # Type of dotfile repo.
# HOME - the inside of the repo has data for your home directory (ex: .zshrc, .config, .bashrc) (requires git to be in PACKAGES)
# CONFIG - the inside of the repo has data for your config directory (ex: gtk, fish, kitty, hypr) (requires git to be in PACKAGES)
# * - ignore dotfiles repo and copy dotfiles from that user's home.
```
Save the file as `my.mkobsfs`
Then run the following command as root:
```
mkobsidiansfs my.mkobsfs
```
### Flash the system image
Locate your drive:
```
lsblk
```
Please pick a drive and not a partition.
Then run the following command as root:
```
obsidianctl install /dev/sdX system.sfs
```
***THIS WILL ERASE THE DRIVE.***

Please replace `/dev/sdX` and `system.sfs` with the correct drive and image.
After this you will have a fully functional ObsidianOS install. Reboot and you will be able to use your newly installed system.
