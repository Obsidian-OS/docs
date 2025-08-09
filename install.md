---
title: Installation guide
nav_order: 2
---

# ObsidianOS Installation Guide
## Getting ready
You may either:
1. Boot off the [customized archiso](https://github.com/Obsidian-OS/archiso/). or,
2. Install the [arch-install-scripts](https://archlinux.org/packages/extra/any/arch-install-scripts/) package on an arch system, build and install [obsidianctl](https://github.com/Obsidian-OS/obsidianctl) and [mkobsidiansfs](https://github.com/Obsidian-OS/mkobsidiansfs).
## Start install
### Formatting the target drive (archiso only, optional)
This is only optional if you are not creating your own image.

As the archiso is located in a ramdisk, you must build in a partition. Firstly, locate your target disk:
```
lsblk
```
Run this command to turn it into an ext4 partition that we will only use for building:
```
mkfs.ext4 /dev/sdX
```
***THIS WILL ERASE THE DRIVE.*** Please choose a disk and not an partition.
Then, mount the drive that will be used for creating the image:
```
mount /dev/sdX /mnt
```
Please switch out `/dev/sdX` with the correct drive.
### Creating a system image (optional)
This step is only optional if you are using the archiso.
Modify the following example to suit your preferences:
```
# --- System creation ---
BUILD_DIR="/mnt/obsidian_rootfs" # SquashFS generation directory
PACKAGES="base linux linux-firmware networkmanager sudo vim nano efibootmgr python squashfs-tools arch-install-scripts base-devel" # Packages to install
OUTPUT_SFS="/etc/system.sfs" # Output SquashFS. Please place this somewhere else if you are not on the archiso.
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
Save the file as `my.mkobsfs`.
Then run the following command:
```
mkobsidiansfs my.mkobsfs
```
### Flash the system image
Locate your drive:
```
lsblk
```
Please pick a drive and not a partition. If you made your own image on the archiso, use the same drive as you formatted in the first step.
Then run the following command:
```
obsidianctl install /dev/sdX /etc/system.sfs
```
***THIS WILL ERASE THE DRIVE.*** Please replace `/dev/sdX` with the correct drive and replace `/etc/system.sfs` with the correct location.
After this you will have a fully functional ObsidianOS install. Reboot and you will be able to use your newly installed system.
