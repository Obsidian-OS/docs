---
title: plugind
nav_order: 4.4
parent: Commands
---
# `plugind` and `pluginctl`

`plugind` is the core daemon for ObsidianOS's dynamic plugin system, designed to extend system functionality by executing user-defined scripts in response to various system events. It works in conjunction with the `pluginctl` command-line interface, which provides a convenient way to manage and interact with the daemon and its registered plugins.

## Overview

The `plugind` daemon runs continuously in the background, listening for commands via a Unix socket (`/tmp/plugind.sock`) and monitoring system events. It loads plugins from predefined directories, parses their manifests, and executes them when their associated events occur. The `pluginctl` utility allows users to send commands to `plugind`, such as listing available plugins, enabling/disabling them, or manually triggering specific plugin actions.

### Key Responsibilities of `plugind`:
- **Plugin Discovery**: Automatically loads plugins from `/etc/plugins` (system-wide) and `~/.config/plugins` (user-specific).
- **Event Monitoring**: Watches for file system changes (via `inotify`) on paths specified by plugins and monitors specific system states (e.g., battery level).
- **Plugin Execution**: Forks new processes to run plugin scripts, with support for user impersonation.
- **Inter-process Communication**: Communicates with the `pluginctl` utility via a Unix domain socket.
- **Logging**: Records daemon activities and plugin execution details to `/var/plugind/daemon.log`.

### Key Responsibilities of `pluginctl`:
- **Daemon Interaction**: Sends commands to the `plugind` daemon.
- **Plugin Management**: Provides commands to list, inspect, enable, disable, and trigger plugins.
- **User Interface**: Presents plugin information in a human-readable format, with an option for raw JSON output.

## Plugin Structure and Configuration

Each plugin is a self-contained directory containing at least a `manifest.plug` file. This file is a bash script that defines the plugin's metadata and its entry point (`main` function).

### `manifest.plug` Variables:

- **`SLOT`**: (Optional) Specifies the system slot (`A`, `B`, or `AB`) for which the plugin is active. If the current system slot does not match, the plugin's execution is skipped. Default is `AB`.
  - Example: `SLOT="A"`

- **`EVENT`**: A space-separated list of event names that will trigger the plugin's `main` function. `plugind` monitors for these events.
  - Common events:
    - `OnEnabled`: Triggered when the plugin is enabled via `pluginctl enable`.
    - `OnDisabled`: Triggered when the plugin is disabled via `pluginctl disable`.
    - `OnBatteryLevelChange`: Triggered when the battery capacity file (`/sys/class/power_supply/BAT0/capacity`) changes.
    - File system event types (e.g., `Create`, `Modify`, `Remove`, `Access`, `CloseWrite`, `Open`, `MovedFrom`, `MovedTo`, `Attrib`, `Ignored`, `Rescan`, `Error`). These correspond to `notify` crate event kinds.
  - Example: `EVENT="OnEnabled OnBatteryLevelChange Modify"`

- **`RUN_AS`**: (Optional) Defines the user context under which the plugin's `main` function will be executed.
  - `@`: The user who invoked the `pluginctl` command.
  - `#`: The `root` user.
  - `<username>`: A specific system username.
  - If empty, the plugin runs as the `plugind` daemon user.
  - Example: `RUN_AS="@"` or `RUN_AS="#"` or `RUN_AS="myuser"`

- **`RESTART_ON`**: (Currently not implemented).

- **`WATCH_PATH`**: (Optional) An absolute file system path that `plugind` will monitor for changes. Any event on this path (or its subdirectories if recursive) that matches an `EVENT` type will trigger the plugin.
  - Example: `WATCH_PATH="/home/user/my_app/config.json"`

### Plugin `main` Function:

The `main` function within `manifest.plug` is the primary entry point for your plugin's logic. When an event triggers the plugin, `plugind` executes this function.

- **Environment Variables**: `plugind` sets specific environment variables before executing `main`:
  - `EVENT_NAME`: Contains the name of the event that triggered the plugin (e.g., `OnBatteryLevelChange`, `Modify`).
  - `EVENT_RETURN`: (Optional) For certain events (like `OnBatteryLevelChange`), this variable contains additional data related to the event (e.g., the new battery level).

### Example `manifest.plug`:

```bash
# manifest.plug for a battery notification plugin
SLOT="AB"
EVENT="OnBatteryLevelChange"
RUN_AS="@"
WATCH_PATH="/sys/class/power_supply/BAT0/capacity"

main() {
    if [ "$EVENT_NAME" == "OnBatteryLevelChange" ]; then
        BATTERY_LEVEL="$EVENT_RETURN"
        if [ -n "$BATTERY_LEVEL" ]; then
            echo "Battery level changed to: $BATTERY_LEVEL%"
            # Example: Send a desktop notification
            # notify-send "Battery Alert" "Battery is at $BATTERY_LEVEL%"
        fi
    fi
}
```

## Usage

### `pluginctl` Command-Line Interface

The `pluginctl` utility provides the following commands to interact with `plugind`:

- **`pluginctl list`**
  - Lists all plugins currently known to the `plugind` daemon, along with their status (enabled/disabled), slot, events, and run-as user.
  - Example Output:
    ```
+-------------+------+------------------------------+--------+----------------+---------+
| Name        | Slot | Event                        | Run As | Restart On     | Enabled |
+-------------+------+------------------------------+--------+----------------+---------+
| test-plugin | AB   | OnTest, OnBatteryLevelChange | #      | OnScriptFailed | true    |
+-------------+------+------------------------------+--------+----------------+---------+
    ```

- **`pluginctl info <plugin_name>`**
  - Displays detailed information about a specific plugin, including its full path, configured events, and other manifest details.
  - Example:
    ```bash
    pluginctl info test-plugin
    ```

- **`pluginctl enable <plugin_name>`**
  - Enables the specified plugin. If the plugin has an `OnEnabled` event defined, it will be triggered.
  - Example:
    ```bash
    pluginctl enable test-plugin
    ```

- **`pluginctl disable <plugin_name>`**
  - Disables the specified plugin. If the plugin has an `OnDisabled` event defined, it will be triggered.
  - Example:
    ```bash
    pluginctl disable test-plugin
    ```

- **`pluginctl trigger <plugin_name> <event_name> [event_data]`**
  - Manually triggers a specific event for a given plugin. This is useful for testing plugins or for integrating with other scripts.
  - `event_data` is an optional string that will be passed to the plugin via the `EVENT_RETURN` environment variable.
  - Example:
    ```bash
    pluginctl trigger battery-mon OnBatteryLevelChange "50"
    pluginctl trigger config-sync ManualSync
    ```

- **`pluginctl reload`**
  - Instructs the `plugind` daemon to re-read all plugin manifests from disk. This is useful after adding, removing, or modifying plugin files.
  - Example:
    ```bash
    pluginctl reload
    ```

- **`pluginctl status`**
  - Provides a basic status check of the `plugind` daemon.
  - Example:
    ```bash
    pluginctl status
    ```

- **`pluginctl --raw <command>`**
  - Most `pluginctl` commands support a `--raw` flag, which outputs the daemon's response as raw JSON, useful for scripting and integration.
  - Example:
    ```bash
    pluginctl --raw list
    ```

## Features

- **Dynamic Plugin Loading**: Plugins are loaded at runtime without requiring daemon restarts.
- **Event-Driven Architecture**: Reacts to system and file system events for automated tasks.
- **Secure Execution**: Utilizes `fork` and `setuid`/`setgid` to execute plugins with appropriate user permissions.
- **Modular Design**: Encourages small, single-purpose scripts for easy management and debugging.
- **Extensible**: New event types and monitoring capabilities can be added to the daemon.

## Requirements

### For `plugind` Daemon:
- GNU/Linux operating system (due to Unix sockets, `inotify`, `fork`, `setuid`/`setgid`).
- `wheel` group for socket permissions.

### For Plugins:
- `bash` (or compatible shell) for `manifest.plug` execution.
- Any utilities or interpreters required by the plugin's `main` function (e.g., `notify-send` for desktop notifications, `python`, `node`).

## Installation

To install `plugind` and `pluginctl` from source, you will need to have Rust and Cargo installed. If you don't have them, you can install them using `rustup`:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Once Rust and Cargo are installed, navigate to the project's root directory and build the binaries:

```bash
cargo build --release
```

This will compile the `plugind` daemon and the `pluginctl` CLI tool. The compiled binaries will be located in `target/release/`.

You can then manually copy them to your system's PATH, for example:

```bash
sudo cp target/release/plugind /usr/local/bin/
sudo cp target/release/pluginctl /usr/local/bin/
```

For `plugind` to run as a system service, you would typically set up a systemd service unit. An example service file might look like this (you would need to create and enable it):

```
[Unit]
Description=ObsidianOS Plugin Daemon
After=network.target

[Service]
ExecStart=/usr/local/bin/plugind
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Save this as `/etc/systemd/system/plugind.service` and then:

```bash
sudo systemctl enable plugind
sudo systemctl start plugind
```

## Development

To contribute to `plugind` or `pluginctl`, or to build from source for development purposes:

1.  **Clone the repository**:
    ```bash
    git clone https://github.com/Obsidian-OS/plugind
    cd plugind
    ```

2.  **Build the project**:
    ```bash
    cargo build
    ```

3.  **Run tests**:
    ```bash
    cargo test
    ```

4.  **Run the daemon (for testing)**:
    ```bash
    cargo run --bin plugind
    ```

5.  **Run the CLI (for testing)**:
    ```bash
    cargo run --bin pluginctl -- <command>
    # Example:
    # cargo run --bin pluginctl -- list
    ```


