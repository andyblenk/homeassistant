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
