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
