# SHPE RC Car 🏎️📷

A first-person-view RC car built for the SHPE Robotics Showcase. A Raspberry Pi 5 streams live video from an on-board camera and reads an Xbox controller, then sends drive and steering commands over USB serial to an Arduino that runs the motors.

> **Status:** 🚧 Early development. The hardware plan is set, and the Pi control and streaming scripts are stubbed out but not written yet.

## System overview

```
 Xbox controller ──(Bluetooth/USB)──►┌──────────────────┐──(USB serial)──► Arduino Uno ──► Motor driver ──► Drive motor
                                     │  Raspberry Pi 5  │                                └──► Steering (servo/DC)
 Camera (Pi Cam or USB 1080p) ──────►│  control + video │──(Wi-Fi stream)──► Driver's laptop
                                     └──────────────────┘
```

| Component | Role | Status |
|---|---|---|
| Raspberry Pi 5 | Video streaming, gamepad input, main controller | ✅ Chosen |
| Arduino Uno | Real-time motor control, receives commands over USB serial | ✅ Chosen |
| Camera | Pi Camera or USB 1080p webcam | Deciding |
| Game controller | Xbox controller | Planned |
| Motor driver | Drives the main motor | TBD |
| Steering actuator | Servo or DC motor | TBD |
| Battery | Likely a 2S LiPo (7.4 V), depending on the motors | TBD |
| Buck converter | Steps battery voltage down to a clean 5 V for the Pi | Planned |

## Power

```
Battery
 ├─► Buck converter (5 V) ─► Raspberry Pi 5  (~3–5 A)
 │                             └─► Arduino via USB  (~500 mA)
 └─► Motor driver (VM / 12 V pins)   [logic VCC + GND from the Arduino via breadboard]

COMMON GROUND: Pi, Uno, motor driver, buck converter and battery grounds must all be tied together.
```

**Peak current estimate:** about **9 A**

| Load | Current |
|---|---|
| Drive motor | ~3 A |
| Steering | ~1 A |
| Pi 5 | ~4 A |
| Headroom | ~1 A |

## Repo layout

```
src/pi/
├── pi_control.py          # gamepad → serial → Arduino control loop (WIP)
└── stream_usb_ffmpeg.sh   # USB camera stream via ffmpeg (WIP)
requirement.txt            # Python deps: evdev, pyserial
```

## Quick start (on the Pi)

```bash
# one-time setup
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirement.txt

# run the control loop (once implemented)
python src/pi/pi_control.py
```

## Roadmap

- [ ] Pick the motor driver, steering actuator and battery
- [ ] Camera streaming script (`stream_usb_ffmpeg.sh`)
- [ ] Gamepad → serial control loop (`pi_control.py`) using `evdev` + `pyserial`
- [ ] Arduino firmware to read serial commands and drive the motors
- [ ] Assemble the chassis and wiring
- [ ] Showcase 🎉

## License

See [LICENSE](LICENSE).
