# VCMC Java/Geyser Plugin 2.2.2

<div align="center">

<img src="https://antoic.com/icons/vcmc.png" width="110" alt="VCMC logo">

**Proximity voice chat for Java servers and Bedrock players connected through Geyser**

[Download VCMC](https://antoic.com/app.html) · [Documentation](https://antoic.com/docs/vcmc.html) · [Changelog](https://antoic.com/changelog/vcmc/) · [Discord](https://discord.gg/HA5gKcpsaq)

[![Version](https://img.shields.io/badge/VCMC-2.2.2-5865F2)](../../releases/latest)
[![API](https://img.shields.io/badge/Paper_API-1.20%2B-DBA53A)](https://papermc.io/)
[![Geyser](https://img.shields.io/badge/Geyser-Compatible-2D88C9)](https://geysermc.org/)

</div>

## What This Plugin Does

The VCMC plugin connects a Java server to the VCMC voice service. It publishes player position, dimension, mute state, groups, effects, and server settings so the app can route voice correctly.

It supports:

- Native Java players
- Bedrock players through Geyser and Floodgate
- Java inventory menus
- Native Bedrock forms for Geyser players
- Optional PlaceholderAPI integration
- The VCMC Java resource pack and voice-state icons

VCMC supports direct server voice, P2P/SFU voice, and an optional Simple Voice Chat bridge. With that bridge enabled, VCMC app and web users can speak with players using the native SVC mod. The managed relay default is `https://ws.antoic.com:8443`.

## Changes in 2.2.2

- Restore bidirectional browser audio with Simple Voice Chat, including browser PCM format conversion.
- Apply the correct proximity distance per listener and fade app audio smoothly to silence at the range boundary.
- Preserve SVC group and addon routing, cancellation, and audio modifications in the bridge.
- Keep native SVC icons for players using its mod and server resource-pack icons for Java players without it.
- Production build processed with ProGuard; 133 Java tests and 10 integration contract checks passed. Web-to-SVC audio and proximity were also checked in a live Java/Bedrock session.

## Requirements

- A Paper or compatible Java server using the Minecraft 1.20 API or newer
- The Java version required by your Minecraft server
- The VCMC app or web client for VCMC users; Java users may instead use the native SVC mod when the optional bridge is enabled
- Internet access to the VCMC service

Optional integrations:

- **Geyser + Floodgate:** Bedrock cross-play and native Bedrock forms
- **PlaceholderAPI:** voice state, level, mute, and megaphone placeholders
- **Simple Voice Chat 2.6.x:** native SVC interoperability, tested with 2.6.21. The bridge validates its adapter on startup; SVC 2.5.x is not supported by this adapter.

## Installation

1. Download the latest release from [CurseForge](https://www.curseforge.com/minecraft/bukkit-plugins/vcmc-voice-chat-for-mc-pluggin) or this repository's [Releases](../../releases/latest).
2. Place the VCMC `.jar` in the server's `plugins/` directory.
3. Restart the server.
4. Keep the generated `vcmc-room-token` private.
5. Make `rp-port` reachable if Java clients cannot download the VCMC resource pack.

The plugin creates its protected room automatically. Players never need to know the room ID or token.

## Player Setup

1. Open the VCMC app and add the server.
2. Join the Minecraft server.
3. Copy `/vcmc:verify "<code>"` from VCMC.
4. Paste it in Minecraft chat.
5. After verification, the same device reconnects automatically on later visits.

Java and Bedrock players can use the same room. Java players receive inventory menus; Bedrock players connected through Geyser receive native forms.

## VCMC 2.2.1 Features

- Distance and spatial voice data
- Custom-dimension support
- Bidirectional app and Minecraft settings
- Master and per-player volume
- Public, private, and administrative groups
- Password-protected groups
- Administrator password bypass with a confirmation warning
- Self mute and administrative mute
- Built-in, environmental, and custom voice effects
- Megaphone with P2P/SFU routing
- Speaking, mute, connection, and megaphone icons
- Stable speaking and connection icons while players join or relay presence snapshots update
- PlaceholderAPI values for TAB and nametag plugins
- Automatic room restoration and reconnection
- Compatibility with older VCMC clients through the legacy protocol
- Simple Voice Chat group interoperability with password validation
- Speaking-only native nametag microphone indicators
- Localized starter command books and `/vcmc:book`
- Korean interface and command-book translations

## Player Commands

| Command | Description |
|---|---|
| `/vcmc:verify "<code>"` | Link the Minecraft player with the code shown by VCMC |
| `/vcmc:menu` | Open voice settings, groups, and per-player volume |
| `/vcmc:book [user\|admin]` | Receive the localized VCMC command guide (30-second cooldown) |
| `/vcmc:m [true\|false]` | Toggle or set your own mute state |
| `/vcmc:groups` | Open the groups menu |
| `/vcmc:groups create <name> [password]` | Create a group |
| `/vcmc:groups join <group_or_name> [password]` | Join a group |
| `/vcmc:groups leave` | Leave the current group |
| `/vcmc:groups list` | List groups |
| `/vcmc:groups delete [group_or_name]` | Delete a group you own |

## Administrator Commands

These commands require `vcmc.admin`, which defaults to server operators.

| Command | Description |
|---|---|
| `/vcmc:admin` | Open the administration menu |
| `/vcmc:mute <player\|selector> <true\|false>` | Force or restore a player's microphone |
| `/vcmc:groups-settings <group> <global\|external\|environmental> <true\|false>` | Change group routing |
| `/vcmc:groups-admin create <group 1-255>` | Create an administrative group |
| `/vcmc:groups-admin move <group\|0> <player\|selector>` | Move players between groups |
| `/vcmc:groups-admin leave <player\|selector>` | Remove players from a group |
| `/vcmc:groups-admin list` | List administrative groups |
| `/vcmc:groups-admin delete <group>` | Delete an administrative group |
| `/vcmc:megaphone <player\|selector> <true\|false>` | Enable or disable megaphone |
| `/vcmc:sfx add <name> <json>` | Create a custom effect |
| `/vcmc:sfx delete <name>` | Delete a custom effect |
| `/vcmc:sfx list` | List custom effects |
| `/vcmc:sfx-player set <player\|selector> <effect>` | Assign a built-in effect |
| `/vcmc:sfx-player set <player\|selector> custom <name>` | Assign a custom effect |
| `/vcmc:sfx-player clear <player\|selector>` | Clear an effect |

### Custom SFX on Java

The Java command accepts a JSON object directly:

```text
/vcmc:sfx add grave {"base":"normal","pitch":-4,"gain":2,"lowpass":4200,"highpass":80,"q":0.8,"distortion":1.5,"dry":1}
```

The Bedrock Addon uses a quoted and escaped JSON string instead.

## Configuration

VCMC creates its room values automatically:

```yaml
vcmc-room-id: ""
vcmc-room-token: ""

server-ip: ""
bedrock-port: 19132

rp-port: 25580
rp-host: ""

settings:
  starter-books: true
  nametag-mic-only-while-speaking: true
```

- Do not share or manually replace `vcmc-room-token`.
- Leave `server-ip` empty for automatic public-IP detection.
- Set `rp-host` only when the resource pack must use a specific public host.
- Open or forward `rp-port` if Java clients cannot receive the pack.
- Server settings can also be changed from `/vcmc:admin`.

## PlaceholderAPI

VCMC includes its own PlaceholderAPI expansion. No eCloud download is required.

| Placeholder | Value |
|---|---|
| `%vcmc_mic_icon%` | Current voice-state icon |
| `%vcmc_mic_state%` | `disconnected`, `muted`, `idle`, or `speaking` |
| `%vcmc_connected%` | `true` or `false` |
| `%vcmc_muted%` | `true` or `false` |
| `%vcmc_speaking%` | `true` or `false` |
| `%vcmc_voice_level%` | Current level from `0` to `100` |
| `%vcmc_megaphone_icon%` | Megaphone icon or an empty value |
| `%vcmc_megaphone%` | `true` or `false` |
| `%vcmc_disconnected_icon%` | Disconnection icon or an empty value |
| `%vcmc_disconnected%` | `true` or `false` |

`%vcmc_icon%` and `%vcmc_state%` are aliases for the first two placeholders.

To let TAB or another formatting plugin control prefixes:

```yaml
settings:
  show-icons: true
  native-name-icons: false
```

See [PLACEHOLDERAPI.md](PLACEHOLDERAPI.md) for the complete integration notes.

## Removed Commands

`/vcmc:join`, `/vcmc:room`, and `/vcmc:reconnect` no longer exist. Room discovery and reconnection are automatic.

## Building from Source

```bash
mvn clean package
```

The release artifact is processed by ProGuard. Generated and obfuscated JARs belong in GitHub or CurseForge releases, not in source commits.

## Related Downloads

- [VCMC app](https://antoic.com/app.html)
- [Bedrock Addon](https://github.com/narucreations1-hub/Voice-Chat-Minecraft-Bedrock-VCMC-Addon)
- [Windows releases](https://github.com/NARUxd/Voice-Chat-Minecraft-PC-VCMC/releases)
- [Official documentation](https://antoic.com/docs/vcmc.html)
- [VCMC changelog](https://antoic.com/changelog/vcmc/)

## Help

- [Discord support](https://discord.gg/HA5gKcpsaq)
- [Repository issues](../../issues)

VCMC is a free, independent project created by Naru. It is not affiliated with Mojang Studios. Minecraft is a trademark of Mojang AB.
