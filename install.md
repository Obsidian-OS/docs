## Before You Begin

Check the [System Requirements](https://obsidian-os.github.io/docs/requirements.html).

***

## Step 1: Choose Your Installation Method

You have four options:

> **💡 Quick Start**: For most users, simply download the ObsidianOS ArchISO, boot from it, and follow the ARbs wizard that appears automatically!


### **Option 1: Use the Customized ArchISO**

- Download a pre-built ArchISO from the [ObsidianOS Download Page](https://obsidian-os.github.io/download.html), or build your own with:
- We also have KDE and Cosmic ISOs.
  ```bash
  git clone https://github.com/Obsidian-OS/archiso/
  cd archiso
  git submodule update --remote --merge --recursive --init
  sudo make -B
  ```
  - The created ISO is in the `out` directory.
- **The customized ArchISO provides both `obsidianctl` and `mkobsidiansfs` tools out of the box.**
- On startup, the ARbs installation wizard is available for easy installation. On the graphical ISOs, we have a graphical installer AND ARbs
- Great for offline installations or when you need a complete bootable environment.

### **Option 2: Use an Existing Arch Linux Host**

- Install the [`arch-install-scripts`](https://archlinux.org/packages/extra/any/arch-install-scripts/) package on your Arch system.
- Manually build and install [obsidianctl](https://github.com/Obsidian-OS/obsidianctl) (or use [AUR](https://aur.archlinux.org/packages/obsidianctl-git)) and [mkobsidiansfs](https://github.com/Obsidian-OS/mkobsidiansfs).
- Proceed with OS image creation and installation using those tools.

### **Option 3: *NEW!* Use an Existing Arch Linux Host with** `ARbs` (ARch image Based inStaller)
- Just run:
```
sudo bash -c "$(curl -fsSL https://arbs.obsidianos.xyz)"
```

(the name is a backronym from the short url)
## Step 2: What Happens When You Boot the ArchISO

When you boot from the ObsidianOS ArchISO:

1. **ARbs Wizard Launches**: The installation wizard starts automatically
2. **Choose Installation Type**: Select "Install ObsidianOS" from the main menu
3. **Dual-Boot Setup**: Choose whether to enable dual-boot alongside your existing OS
4. **Disk Selection**: The wizard shows all available storage devices with sizes and models
5. **System Image**: Choose from default, pre-configured, or custom images
6. **Final Confirmation**: Review all settings before proceeding
7. **Automatic Installation**: ARbs handles the entire installation process

**That's it!** No need to remember commands or manually configure anything.

## Step 3: Create a System Image (Manual Method Only)

1. Configure `my.mkobsfs` for your needs:
   - Example config includes build directory, package list, user settings, and optional dotfiles arrangement.
2. Build the SquashFS image:
   ```bash
   mkobsidiansfs my.mkobsfs
   ```

***

## Step 4: Install the OS Image (Manual Method Only)

1. Check available drives:
   ```bash
   lsblk
   ```
2. Install the image:
   ```bash
   obsidianctl install /dev/sdX /etc/system.sfs
   ```
   - Replace `/dev/sdX` with the target drive and adjust `/etc/system.sfs` as needed. Also, inputting a `.mkobsfs` file instead of the SquashFS image is supported.

***

## Step 5: Reboot

- When finished, reboot your system. You now have a functional ObsidianOS installation.

***

### **More info**

- **ARbs inside the ObsidianOS ArchISO** is the preferred and most user-friendly method, providing a complete guided installation experience.
- The **customized ArchISO** includes all essential tools and the ARbs wizard out of the box.
- For advanced users, building from a regular Arch host remains fully supported.
- ARbs can also be used on existing Arch systems for easy system management and updates.
