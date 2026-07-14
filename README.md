# OVlink
License-free digital video transmission module using ESP32 — 技適対応・免許不要

# OVLink - License-Free Digital Video Transmission Module

<p align="center">
  <strong>Real-time HD video transmission over 2.4GHz WiFi — no license required</strong>
</p>

OVLink is a compact, low-latency digital video transmission module built on the ESP32 ecosystem. Designed for FPV drones, robotics, education, and industrial monitoring, OVLink uses license-free 2.4GHz WiFi with certified radio modules, making it legal to use without any amateur radio license.

> 🇯🇵 日本では技適対応・免許不要で使用できます

[![OVLink Demo](https://img.youtube.com/vi/-McKLC0uo5M/0.jpg)](https://youtu.be/-McKLC0uo5M)

## Features

- **License-free operation** — Uses 技適 (Japan) certified 2.4GHz WiFi modules. No amateur radio license needed.
- **Low latency** — Targeting sub-50ms glass-to-glass latency for real-time FPV operation
- **HD video** — 720p @ 60fps target with H.264 hardware encoding
- **Smartphone display** — View live video on any Android phone via USB, no Raspberry Pi or dedicated ground station needed
- **Compact & lightweight** — Designed to fit on micro drones and small robots
- **Open development** — Development progress shared openly via Qiita and X

## Current Status

### ✅ HVGA Prototype (Working)

| Spec | Value |
|------|-------|
| Resolution | HVGA (480×320) |
| Frame rate | 30 fps |
| Latency | ~60-80ms |
| Range | ~100m line-of-sight |
| Encoding | MJPEG |
| Wireless | 2.4GHz WiFi (UDP) |
| Display | Custom Android app via USB CDC |

### 🚧 HD Version (In Development)

| Spec | Target |
|------|--------|
| Resolution | 720p (1280×720) |
| Frame rate | 60 fps |
| Latency | < 50ms |
| Encoding | H.264 (hardware) |
| Wireless | 2.4GHz WiFi (UDP) |
| Display | Custom Android app via USB CDC |

## System Architecture

### HVGA Version (Current)

```
[Camera OV3660] → [XIAO ESP32-S3 (TX)]
                         |
                    WiFi 2.4GHz (UDP)
                         |
                   [XIAO ESP32-S3 (RX)]
                         |
                      USB CDC
                         |
                   [Android App]
```

### HD Version (Planned)

```
[Camera OV5647] → [ESP32-P4 (H.264 Encode)]
  (MIPI CSI-2)              |
                          SPI
                             |
                   [XIAO ESP32-S3 (TX)]
                             |
                        WiFi 2.4GHz (UDP)
                             |
                   [XIAO ESP32-S3 (RX)]
                             |
                          USB CDC
                             |
                       [Android App]
```

## Why OVLink?

### The Problem

In Japan (and many other countries), traditional FPV video transmission requires:
- An amateur radio license (4th class or above in Japan)
- Frequency allocation registration
- Restriction to non-commercial use (amateur radio cannot be used for business purposes)

This means **schools, businesses, and hobbyists without licenses** cannot legally use conventional FPV systems.

### The Solution

OVLink uses **2.4GHz WiFi** with radio-certified modules (技適 in Japan), which means:
- ✅ No license required
- ✅ No registration needed
- ✅ Legal for commercial and educational use
- ✅ Anyone can use it immediately

### Comparison with Existing Solutions

| | OVLink (HD target) | hx-esp32-cam-fpv | Analog 5.8GHz FPV |
|---|---|---|---|
| Resolution | 720p | 800×456 | ~480i |
| Encoding | H.264 (hardware) | MJPEG | Analog |
| Frame rate | 60fps | 30-50fps | 60fps |
| License required | No | No* | Yes |
| Ground station | Smartphone | Raspberry Pi / Radxa | FPV Goggles |
| Latency target | <50ms | 90-110ms | ~20ms |

*hx-esp32-cam-fpv uses packet injection which may not comply with regulations in all regions

## Technical Details

### Why MJPEG for HVGA version?

The ESP32-S3 lacks a hardware H.264 encoder. Software H.264 encoding is not feasible in real-time on this platform. MJPEG is natively supported by the camera driver, providing low-latency encoding suitable for proof-of-concept.

### Why H.264 for HD version?

The ESP32-P4 includes a hardware H.264 encoder capable of 1080p@30fps. At 720p, the theoretical maximum is ~67fps, providing sufficient headroom for stable 60fps operation. H.264's inter-frame compression dramatically reduces bandwidth compared to MJPEG, enabling higher quality at the same data rate.

### Why USB CDC?

Connecting the receiver to a smartphone via USB CDC provides:
- Near-zero additional latency
- Stable bandwidth
- Phone's WiFi remains free for other use
- No need for dedicated ground station hardware (Raspberry Pi, etc.)

### Why separate TX chips?

The TX side uses two chips (ESP32-P4 for encoding + XIAO ESP32-S3 for WiFi) to:
- Isolate encoding and transmission concerns
- Maintain radio certification compliance (技適)
- Leverage ESP32-P4's encoding power while using ESP32-S3's certified WiFi

## Use Cases

- **FPV Drones** — License-free digital FPV for hobby and racing drones
- **Education** — STEM robotics, classroom drone experiences, robot competitions without license barriers
- **Industrial** — Warehouse inspection robots, agricultural monitoring, facility inspection
- **Hobby** — RC cars, boats, and any remote-controlled vehicle with FPV

## Roadmap

- [x] HVGA prototype (480×320 @ 30fps)
- [x] Custom Android receiver app
- [ ] ESP32-P4 HD version PCB design
- [ ] H.264 encoding pipeline implementation
- [ ] 720p @ 60fps validation
- [ ] Adaptive bitrate control
- [ ] FEC (Forward Error Correction) for packet loss resilience
- [ ] Range improvement with external antenna
- [ ] Crowdfunding campaign (Kibidango)
- [ ] Prototype distribution to reviewers

## Links

- 📝 [Qiita Article (Japanese)](https://qiita.com/ovlink000) — Technical write-up of the HVGA prototype
- 🐦 [X (Twitter)](https://x.com/KZvtxjlyz) — Development updates


## Hardware

### HVGA Version
- **TX:** Seeed XIAO ESP32-S3 + OV3660 camera
- **RX:** Seeed XIAO ESP32-S3
- **Display:** Android smartphone (custom app)

### HD Version (Planned)
- **Encoder:** Waveshare ESP32-P4 module
- **WiFi TX:** Seeed XIAO ESP32-S3 (技適 certified)
- **Camera:** OV5647 MIPI CSI-2 (120° fixed focus)
- **RX:** Seeed XIAO ESP32-S3
- **Display:** Android smartphone (custom app, MediaCodec H.264 decode)
- **PCB:** 4-layer, manufactured by JLCPCB
- **RX Antenna:** Seeed certified external rod antenna (2.81 dBi)

## License

TBD

## Author

**kz OGAWA** — Student developer based in Japan

Building OVLink to make real-time video transmission accessible to everyone, without license barriers.
