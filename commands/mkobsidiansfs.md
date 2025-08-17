---
title: mkobsidiansfs
nav_order: 4.2
parent: Commands
---
# `mkobsidiansfs`
`mkobsidiansfs` is a system image building tool designed to create ObsidianOS SquashFS system images. It automates the process of bootstrapping an Arch system, installing packages, configuring users, and generating a compressed filesystem image ready for deployment.

You can run the `mkobsidiansfs` script directly. **This script must be run as root** as it performs system-level operations like package installation and filesystem creation.

```bash
sudo mkobsidiansfs [config_file] [output_sfs]
```

## Overview
The tool creates a complete system image by:
1. Bootstrapping a base Arch system using `pacstrap`
2. Installing specified packages and AUR packages
3. Configuring users, services, and system settings
4. Running custom scripts and post-install commands
5. Generating a compressed SquashFS image

## Usage

### Basic Usage
```bash
sudo mkobsidiansfs
```

### With Custom Configuration
```bash
sudo mkobsidiansfs /path/to/config.mkobsfs
```

### With Custom Output Name
```bash
sudo mkobsidiansfs /path/to/config.mkobsfs custom_system.sfs
```

## Configuration

The script uses a configuration file (`.mkobsfs` extension) to customize the build process. You can create your own configuration file by copying the default settings and modifying them.

### Configuration Variables

#### Build Settings
- **`BUILD_DIR`**: Directory for building the rootfs (default: `/tmp/obsidian_rootfs`)
- **`OUTPUT_SFS`**: Output SquashFS filename (default: `system.sfs`)

#### Package Management
- **`PACKAGES`**: Space-separated list of Arch packages to install
- **`HAVE_AUR`**: AUR helper to install (`yay-bin` or `none`)
- **`YAY_GET`**: AUR packages to install (space-separated)

#### System Configuration
- **`TIMEZONE`**: Olson timezone identifier (e.g., `America/New_York`)
- **`HOSTNAME`**: System hostname (default: `obsidianbtw`)
- **`SERVICES`**: Systemd services to enable (space-separated)
- **`ROOT_HAVEPASSWORD`**: Set to any value to remove root password

#### User Management
- **`ADMIN_USER`**: Username for admin user (creates user if set)
- **`ADMIN_PASSWORD`**: Password for admin user (empty = interactive)
- **`ADMIN_DOTFILES`**: Git repository for user dotfiles
- **`ADMIN_DOTFILES_TYPE`**: Dotfiles repository type:
  - `HOME`: Repository contains home directory files (`.zshrc`, `.config`, etc.)
  - `CONFIG`: Repository contains `.config` directory files
  - `*`: Copy dotfiles from specified user's home directory

#### Custom Scripts
- **`CUSTOM_SCRIPTS_DIR`**: Directory containing scripts to run in chroot
- **`POST_INSTALL`**: Bash command to execute after installation

#### Network Update
- **`NETUPDATE`**: Internal variable for obsidianctl netupdate (do not set)

### Environment Variables and Profiles

The configuration file supports environment variables, allowing you to create dynamic configurations and profile-based setups. You can use environment variables in your `.mkobsfs` files to create flexible, reusable configurations.

#### Environment Variable Support

Since `.mkobsfs` files are sourced as bash scripts, they support:
- Environment variable expansion (`$VARIABLE`)
- Default value fallbacks (`${VARIABLE:-default}`)
- Conditional logic with `if` statements
- Shell parameter expansion
- All bash scripting features

#### Using Environment Variables

```bash
# Set environment variables before running mkobsidiansfs
export PROFILE="gaming"
export USERNAME="neo"
export TIMEZONE="America/New_York"

# Then run with your config file
sudo mkobsidiansfs myconfig.mkobsfs
```

#### Practical Profile Usage

**Create different system images based on profile:**
```bash
# Development system
PROFILE=dev USERNAME=developer TIMEZONE=America/New_York sudo mkobsidiansfs devconfig.mkobsfs dev_system.sfs

# Gaming system
PROFILE=gaming USERNAME=gamer TIMEZONE=America/Los_Angeles sudo mkobsidiansfs gamingconfig.mkobsfs gaming_system.sfs

# Server system
PROFILE=server USERNAME=admin TIMEZONE=UTC sudo mkobsidiansfs serverconfig.mkobsfs server_system.sfs
```

**Using a single config file with multiple profiles:**
```bash
# The same config file can handle different profiles
PROFILE=dev sudo mkobsidiansfs universal_config.mkobsfs dev_system.sfs
PROFILE=gaming sudo mkobsidiansfs universal_config.mkobsfs gaming_system.sfs
```

#### Profile-Based Configuration Examples

**Development Profile (`PROFILE=dev`):**
```bash
# devconfig.mkobsfs
if [[ "$PROFILE" == "dev" ]]; then
  PACKAGES="$PACKAGES base-devel git neovim python nodejs npm"
  HAVE_AUR="yay-bin"
  YAY_GET="obsidianctl-git neovim-git"
  ADMIN_USER="${USERNAME:-dev}"
  ADMIN_DOTFILES="https://github.com/username/dev-dotfiles.git"
  ADMIN_DOTFILES_TYPE="CONFIG"
  POST_INSTALL="echo 'Development environment ready!'"
fi
```

**Gaming Profile (`PROFILE=gaming`):**
```bash
# gamingconfig.mkobsfs
if [[ "$PROFILE" == "gaming" ]]; then
  PACKAGES="$PACKAGES steam gamemode lib32-mesa vulkan-icd-loader"
  HAVE_AUR="yay-bin"
  YAY_GET="obsidianctl-git gamemode"
  ADMIN_USER="${USERNAME:-gamer}"
  ADMIN_DOTFILES="https://github.com/username/gaming-dotfiles.git"
  ADMIN_DOTFILES_TYPE="HOME"
  POST_INSTALL="echo 'Gaming setup complete!'"
fi
```

**Server Profile (`PROFILE=server`):**
```bash
# serverconfig.mkobsfs
if [[ "$PROFILE" == "server" ]]; then
  PACKAGES="$PACKAGES openssh nginx mariadb postgresql"
  SERVICES="NetworkManager sshd nginx mariadb postgresql"
  ADMIN_USER="${USERNAME:-admin}"
  ROOT_HAVEPASSWORD="disabled"
  POST_INSTALL="echo 'Server environment configured!'"
fi
```

### Example Configuration File

Create a file named `myconfig.mkobsfs`:

```bash
# Custom configuration for mkobsidiansfs
PACKAGES="$PACKAGES firefox"
HAVE_AUR="yay-bin"
YAY_GET="obsidianctl-git neovim-git"
TIMEZONE="${TIMEZONE:-UTC}"
HOSTNAME="${HOSTNAME:-obsidianbtw}"
SERVICES="NetworkManager sshd"
ADMIN_USER="${USERNAME:-neo}"
ADMIN_DOTFILES="https://github.com/username/dotfiles.git"
ADMIN_DOTFILES_TYPE="HOME"
CUSTOM_SCRIPTS_DIR="/path/to/custom/scripts"
POST_INSTALL="echo 'Installation complete!'"

# Profile-specific configurations
if [[ "$PROFILE" == "dev" ]]; then
  PACKAGES="$PACKAGES base-devel git neovim python"
  YAY_GET="$YAY_GET neovim-git"
fi

if [[ "$PROFILE" == "gaming" ]]; then
  PACKAGES="$PACKAGES steam gamemode"
  YAY_GET="$YAY_GET gamemode"
fi
```

## Features

### Automatic Package Installation
- Installs base Arch Linux system
- Adds specified packages from official repositories
- Optionally installs AUR packages using yay

### User Management
- Creates admin user with sudo privileges
- Sets up dotfiles from Git repositories
- Configures user passwords interactively or via config

### Custom Scripts
- Runs custom scripts in chroot environment
- Executes post-install commands
- Supports complex customization workflows

### System Configuration
- Sets timezone and hostname
- Enables systemd services
- Configures root user password

### AUR Integration
- Automatically installs yay-bin if specified
- Downloads and installs AUR packages
- Handles build dependencies

## Requirements

The script requires the following packages to be installed on the host system:
- `git` - For dotfiles and AUR package management
- `squashfs-tools` - For creating SquashFS images
- `arch-install-scripts` - For pacstrap and arch-chroot

## Output

The script generates:
- A compressed SquashFS system image
- Configuration backup in `/etc/config.mkobsfs` (if config file provided)
- Network update flag file (if enabled)

## Error Handling

The script includes comprehensive error checking:
- Validates required commands are available
- Checks for root privileges
- Verifies package installation success
- Handles mount/unmount operations gracefully

## Examples

### Minimal System
```bash
sudo mkobsidiansfs
```

### Custom System with AUR Packages
```bash
sudo mkobsidiansfs myconfig.mkobsfs my_system.sfs
```

### Development System
```bash
# Create config with development tools
echo 'PACKAGES="base linux linux-firmware networkmanager sudo vim nano efibootmgr python squashfs-tools arch-install-scripts base-devel git gptfdisk"
HAVE_AUR="yay-bin"
YAY_GET="obsidianctl-git neovim-git"
ADMIN_USER="dev"
ADMIN_DOTFILES_TYPE="CONFIG"' > devconfig.mkobsfs

sudo mkobsidiansfs devconfig.mkobsfs dev_system.sfs
```

## Notes

- The build process creates temporary files in `/tmp/obsidian_rootfs`
- Custom scripts are copied to the chroot environment and executed
- AUR packages are installed using the admin user account
- The script automatically cleans up temporary files after completion
- Network update functionality is integrated with obsidianctl 
