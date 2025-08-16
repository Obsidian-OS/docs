---
title: obsidianctl
nav_order: 4.1
parent: Commands
---
# `obsidianctl`
`obsidianctl` is a command-line utility designed to manage A/B boot slots and shared partitions on ObsidianOS systems. It provides functionalities for system installation, status monitoring, active slot switching, and updating system images.

You can run the `obsidianctl` script directly. Remember to run it with `sudo` as all commands other than `obsidianctl status` require root privileges.

```bash
sudo obsidianctl [command] [options]
```

## Commands

### `status`

Displays the currently active A/B slot and various system details. This command does not require root.

```bash
obsidianctl status
```

### `install <device> <system_sfs>`

Partitions the specified device and installs the SquashFS system image. **WARNING: This will erase all data on the target device.**

*   `<device>`: The target block device (e.g., `/dev/sda`).
*   `<system_sfs>`: Path to the SquashFS system image file (e.g., `/path/to/obsidianos.sfs`). Defaults to `/etc/system.sfs`

```bash
sudo obsidianctl install /dev/sda /path/to/your_system.sfs
```

### `switch <slot>`

Switches the active boot slot to either 'a' or 'b'. This change is persistent across reboots.

*   `<slot>`: The slot to make active (`a` or `b`).

```bash
sudo obsidianctl switch a
```

### `switch-once <slot>`

Switches the active boot slot to either 'a' or 'b'. This change is not persistent.

*   `<slot>`: The slot to make active temporarily (`a` or `b`).

```bash
sudo obsidianctl switch-once a
```

### `update <slot> <system_sfs>`

Updates a specific A/B slot with a new SquashFS system image. **WARNING: This will erase all data on the specified slot.**

*   `<slot>`: The slot to update (`a` or `b`).
*   `<system_sfs>`: Path to the new SquashFS system image file.

```bash
sudo obsidianctl update b /path/to/new_system_image.sfs
```

### `sync <slot>`

Clones the currently running slot to the specified slot. This is a block-level copy using `dd`. It copies both the root and ESP partitions. **WARNING: This will erase all data on the specified slot.**

*   `<slot>`: The slot to sync to (`a` or `b`).

```bash
sudo obsidianctl sync b
```

### `slot-diff`

Shows a tiny diff between the currently running slot and the inactive slot.

```bash
sudo obsidianctl slot-diff
```

### `netupdate <slot>`

Updates the specified slot over the internet, downloads the latest official system image. **WARNING: THIS REQUIRES YOU TO BE USING THE OFFICIAL SYSTEM IMAGE**

*    `<slot>`: The slot to netupdate (`a` or `b).

```bash
sudo obsidianctl netupdate b
```

### `enter-slot <slot> --enable-networking --mount-essentials --mount-home --mount-root`

Chroots into the specified slot, mounting all essential mountpoints, ensuring best experience.

*    `<slot>`: The slot to chroot into.
*    `--enable-networking`: Enable networking in the chroot.
*    `--mount-essentials`: Mount /proc, /sys, and /dev.
*    `--mount-home`: Bind mount /home into the chroot.
*    `--mount-root`: Bind mount /root into the chroot.

```bash
sudo obsidianctl enter-slot b --enable-networking
```
