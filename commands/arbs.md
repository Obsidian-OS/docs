---
title: ARbs
nav_order: 4.3
parent: Commands
---
# `ARbs` - ARch image Based inStaller

`ARbs` (pronounced "Arbs") is an interactive, user-friendly installation wizard for ObsidianOS. It provides a beautiful terminal-based interface for installing, updating, and managing ObsidianOS systems without needing to remember complex command-line syntax.

The wizard automatically detects your environment and provides different options based on whether you're running from the ObsidianOS ArchISO or an existing Arch system.

## Overview

ARbs is a Python-based wizard that:
- Provides an intuitive menu-driven interface
- Automatically detects available disks and system images
- Handles both installation and system management tasks
- Integrates seamlessly with `obsidianctl` and `mkobsidiansfs`
- Supports dual-boot configurations
- Offers different functionality based on your environment

## Usage

### Basic Usage
```bash
# Run the wizard
python3 obsidian-wizard.py

# Or if you have it installed system-wide (like in ObsidianOS ArchISO)
obsidian-wizard
```

### Quick Installation (Recommended)
```bash
# One-liner installation and execution
sudo bash -c "$(curl -fsSL https://arbs.obsidianos.xyz)"
```

## Features

### **Beautiful Terminal Interface**
- ASCII art ObsidianOS logo
- Color-coded menus and status information
- Responsive design that adapts to terminal size
- Professional-looking progress bars and confirmations

### **Automatic Detection**
- **Disk Detection**: Automatically finds available storage devices
- **Image Detection**: Discovers `.mkobsfs` config files and `.sfs` system images
- **Environment Detection**: Adapts functionality based on ArchISO vs. host system
- **Slot Detection**: Identifies current A/B boot slots

### **Installation Features**
- **Fresh Installation**: Complete system installation with dual-boot support
- **System Repair**: Reinstall/repair existing ObsidianOS installations
- **Configuration Creation**: Interactive creation of `.mkobsfs` files
- **Image Selection**: Choose from pre-configured or custom system images

### **System Management** (Host System Only)
- **Slot Switching**: Temporary and permanent A/B slot switching
- **System Updates**: Update specific boot slots with new images
- **Slot Synchronization**: Clone active slot to inactive slot
- **System Reboot**: Safe system restart functionality

## Menu Options

### **Main Menu** (All Environments)
- **Install ObsidianOS**: Fresh system installation
- **Repair ObsidianOS**: System repair/reinstallation
- **Drop to Terminal**: Exit to command line
- **Reboot System**: Restart the system

### **Additional Options** (Host System Only)
- **Update System**: Update specific boot slots
- **Switch Slot and Reboot (temporary)**: One-time slot switch
- **Switch Slot and Reboot (permanent)**: Persistent slot switch
- **Sync slots**: Clone active slot to inactive slot

## Installation Flow

### 1. **Dual Boot Configuration**
Choose whether to enable dual-boot alongside existing operating systems:
- **Enable Dual Boot**: Preserves existing OS, installs ObsidianOS alongside
- **Single Boot Only**: Replaces existing OS with ObsidianOS

### 2. **Disk Selection**
The wizard displays all available storage devices with:
- Device name (e.g., `/dev/sda`)
- Storage capacity
- Device model information

**⚠️ Warning**: Selected disk will be modified during installation.

### 3. **System Image Selection**
Choose from multiple image sources:

#### **Default System Image** (ArchISO only)
- Uses the built-in `/etc/system.sfs` image
- Quick installation with standard configuration

#### **Pre-configured Images** (ArchISO only)
- Images stored in `/usr/preconf/` directory
- Pre-built configurations for different use cases

#### **System Images** (All environments)
- `.sfs` files in `/usr/preconf/` or current directory
- Ready-to-install SquashFS images

#### **Create New Config**
- Generates a new `~/config.mkobsfs` file
- Opens in `nano` editor for customization
- Uses sensible defaults for quick setup

#### **Local Directory**
- Images and configs in current working directory
- Useful for custom builds and testing

### 4. **Final Confirmation**
Review installation details:
- **Action**: Install/Repair/Update
- **Target**: Selected disk or slot
- **Image**: System image or config file
- **Slot**: Target boot slot (if applicable)
- **Dual Boot**: Dual-boot configuration status

### 5. **Execution**
The wizard executes the appropriate `obsidianctl` command with:
- Progress indication
- Real-time status updates
- Error handling and reporting
- Success/failure confirmation

## Configuration Files

### **Default Configuration Template**
When creating a new config, ARbs generates a `~/config.mkobsfs` with:

```bash
BUILD_DIR="obsidian_rootfs"
PACKAGES="base linux linux-firmware networkmanager sudo vim nano efibootmgr python squashfs-tools arch-install-scripts base-devel git gptfdisk wget os-prober pv"
OUTPUT_SFS="system.sfs"
TIMEZONE=""
HOSTNAME="obsidianbtw"
YAY_GET="obsidianctl-git"
ROOT_HAVEPASSWORD="nopassword"
CUSTOM_SCRIPTS_DIR=""
ADMIN_USER="user"
ADMIN_DOTFILES=""
ADMIN_DOTFILES_TYPE=""
```

### **Customization**
- Edit the generated file in `nano`
- Add your preferred packages and settings
- Configure user accounts and dotfiles
- Set timezone and hostname preferences

## Environment-Specific Features

### **ArchISO Environment**
- **Automatic tmpfs resizing**: Optimizes memory usage for installation
- **Built-in tools**: `obsidianctl` and `mkobsidiansfs` available by default
- **Pre-configured images**: Access to official system images
- **Installation focus**: Optimized for fresh system installation

### **Host System Environment**
- **Full system management**: All obsidianctl features available
- **A/B slot operations**: Complete slot switching and management
- **Update capabilities**: System image updates and slot synchronization
- **Flexible deployment**: Install to any available storage device

## Keyboard Navigation

- **↑/↓**: Navigate menu options
- **Enter**: Select highlighted option
- **Q**: Quit/exit current menu
- **Ctrl+C**: Emergency exit

## Error Handling

### **Graceful Error Recovery**
- **Disk detection failures**: Clear error messages and recovery options
- **Command execution errors**: Detailed error reporting and status
- **User interruptions**: Clean exit with confirmation
- **Critical failures**: Automatic system reboot for safety

### **User Feedback**
- **Progress indicators**: Real-time status updates
- **Confirmation dialogs**: Clear warnings before destructive operations
- **Error reporting**: Detailed information for troubleshooting
- **Recovery options**: Multiple paths to resolve issues

## Examples

### **Quick Installation from ArchISO**
```bash
# Boot from ObsidianOS ArchISO
# Select "Install ObsidianOS" from the wizard
# Choose dual-boot or single-boot
# Select target disk
# Choose "Default System Image"
# Confirm and execute
```

### **System Update from Host**
```bash
# Run ARbs on existing Arch system
python3 obsidian-wizard.py

# Select "Update System"
# Choose target slot (A or B)
# Select new system image
# Confirm and execute
```

### **Custom Configuration Installation**
```bash
# Run ARbs
python3 obsidian-wizard.py

# Select "Install ObsidianOS"
# Choose "Create New Config"
# Customize in nano editor
# Save and continue with installation
```

## Integration

### **obsidianctl Integration**
ARbs automatically calls the appropriate `obsidianctl` commands:
- `obsidianctl install` for fresh installations
- `obsidianctl update` for system updates
- `obsidianctl switch` for slot switching
- `obsidianctl sync` for slot synchronization

### **mkobsidiansfs Integration**
- Generates default configuration templates
- Integrates with custom `.mkobsfs` files
- Supports both config files and built images

## Requirements

- **Python 3**: Core runtime environment
- **Linux terminal**: TTY support for interactive interface
- **obsidianctl**: For system operations (auto-detected)
- **Root privileges**: Required for system installation and management

## Notes

- The wizard automatically detects and adapts to your environment
- All destructive operations require explicit confirmation
- Progress is clearly indicated during long-running operations
- Error handling provides clear recovery paths
- The interface is designed to be intuitive for both beginners and advanced users 