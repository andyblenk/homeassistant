# EUROM Electric Heaters in Home Assistant with LocalTuya

This guide explains how to integrate EUROM Wi-Fi electric heaters into Home Assistant using LocalTuya. Two visually similar EUROM heater generations use different Tuya data points and different local protocols. Selecting the correct variant is essential.

Both configurations below were tested with real devices. Do not mix the protocol or data-point mappings between the two generations.

## Identify the EUROM heater generation

The Tuya product profile shown in the Tuya Developer Platform is the most reliable identifier.

| Variant | Tuya product profile | Protocol | Mode values | Power-level DP | Required integration |
|---|---|---:|---|---:|---|
| Older generation | `LPL-15 HEATER（新版周程序）EUROM` | `3.3` | `m` / `p` | `101` | LocalTuya or Local Tuya AB |
| Newer generation | `LPL-15W WIFI thermostat EUROM` | `3.5` | `manual` / `auto` | `5` | [Local Tuya AB](../custom-integrations/local-tuya-ab.md) |

The tested upstream LocalTuya version does not provide Tuya LAN protocol 3.5. Newer LPL-15W heaters therefore require the unofficial Local Tuya AB fork. The fork uses the same `localtuya` Home Assistant domain and remains compatible with existing LocalTuya configuration entries.

## Common requirements

- The heater is paired and online in Smart Life.
- Its current local IP address, Device ID, and Local Key are available. See [Find a Tuya Device ID and Local Key](../guides/tuya-device-id-local-key.md).
- Smart Life is completely closed before LocalTuya connects.
- Only one Home Assistant or diagnostic instance connects to the heater.
- A new Local Key is obtained after pairing the heater again.

In Home Assistant, open **Settings → Devices & services → LocalTuya → Configure → Add a new device**.

## Older EUROM heater: protocol 3.3

### Device settings

| Setting | Value |
|---|---|
| Name | Any descriptive name |
| Host | Current local IP address |
| Device ID | Current Device ID |
| Local Key | Current Local Key |
| Protocol Version | `3.3` |
| Enable debugging | Off |
| Scan interval | Empty |
| Manual DPS | Empty if discovery works; otherwise `1,2,3,4,12,101,102,103,104` |
| DPIDs for RESET command | Empty |

Select `climate` as the entity type after submitting the device settings. When manual data points are used, values shown as `-1` during configuration are expected until a real status update is received.

### Climate entity settings

| Setting | Value |
|---|---|
| ID | DP `1` |
| Friendly name | Any descriptive name |
| Target Temperature | DP `2` |
| Current Temperature | DP `3` |
| Temperature Step | `1` |
| Minimum / Maximum Temperature Constant | `7` / `35` |
| Maximum / Minimum Temperature DP | Empty |
| Precision | `1` |
| HVAC Mode DP | DP `4` |
| HVAC Mode Set | `manual (m) / automatic (p)` |
| HVAC Fan Mode DP / Set | Empty |
| HVAC Current Action DP | DP `1` |
| HVAC Current Action Set | `True/False` |
| Eco DP / value | Empty |
| Presets DP | DP `101` |
| Presets Set | `low/mid/high/off` |
| Temperature Unit | `celsius` |
| Target Precision | `1` |
| Enable heuristic action | Off |

Enable **Do not add any more entities** after this Climate entity and finish the setup.

### Older-generation data points

| DP | Function |
|---:|---|
| `1` | Power |
| `2` | Target temperature |
| `3` | Current temperature |
| `4` | Manual `m` / automatic `p` mode |
| `12` | Fault state |
| `101` | Power level: `low`, `mid`, `high`, `off` |
| `102` | Eco mode |
| `103` | Timer data |
| `104` | Smart timer |

## Newer EUROM LPL-15W heater: protocol 3.5

The newer heater requires genuine Tuya LAN protocol `3.5`. Protocol `3.3`, the LocalTuya `3.22` option, or manually forcing data points does not replace protocol 3.5 support.

Install [Local Tuya AB](../custom-integrations/local-tuya-ab.md) before adding this variant. Releases `v5.2.3-ab.1` and `v5.2.3-ab.2` contain the required protocol implementation; `v5.2.3-ab.2` is the current tested release.

### Device settings

| Setting | Value |
|---|---|
| Name | Any descriptive name |
| Host | Current local IP address |
| Device ID | Current Device ID |
| Local Key | Current Local Key |
| Protocol Version | `3.5` |
| Enable debugging | Off under normal operation |
| Scan interval | Empty |
| Manual DPS | Empty; use automatic discovery |
| DPIDs for RESET command | Empty |

Select `climate` after submitting the device settings.

### Climate entity settings

| Setting | Value |
|---|---|
| ID | DP `1` |
| Friendly name | Any descriptive name |
| Target Temperature | DP `2` |
| Current Temperature | DP `3` |
| Temperature Step | `1` |
| Minimum Temperature Constant | `7` practical minimum; Tuya model allows `0` |
| Maximum Temperature Constant | `37` |
| Maximum / Minimum Temperature DP | Empty |
| Precision | `1` |
| HVAC Mode DP | DP `4` |
| HVAC Mode Set | `manual/auto` |
| HVAC Fan Mode DP / Set | Empty |
| HVAC Current Action DP | DP `107` |
| HVAC Current Action Set | `True/False` |
| Eco DP / value | Empty |
| Presets DP | DP `5` |
| Presets Set | `low/mid/high/off` |
| Temperature Unit | `celsius` |
| Target Precision | `1` |
| Enable heuristic action | Off |

Enable **Do not add any more entities** and finish the setup.

### Newer-generation data points

The successful protocol 3.5 connection discovered these local data points:

`1,2,3,4,5,6,21,101,102,103,104,105,107,109`

| DP | Tuya code | Function |
|---:|---|---|
| `1` | `switch` | Power |
| `2` | `temp_set` | Target temperature, 0–37 °C, step 1 |
| `3` | `temp_current` | Current temperature |
| `4` | `mode` | `manual` / `auto` |
| `5` | `level` | `low` / `mid` / `high` / `off` |
| `6` | `eco` | Eco control |
| `21` | `fault` | Fault state |
| `101` | `sensor_temp` | Optional sensor temperature |
| `102` | `sensor_temp_set` | Optional sensor target temperature |
| `103` | `sensor_switch` | Optional sensor switch |
| `104` | `sensor_status` | Optional sensor binding state |
| `105` | `sensor_control` | Optional sensor group control |
| `107` | `sensor_heating` | Actual heating state |
| `109` | `eco_status` | Eco state |

The Tuya cloud model also defines DPs `23`, `106`, and `108`. They were not required for the Home Assistant Climate entity.

## Troubleshooting an unavailable EUROM heater

1. Close Smart Life completely, including the app switcher.
2. Stop any second Home Assistant or Tuya diagnostic instance using the same heater.
3. Verify the reserved IP address.
4. Verify the Local Key, especially after pairing again.
5. Reload LocalTuya or restart Home Assistant.
6. Power-cycle the heater if necessary.
7. For an LPL-15W device, verify that Local Tuya AB and protocol `3.5` are selected.

An open TCP port `6668` only proves that the device accepts a network connection. It does not prove that the Local Key, protocol selection, encryption, or Tuya session handshake is correct.
