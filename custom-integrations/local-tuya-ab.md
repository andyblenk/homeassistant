# Local Tuya AB: LocalTuya with Tuya Protocol 3.5

Local Tuya AB is an unofficial fork of [LocalTuya by rospogrigio](https://github.com/rospogrigio/localtuya). It adds features needed by newer EUROM electric heaters and an EMKE heated towel rack.

The source code is available in the separate [andyblenk/localtuya repository](https://github.com/andyblenk/localtuya).

> **Warning:** This custom integration is provided without warranty. Create a complete Home Assistant backup before installing it. Installation and use are at your own risk.

## Compatibility with official LocalTuya

Local Tuya AB keeps the important parts of official LocalTuya unchanged:

- Home Assistant domain: `localtuya`
- Installation directory: `/config/custom_components/localtuya`
- Existing LocalTuya configuration format

This means it can replace the tested official LocalTuya version without recreating existing devices. However, it is not a second, parallel integration. Official LocalTuya and Local Tuya AB cannot be installed at the same time because both use the same domain and directory.

When changing the installed package, do not delete LocalTuya under **Settings → Devices & services**. That entry contains the saved device configuration.

Compatibility was confirmed with the releases listed below. Future Home Assistant or LocalTuya updates can require new tests.

## Releases

### v5.2.3-ab.1

[Open GitHub release](https://github.com/andyblenk/localtuya/releases/tag/v5.2.3-ab.1)

This release is based on LocalTuya 5.2.3 and adds:

- Tuya LAN protocol `3.5`
- Protocol 3.5 session-key handling and encryption
- Status updates, commands, push messages, and heartbeats for protocol 3.5
- EUROM mode mapping `manual (m) / automatic (p)`
- EUROM power levels `low/mid/high/off`

The release was tested in a separate Home Assistant Docker instance and then with real EUROM LPL-15W heaters.

### v5.2.3-ab.2

[Open GitHub release](https://github.com/andyblenk/localtuya/releases/tag/v5.2.3-ab.2)

This release contains everything from `v5.2.3-ab.1` and adds:

- Optional **Send values as integers** setting for Number entities
- Integer conversion only when the option is enabled
- Unchanged behavior when the option is not enabled

Some Tuya devices ignore a whole number when Home Assistant sends it as a floating-point value. The new option solves this without changing existing Number entities. It was tested with the target temperature of the EMKE heated towel rack.

## Devices using Local Tuya AB

- [Newer EUROM LPL-15W electric heaters](../devices/eurom-electric-heaters.md) use protocol `3.5` and the additional climate mappings.
- [EMKE heated towel rack](../devices/emke-heated-towel-rack.md) uses protocol `3.5` and the integer Number option.

## Install with HACS without losing existing devices

These steps replace the LocalTuya files but keep the saved Home Assistant configuration.

1. Create a complete Home Assistant backup.
2. Note the currently installed LocalTuya version.
3. In HACS, remove or uninstall the downloaded official LocalTuya package.
4. **Do not delete LocalTuya under Settings → Devices & services.**
5. In HACS, open **Custom repositories**.
6. Add `https://github.com/andyblenk/localtuya` as an **Integration**.
7. Open Local Tuya AB and select `v5.2.3-ab.2`.
8. Download the release. HACS should install it in `/config/custom_components/localtuya`.
9. Restart Home Assistant completely.
10. Check the existing LocalTuya devices before adding new protocol 3.5 devices.

The existing devices remain because Home Assistant stores their settings separately and Local Tuya AB uses the same `localtuya` domain. Deleting the Config Entry under **Devices & services** would remove those saved settings.

## Update Local Tuya AB

1. Create a new Home Assistant backup.
2. Read the release notes.
3. Select and download the new release in HACS.
4. Restart Home Assistant.
5. Check existing devices and the functions changed by the release.

## Return to official LocalTuya

1. Restore the backup if the update damaged the configuration.
2. Otherwise, replace the HACS package with a compatible official LocalTuya release.
3. Do not delete the LocalTuya Config Entry.
4. Restart Home Assistant and check the devices.

Devices that require protocol `3.5` will be unavailable if the installed official version does not support protocol 3.5. Older protocol 3.3 devices should keep working when the official release remains compatible with their saved configuration.

## License and attribution

Local Tuya AB is an unofficial fork. It is not affiliated with or endorsed by the LocalTuya maintainers, Tuya, or Home Assistant.

The upstream project and this fork use the [GNU General Public License v3.0](https://github.com/andyblenk/localtuya/blob/v5.2.3-ab.2/LICENSE). The original license, history, and attribution are retained. The complete modified source code is public.

- Upstream project: <https://github.com/rospogrigio/localtuya>
- Extended fork: <https://github.com/andyblenk/localtuya>
