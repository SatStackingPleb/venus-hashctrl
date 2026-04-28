
# VenusHashCtrl

Victron-native, DC-powered Bitcoin mining for solar systems.

VenusHashCtrl runs directly on Venus OS and turns ASIC miners into dynamic loads that absorb excess solar energy while maintaining battery float voltage. No inverters. No PSUs. No wasted power.

---

## Why it Exists

Solar systems produce variable energy. Bitcoin miners consume fixed energy.

This mismatch creates three failures:

- Excess solar is wasted when batteries are full
- Batteries are drained when mining ignores system conditions
- AC conversion introduces unnecessary losses

VenusHashCtrl resolves this by making mining responsive to energy availability:

- Converts mining into a flexible load
- Matches power draw to real-time solar conditions
- Maintains battery at float instead of cycling
- Eliminates AC conversion by operating directly on DC

Result: energy dictates compute, not the other way around.

---

## Value Proposition

Designed for RVers and off-gridders already running Victron systems.

- Uses existing Victron hardware (no system redesign)
- Converts unused solar into productive output
- Preserves battery health by maintaining float voltage
- Eliminates inverter and PSU losses
- Adapts automatically to clouds, loads, and temperature

### Outcome

- Idle solar → productive output
- Stable batteries → longer lifespan
- Existing system → increased utility

Not a mining setup.

A Victron-native energy layer that monetizes surplus power without compromising off-grid reliability.

---

## Core Concept

```plaintext
Solar → MPPT → Battery → Control → Miner
````

Mining only runs when excess solar exists.
Battery stability is always prioritized.

---

## Key Features

- Native integration with Victron Energy systems via dbus
- Direct DC-powered mining (no inverter or PSU losses)
- Relay-based full miner control (Phase 1)
- Orion DC-DC per-board control (Phase 2)
- Dynamic frequency tuning via PyASIC (LuxOS / Braiins OS+)
- Float voltage–driven control logic (not SOC-first)
- Safe mode with automatic recovery
- MQTT integration for Home Assistant
- Lightweight web status endpoint
- Bootable SD deployment (headless-first)

---
## Firmware Strategy

### Primary Target: LuxOS

VenusHashCtrl is designed to integrate with LuxOS first.

Reason:

- Stable API surface
- Fine-grained frequency and voltage control
- Reliable telemetry (temps, hashrate, board status)

### Secondary Support

- Braiins OS+ (planned via abstraction layer)

All miner control must pass through a firmware abstraction layer to allow multi-firmware support without changing core logic.

## Operating Modes

### Phase 1 — Direct Bus Mode

- 12V systems only
- Relay-controlled full miner
- Cost-effective deployment
- Frequency scaling for load matching

### Phase 2 — Orion Mode

- Victron Orion DC-DC powered hashboards
- No relay switching
- Fine-grained, fully Victron-native control

---

## System Requirements

- Victron Energy system (Venus OS required)
- 12V LiFePO₄ battery bank
- Raspberry Pi running Venus OS
- ASIC miner (Bitmain supported first)

---

## Design Constraints

- Battery protection is priority
- Float voltage is primary control signal
- Native Victron dbus integration
- No AC conversion
- No supercapacitors
- Stability over maximum hashrate

---

## Ecosystem

- Victron Energy (Venus OS, MPPT, batteries)
- PyASIC (miner control abstraction)
- Home Assistant (optional via MQTT)

---

## Inspiration

Inspired by 100AcresRanch trailblazing the direct-to-DC mining concepts and the Victron and off-grid open-source communities.

---

Project in active development.
