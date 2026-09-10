# EMKE Heated Towel Rack in Home Assistant with LocalTuya

This guide shows how to add an EMKE smart heated towel rack to Home Assistant. The setup provides separate controls and sensors for power, target temperature, timer, remaining time, current temperature, and operating state.

The tested device is shown by Tuya as `Smart Towel Rack` or `Heated Towel Rack`, model `119`, product ID `yghsoyzoicezv8sy`, and category `mjj`.

## What you need

| Requirement | Tested value |
|---|---|
| Tuya LAN protocol | `3.5` |
| Home Assistant integration | [Local Tuya AB](../custom-integrations/local-tuya-ab.md) |
| Integration release | `v5.2.3-ab.2` |
| DP detection | Enter the DPs manually |

The tested official LocalTuya version does not support this protocol 3.5 device. Local Tuya AB adds protocol 3.5 and the integer option needed for the target temperature.

Before you begin:

- Pair the towel rack with Smart Life and check that it is online.
- Reserve a stable local IP address in your router.
- Get its Device ID and Local Key. Follow the [Tuya Device ID and Local Key guide](../guides/tuya-device-id-local-key.md).
- Close Smart Life completely before Home Assistant connects.
- Make sure no second Home Assistant or test tool is connected to the device.

## Add the device

In Home Assistant, open **Settings → Devices & services → LocalTuya → Configure → Add a new device**.

| Setting | Value |
|---|---|
| Name | For example `Bathroom heated towel rack` |
| Host | Current local IP address |
| Device ID | Current Device ID |
| Local Key | Current Local Key |
| Protocol Version | `3.5` |
| Enable debugging | Off |
| Scan interval | Empty |
| Manual DPS | `1,2,3,9,10,11,12,13,14,101` |
| DPIDs for RESET command | Empty |

Automatic DP detection was not reliable with this device. The manual list above worked.

Do not create one Climate entity. Add the following entities one after another.

## Power switch

Entity type: `switch`

| Setting | Value |
|---|---|
| ID | DP `1` |
| Friendly name | For example `Bathroom heated towel rack` |
| Current / Current Consumption / Voltage | Empty |
| Restore the last value after a lost connection | Off |
| Passive entity | Off |
| Default value | Empty |

## Target temperature

Entity type: `number`

| Setting | Value |
|---|---|
| ID | DP `2` |
| Friendly name | `Target temperature` |
| Minimum | `30` |
| Maximum | `70` |
| Minimum increment | `5` |
| Send values as integers | **On** |
| Restore the last value after a lost connection | Off |
| Passive entity | Off |
| Default value | Empty |

The **Send values as integers** option is important. Without it, the towel rack can ignore a new target temperature.

## Timer

Entity type: `select`

| Setting | Value |
|---|---|
| ID | DP `12` |
| Friendly name | `Timer` |
| Valid entries | `cancel;1h;2h;3h;4h;5h;6h;7h;8h;9h;10h;11h;12h;13h;14h;15h;16h;17h;18h;19h;20h;21h;22h;23h;24h` |
| Friendly entries | `Off;1h;2h;3h;4h;5h;6h;7h;8h;9h;10h;11h;12h;13h;14h;15h;16h;17h;18h;19h;20h;21h;22h;23h;24h` |
| Restore the last value after a lost connection | Off |
| Passive entity | Off |
| Default value | Empty |

Home Assistant displays `Off`, but sends the value `cancel` to the device. DP `13` shows the remaining time in minutes.

## Sensors

Add these Sensor entities:

| DP | Suggested name | Unit | Device class | Scaling factor |
|---:|---|---|---|---:|
| `3` | `Temperature` | `°C` | Temperature | `1` |
| `101` | `Internal temperature` | `°C` | Temperature | `1` |
| `13` | `Remaining time` | `min` | Duration | `1` |
| `14` | `Operating state` | Empty | None | `1` |

DP `14` reports values such as `heating` and `standby`.

After the final Sensor entity, enable **Do not add any more entities** and finish the setup.

## DP overview

A DP, or data point, is one value or function of a Tuya device.

| DP | Tuya code | Function |
|---:|---|---|
| `1` | `switch` | Power |
| `2` | `temp_set` | Target temperature, 30–70 °C in 5 °C steps |
| `3` | `temp_current` | Current temperature |
| `9` | `temp_unit_convert` | Temperature unit; no entity needed |
| `10` | `temp_set_f` | Fahrenheit target temperature; not used |
| `11` | `temp_current_f` | Fahrenheit current temperature; not used |
| `12` | `countdown_set` | Timer from `1h` to `24h`, or `cancel` |
| `13` | `countdown_left` | Remaining time in minutes |
| `14` | `work_state` | Operating state such as `heating` or `standby` |
| `101` | `inside_temp` | Internal temperature |

## If the device is unavailable

1. Close Smart Life completely.
2. Stop other Home Assistant test systems or Tuya tools using this device.
3. Check the local IP address.
4. Check the Local Key, especially after pairing again.
5. Reload LocalTuya or restart Home Assistant.
6. Turn the towel rack off at the power supply for a short time if needed.

The Local Key can change when the device is paired again.
