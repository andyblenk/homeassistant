# Home Assistant Device Integration Guides

This directory contains practical guides for integrating specific smart-home devices with Home Assistant. Each guide documents the tested protocol, integration version, Tuya data points, Home Assistant entities, and device-specific behavior.

These are configuration guides, not copies of integration source code. Custom integration code remains in its own repository and is linked where required.

## Available guides

| Device | Home Assistant integration | Local protocol | Guide |
|---|---|---:|---|
| Older EUROM electric heaters | LocalTuya | `3.3` | [EUROM electric heaters](eurom-electric-heaters.md) |
| Newer EUROM LPL-15W electric heaters | Local Tuya AB | `3.5` | [EUROM electric heaters](eurom-electric-heaters.md) |
| EMKE smart heated towel rack | Local Tuya AB | `3.5` | [EMKE heated towel rack](emke-heated-towel-rack.md) |

## Before following a guide

- Create a Home Assistant backup.
- Make sure the device works in Smart Life or the relevant vendor app.
- Reserve a stable local IP address for the device in your router.
- Follow the [Tuya Device ID and Local Key guide](../guides/tuya-device-id-local-key.md) when these credentials are required.
- Close Smart Life before connecting locally; some Tuya devices accept only one active local connection reliably.
- Do not connect the same device from production Home Assistant and a separate test instance at the same time.

The guides describe configurations confirmed with specific devices. A vendor may change hardware, firmware, data points, or Tuya protocol without changing the retail product name.
