# Home Assistant Automation Blueprints

This directory contains reusable Home Assistant automation blueprints for practical smart-home tasks. The collection includes motion-based lighting, electric heating, room temperature synchronization, Viessmann heating modes, Limodor ventilation, and HomePod doorbell announcements.

The blueprints are configurable and reusable. Device and entity IDs are selected when a blueprint is imported and are not hard-coded into the YAML files.

## Importing a blueprint

1. Open **Settings → Automations & scenes → Blueprints** in Home Assistant.
2. Select **Import Blueprint**.
3. Paste the raw GitHub URL shown for the desired blueprint.
4. Import it and create an automation from the blueprint.

You can also copy a YAML file manually into the appropriate Home Assistant blueprint directory. Review the required helpers, timers, entities, and minimum Home Assistant version before creating an automation.

## Electric Heater Control

[Source](electric_heater_control.yaml) · [Raw import URL](https://raw.githubusercontent.com/andyblenk/homeassistant/main/blueprints/electric_heater_control.yaml)

Controls an electric heater exposed as a Home Assistant Climate entity. It compares the current room temperature with the target temperature of another Climate entity and selects `off`, low, medium, or optionally high power.

The high-power preset can be limited to a configurable duration. Automatic control, presence, and window contacts provide the main safety conditions. Normal heating can optionally depend on photovoltaic power or battery state of charge. An optional Input Boolean can dynamically ignore these energy restrictions. A Home Assistant Timer can temporarily request continuous heating, while a configurable maximum runtime prevents the heater from remaining enabled indefinitely.

Main inputs:

- Electric-heater and target-temperature Climate entities
- Room temperature sensor
- Optional automatic-control helper
- Presence entity and optional multiple window sensors
- Optional Timer for continuous heating
- Configurable low, medium, and high presets
- Optional photovoltaic-power and battery sensors
- Optional Input Boolean to override photovoltaic and battery restrictions
- Maximum continuous heating duration

## HomePods Doorbell

[Source](homepod_doorbell.yaml) · [Raw import URL](https://raw.githubusercontent.com/andyblenk/homeassistant/main/blueprints/homepod_doorbell.yaml)

Plays a configurable doorbell sound on selected HomePod or AirPlay media players when a doorbell entity is triggered. Multiple speakers, playback volume, and the media path can be configured.

Main inputs:

- Doorbell trigger entity
- One or more HomePod or AirPlay media players
- Playback volume
- MP3 ringtone path

## Limodor Fan Control

[Source](limodor_control.yaml) · [Raw import URL](https://raw.githubusercontent.com/andyblenk/homeassistant/main/blueprints/limodor_control.yaml)

Controls the signal line of a Limodor ventilation fan while leaving its permanent power supply untouched. The fan can be requested by state-based entities, Input Buttons, daily schedules, and an optional humidity controller using up to two sensors.

Overlapping requests run in parallel, and the signal remains active until the last request ends. A global safety timeout prevents an unexpectedly long signal. On Home Assistant startup or after automation reload, active sources and humidity conditions are evaluated again.

Important:

- Requires Home Assistant 2024.10.0 or newer
- Controls only the Limodor signal line, not its permanent power supply
- The Limodor's own run-on time starts after the signal line is switched off
- Humidity control needs a separate Home Assistant Timer helper

Main inputs:

- Limodor signal switch and global safety timeout
- Up to three source entities with individual delays and minimum durations
- Up to three weekday-aware schedules
- Up to two humidity sensors
- Humidity thresholds, check interval, and Timer helper

## Room Climate Temperature Control

[Source](room_climate_temperature_control.yaml) · [Raw import URL](https://raw.githubusercontent.com/andyblenk/homeassistant/main/blueprints/room_climate_temperature_control.yaml)

Synchronizes the target temperature between a master Home Assistant Climate entity and optional additional thermostats. Only target temperatures are synchronized; HVAC modes, presets, and measured temperatures remain unchanged.

An Input Boolean selects **Manual** or **Automatic** operation. A manual temperature change on any connected thermostat enables Manual, synchronizes that temperature to the other thermostats, and holds it. With Automatic selected, Reduced, Comfort, Normal, Eco, and Heat behavior is determined by conditions and weekday-aware time windows. Entering Reduced always applies the Reduced temperature; a later manual change can deliberately preheat. Optional safeguards end Manual and return to Automatic.

Main inputs:

- Master Climate entity and optional additional thermostats
- Manual/Automatic Input Boolean
- Normal, Comfort, Eco, and Reduced temperatures
- Optional Reduced, Comfort, and Normal conditions
- Up to four weekday-aware time windows
- Optional heating safeguard times

## Smart Motion Light Control

[Source](smart_motion_light_control.yaml) · [Raw import URL](https://raw.githubusercontent.com/andyblenk/homeassistant/main/blueprints/smart_motion_light_control.yaml)

Controls one or more lights using motion sensors and ambient brightness. A configurable Input Select provides `auto`, `on`, and `off` modes. The automation includes automatic switch-off and a short period in which the lux value is ignored to prevent flickering after the light changes the measured brightness.

Main inputs:

- One or more motion sensors
- One or more light switches
- Lux sensor and threshold
- Input Select with `auto`, `on`, and `off` values
- Timer entity and automatic switch-off durations

## Viessmann Heating Control

[Source](viessmann_heating_control.yaml) · [Raw import URL](https://raw.githubusercontent.com/andyblenk/homeassistant/main/blueprints/viessmann_heating_control.yaml)

Controls a Viessmann heating system through the Home Assistant ViCare integration. Summer conditions select domestic-hot-water-only mode `dhw`; combined summer and away conditions select `standby`, disabling both heating and domestic hot water. Away conditions outside summer select ViCare `forcedReduced`: heating remains at the boiler's configured reduced temperature and domestic hot water is disabled. Normal operation uses `dhwAndHeating`.

An optional preheat Timer can override absence. The automation checks the active ViCare mode before sending a command and periodically verifies the required state without producing unnecessary service calls.

Main inputs:

- Viessmann ViCare Climate entity
- Optional away-condition entities
- Optional summer-condition entities
- Optional preheat Timer
