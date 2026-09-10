# Custom Home Assistant Integrations

This directory documents custom or extended Home Assistant integrations used by the device guides in this repository.

The integration source code stays in its own repository. The pages here explain what changed, which release was tested, how to install it, and how to return to the official integration.

## Available integration

### Local Tuya AB

[Local Tuya AB](local-tuya-ab.md) is an unofficial fork of [LocalTuya](https://github.com/rospogrigio/localtuya). It adds:

- Tuya LAN protocol `3.5`
- Additional EUROM climate mappings
- Optional integer writes for Number entities

It is used by the newer EUROM heater and EMKE heated towel rack guides.

## Important notice

Custom integrations are not supported by the Home Assistant project. A wrong or incompatible version can stop devices from loading.

Create a complete Home Assistant backup before installation. Installation and use are at your own risk.
