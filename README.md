# Smart Home Blueprints and Scripts
Collection of Home Assistant blueprints, automation examples, and practical scripts for real-world smart-home setups.

## Home Assistant Blueprints

### Smart Motion Light Control
Path: `blueprints/smart_motion_light_control.yaml`

Turns lights on when motion is detected while respecting ambient brightness (lux) and a user-defined light mode (auto/on/off). Uses a timer for auto-off and a short cooldown window where lux is ignored to prevent flicker.

Requires an `input_select` with values `auto`, `on`, `off` to control the mode:
- `auto`: motion-based control using the configured settings
- `on`: always on
- `off`: always off (motion ignored)

Supports multiple motion sensors and multiple lights that can be switched together.

Inputs:
- Motion sensors (binary_sensor/switch)
- Light switches
- Lux sensor and threshold
- Light mode input_select with labels for auto/on/off
- Timer entity and auto-off durations

### HomePods Doorbell
Path: `blueprints/homepod_doorbell.yaml`

Plays a doorbell sound on selected HomePods/AirPlay speakers when a doorbell trigger fires. Supports adjustable volume and a custom MP3 path.

Inputs:
- Doorbell trigger entity
- Media players (HomePods/AirPlay)
- Volume (0.1–1.0)
- Ringtone path (MP3)

### Limodor Fan Control
Path: `blueprints/limodor_control.yaml`

Controls the signal line of a Limodor fan while leaving its permanent power supply untouched. The fan can be requested by up to three state-based entities or Input Buttons, three independent daily schedules, and an optional humidity controller with up to two sensors.

State-based sources support individual start delays and minimum signal durations. Input Buttons create timed requests. Schedules support selectable weekdays, configurable durations, and optional on/off conditions. Humidity control can react to a rising value above an activation threshold and periodically recheck for persistently high humidity.

Overlapping requests run in parallel. The signal line remains on until the last active request ends. A configurable global safety timeout forces the signal line off if it remains active too long. On Home Assistant startup or automation reload, active sources and humidity conditions are reevaluated deterministically.

Important:
- Requires Home Assistant 2024.10.0 or newer
- Controls only the Limodor signal line, not its permanent power supply
- The Limodor's internal run-on time starts after the signal line is switched off and is not included in configured durations
- Humidity control requires a dedicated Timer helper for cycle lockout

Inputs:
- Limodor signal switch and global safety timeout
- Up to three source entities (light, switch, binary sensor, Input Boolean, or Input Button)
- Per-source start delay and minimum signal duration
- Up to three schedules with weekdays, duration, and optional condition
- Up to two humidity sensors
- Activation and persistent-humidity thresholds
- Humidity signal duration, periodic check interval, recheck pause, and Timer helper

### Electric Heater Control
Path: `blueprints/electric_heater_control.yaml`

Controls an electric heater exposed as a Climate entity. It compares a room temperature sensor with the target temperature of another Climate entity and selects `off`, low, medium, or optionally high power. Preset names are configurable for compatibility with different heaters. High can be limited to a configurable duration per heating cycle and then falls back to the normal medium/low regulation.

Automatic control, presence, and a window contact provide the main safety conditions. Normal heating can optionally depend on PV power or battery state of charge. A Timer can temporarily request continuous heating at medium power, while PV surplus heating can use a directly configured higher target temperature after a configured start time.

A configurable continuous-runtime limit switches the heater off after five hours by default. A later regular trigger may start a new heating cycle when all conditions are still met.

Inputs:
- Electric-heater and target-temperature Climate entities
- Room temperature sensor
- Optional automatic-control Input Boolean
- Presence entity and optional window sensor
- Optional continuous-heating Timer
- Low, medium, and optional high presets with temperature thresholds and a maximum high duration
- Optional PV-power and battery sensors with thresholds
- Optional PV-surplus target temperature, PV threshold, and start time
- Optional Electric-heater-only Input Boolean output
- Maximum continuous heating duration, default 5 hours

### Room Climate Temperature Control
Path: `blueprints/room_climate_temperature_control.yaml`

Controls the target temperature of one room. A Home Assistant Climate entity acts as the master and synchronizes target-temperature changes bidirectionally with optional additional wall or hardware thermostats. Only target temperatures are synchronized; HVAC modes, presets, and measured temperatures remain untouched.

Manual changes from any connected thermostat are rounded to a common step, stored in an Input Number, and distributed to the other thermostats without creating synchronization loops. Selected Reduced conditions, such as vacation or nobody at home, apply Reduced. Deliberate preheating can override them through any synchronized thermostat or the Heating/Eco switch. Optional Comfort conditions replace Normal with Comfort immediately and return to Normal only after all conditions have remained off for five minutes. Up to four optional weekday-aware time windows can independently apply Reduced, Eco, or Heat. A manual temperature or Heating/Eco change overrides only the currently active time window; the next window starts normally.

Inputs:
- Master Climate entity and optional additional Climate thermostats
- Desired-temperature Input Number
- Heating/Eco Input Boolean
- Normal, Comfort, Eco, and Reduced temperatures
- Optional Reduced-condition Boolean, binary-sensor, and Timer entities
- Optional Comfort-condition entities
- Up to four optional weekday-aware time windows using Reduced, Eco, or Heat
- Up to three optional heating safeguard times with one shared blocker entity

### Viessmann Heating Control
Path: `blueprints/viessmann_heating_control.yaml`

Controls the operating mode of a Viessmann heating system through the ViCare integration. Away conditions select `standby`, disabling heating and domestic hot water. An optional Pre-heat Timer overrides absence and selects `dhw` during summer or `dhwAndHeating` otherwise. Summer conditions select `dhw` for domestic hot water only. With no active condition, `dhwAndHeating` restores normal scheduled heating and hot water operation.

The blueprint accepts multiple Boolean, binary-sensor, or Timer entities for both condition groups. It reevaluates changes immediately and performs a 15-minute safety check while avoiding repeated ViCare calls when the correct mode is already active.

Inputs:
- Viessmann ViCare Climate entity
- Optional away-condition entities, such as nobody at home or vacation
- Optional summer-condition entities
- Optional Pre-heat Timer
