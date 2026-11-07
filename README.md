# My Bluetooth

A local customization of Omarchy's `omarchy.bluetooth` bar widget, installed as `neo.bluetooth`. It retains the upstream `omarchy.bluetooth` IPC target and shell integration.

## Features

- Connect, pair, disconnect, and forget devices; show battery level when available.
- Group connected, remembered, and discovered devices; filter address/UUID-only names.
- Scan only when the manual scan switch is enabled; closing the panel disables it.
- Control Bluetooth power through Omarchy's persistent rfkill helper.
- Use keyboard navigation and route newly connected Bluetooth audio through PipeWire.

Left-click the bar icon to open the panel; right-click it to toggle Bluetooth power. Click a connected device to disconnect, or another device to connect/pair. Use the Forget button to remove a remembered device.

## Requirements and installation

Requires the Omarchy shell and its Qt/Quickshell Bluetooth and PipeWire modules, BlueZ, PipeWire, and these Omarchy helpers on `PATH`:

```text
omarchy-bluetooth-device
omarchy-bluetooth-power
omarchy-audio-output-set-default
```

The repository lives at `~/Developer/my-bluetooth`; `~/.config/omarchy/plugins/neo.bluetooth` links to it for the running shell. On another Omarchy machine, copy or link the project into that plugin directory, then validate and enable it:

```bash
omarchy plugin validate "$(realpath ~/.config/omarchy/plugins/neo.bluetooth)"
omarchy plugin enable neo.bluetooth
```

Use this widget as the replacement for the stock Bluetooth widget: both use the same IPC target. Source edits in the plugin directory hot-reload through Omarchy. This is a shell plugin, not a standalone QML application.

## Files

| Path | Purpose |
| --- | --- |
| `manifest.json` | Plugin identity, entry point, and upstream origin |
| `Panel.qml` | Bluetooth UI, discovery lifecycle, device actions, and audio routing |
| `Model.js` | Device grouping, filtering, state projection, and matching helpers |

## Provenance

Cloned from Omarchy's built-in `omarchy.bluetooth` plugin. Existing upstream author metadata is retained in the manifest; this repository records the local customization. No separate license has been added.

## Validation

```bash
omarchy plugin validate .
node --check Model.js
```

Validate the resolved repository path: the validator rejects a symlink passed as the plugin folder, although the shell discovers linked plugin directories. Manifest and JavaScript checks do not exercise hardware, audio routing, or the live panel. There is no standalone automated test suite.
