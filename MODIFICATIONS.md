# AetherDeck — Modifications

AetherDeck is a fork of [nekename/OpenDeck](https://github.com/nekename/OpenDeck) maintained for the AetherSDR amateur radio community on Linux. This file documents modifications per GPL-3.0 §5(a).

## Base

- **Upstream**: nekename/OpenDeck v2.11.1
- **License**: GPL-3.0-or-later (preserved; see [LICENSE.md](LICENSE.md))

## Significant changes from upstream

### Hardware support
- Stream Deck + XL (USB PID `0x00c6`) — adds `Kind::PlusXl` arm in `src-tauri/src/elgato.rs` and consumes a fork of the `elgato-streamdeck` Rust crate (path dependency to `../elgato-streamdeck-plusxl`) until [OpenActionAPI/rust-elgato-streamdeck#54](https://github.com/OpenActionAPI/rust-elgato-streamdeck/pull/54) merges.

### Touch strip & dial UI
- Imports the touch-strip rendering work from [KatsuJinCode/OpenDeck PR #306](https://github.com/nekename/OpenDeck/pull/306) plus alignment + dial-circles improvements ([ten9876/OpenDeck pr306-frontend-fixes](https://github.com/KatsuJinCode/OpenDeck/pull/1)).
- Renders both a touch-strip slot and a dial circle per encoder, matching Elgato's app dual-representation behavior.

### Window chrome
- Frameless window with custom Discord-style titlebar (`src/components/Titlebar.svelte`).
- Custom resize handles for the non-decorated window (`src/components/ResizeHandles.svelte`).
- Default window size `1100x900`; minimum `480x360`. Removed dynamic-resize-on-device-connect behavior so user-adjusted sizes persist.

### Device area
- 60% zoom on the device area for a more compact display (`zoom: 0.6` on the rowgroup in `DeviceView.svelte`).
- Removed `device-fade-x/y/xy` mask gradients.

### Plugin settings UI (Elgato SDK alignment)
- AetherDeck auto-renders top-level `PropertyInspectorPath` from the Elgato SDK manifest as a "Plugin Settings" panel. Upstream OpenDeck does NOT auto-render this — it only fires `showSettingsInterface` for plugins that opt in via a non-standard `HasSettingsInterface: true` flag. AetherDeck implements the standard SDK behavior in addition to the OpenDeck-specific path.
- Components: `src/components/PluginSettingsView.svelte`, `get_plugin_property_inspector_path` Tauri command in `src-tauri/src/events/frontend/plugins.rs`.

### Settings dialog
- Removed the upstream-specific "leave a star on GitHub / sponsor me" footer with embedded links to nekename's repo and sponsor page (`src/components/SettingsView.svelte`).

## Fork direction

Wherever OpenDeck and the Elgato Stream Deck SDK diverge, AetherDeck follows the SDK. AetherDeck-specific divergences are NOT submitted as PRs to upstream OpenDeck — they are deliberate fork choices. Generally-useful bug fixes ARE upstreamed.

## Authorship

Original OpenDeck authorship and copyright headers are preserved throughout the source. Squashed initial-fork commit history references upstream commits via the `upstream` (nekename/OpenDeck) and `katsujincode` (KatsuJinCode/OpenDeck) git remotes, where individual contributions remain visible.
