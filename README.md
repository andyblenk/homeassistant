# Home Assistant Blueprints and Device Integration Guides

Practical Home Assistant blueprints, device setup guides, and documentation for custom integrations. The configurations in this repository were created for real smart-home installations and are documented so they can be understood, reproduced, and adapted.

The repository focuses on useful solutions rather than large frameworks. Each guide explains the relevant Home Assistant settings, device data points, compatibility requirements, and known limitations.

## Repository contents

| Area | Description |
|---|---|
| [Blueprints](blueprints/README.md) | Reusable Home Assistant automation blueprints for heating, lighting, ventilation, doorbells, and room control. |
| [Device guides](devices/README.md) | Tested setup instructions for integrating specific smart-home devices with Home Assistant. |
| [General guides](guides/README.md) | Shared procedures such as finding a Tuya Device ID and Local Key. |
| [Custom integrations](custom-integrations/README.md) | Documentation for custom or extended Home Assistant integrations used by these device guides. |

## Available device guides

- [EUROM electric heaters with LocalTuya](devices/eurom-electric-heaters.md) — covers both EUROM hardware generations, including newer Tuya protocol 3.5 models.
- [EMKE heated towel rack with LocalTuya](devices/emke-heated-towel-rack.md) — switch, target temperature, timer, remaining time, temperature, and operating-state entities.

## Available custom integration

- [Local Tuya AB](custom-integrations/local-tuya-ab.md) — an unofficial LocalTuya fork adding Tuya protocol 3.5 support, EUROM climate mappings, and optional integer writes for Number entities.

## Philosophy

- **Practical:** configurations are based on devices and automations used in a real Home Assistant installation.
- **Reproducible:** important parameters, data points, versions, and setup steps are recorded.
- **Local-first:** local device control is preferred where it provides reliable operation and useful Home Assistant entities.
- **Transparent:** custom changes and differences from upstream projects are documented clearly.
- **Focused:** the repository contains only solutions that are useful and maintainable.
- **Safe to share:** passwords, tokens, Local Keys, private Device IDs, and private network addresses are never published.

## Important notice

These projects and guides are provided without warranty. Create a Home Assistant backup before replacing integrations or changing an existing configuration. Device firmware, Home Assistant, HACS, and third-party integrations can change over time, so verify that a guide still matches your environment.

Home Assistant, Tuya, Smart Life, LocalTuya, EUROM, EMKE, Viessmann, HomePod, and other product names belong to their respective owners. This repository is not officially affiliated with or endorsed by those projects or companies.
