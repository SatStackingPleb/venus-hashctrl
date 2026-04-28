
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

Reasons:

- Stable, well-documented API surface
- Fine-grained frequency and voltage control per board
- Reliable telemetry (temps, hashrate, board status)
- Native **Ignore PSU** mode — critical for DC-direct operation

#### Why Ignore PSU Matters

Standard ASIC firmware expects to communicate with an APW-series PSU over a proprietary protocol. Without a PSU present, the miner faults and refuses to operate.

LuxOS's Ignore PSU feature disables this handshake requirement, allowing hashboards to run on any DC power source — including a direct battery bus. This is what makes VenusHashCtrl's core design possible at the firmware level. Without it, DC-direct operation would require physical PSU spoofing or hardware modification.

This feature effectively legitimizes the architecture. The miner doesn't need to be tricked — the firmware is simply told there is no PSU to monitor.

### Secondary Support

#### Braiins OS+

- Planned via firmware abstraction layer
- Power limit API well-suited to solar load matching
- Lacks per-board voltage control compared to LuxOS

#### Mujina

Mujina is an emerging open-source Bitcoin mining firmware from the 256 Foundation targeting multiple ASIC platforms including 256 Foundation's own Libreboard. It represents the longer-term direction for this project:

- Open codebase enables deeper integration than closed firmware APIs
- Community-driven development aligns with the off-grid and open-source ethos of this project
- REST API in active development (currently unstable)

Currently runs on Bitaxe Gamma and EmberOne00. S19-series installable images are a stated near-term target but not yet available.

Support for Mujina is tracked as a future integration target once S19-class support ships and its API stabilizes.

### Deployment Priority Rationale

LuxOS and Braiins OS+ are prioritized over Mujina for one reason: **mass deployability**.

LuxOS and Braiins OS+ are already running on a large installed base of S19-class machines. A user can install VenusHashCtrl and be operational without touching their miner's firmware — the control layer speaks to whatever is already there.

Mujina requires reflashing the miner, which is a meaningful barrier: voided warranties, brick risk, and a step most operators won't take to try new software. Mujina support is the right long-term direction, but prioritizing it would limit the project's reach to early adopters willing to take that risk.

The goal is a configuration that can be deployed at scale to existing control boards and machines, with no hardware modification and no firmware flashing required.

### Abstraction Layer

All miner control passes through a firmware abstraction layer. Core control logic never calls firmware APIs directly — it calls abstract operations (`set_frequency`, `stop`, `get_telemetry`) that each firmware adapter implements. This keeps LuxOS-specific behavior isolated and allows Braiins OS+ and Mujina support to be added without touching control logic.


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
