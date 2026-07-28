# PlaceholderAPI support

VCMC includes its PlaceholderAPI expansion internally. PlaceholderAPI is optional,
and no separate VCMC expansion needs to be downloaded from the eCloud.

Available placeholders:

| Placeholder | Value |
| --- | --- |
| `%vcmc_mic_icon%` | Current VCMC voice icon (recommended for TAB/nametags) |
| `%vcmc_mic_state%` | `disconnected`, `muted`, `idle`, or `speaking` |
| `%vcmc_connected%` | `true` or `false` |
| `%vcmc_muted%` | `true` or `false` |
| `%vcmc_speaking%` | `true` or `false` |
| `%vcmc_voice_level%` | Current voice level from `0` to `100` |
| `%vcmc_megaphone_icon%` | Megaphone icon while the player has megaphone enabled; empty otherwise |
| `%vcmc_megaphone%` | `true` or `false` |
| `%vcmc_disconnected_icon%` | Disconnection icon while the player is disconnected; empty otherwise |
| `%vcmc_disconnected%` | `true` or `false` |

`%vcmc_icon%` and `%vcmc_state%` are shorter aliases for the first two.

To let TAB, a nametag plugin, or another formatter own the player's prefix, use:

```yaml
settings:
  show-icons: true
  native-name-icons: false
```

Then add `%vcmc_mic_icon%` wherever that plugin formats the player name. VCMC will
continue updating placeholder values, but it will not create a scoreboard team,
replace a team prefix, or replace the Java player-list name.

The icon glyphs are supplied by VCMC's Java resource pack, as with native icons.
