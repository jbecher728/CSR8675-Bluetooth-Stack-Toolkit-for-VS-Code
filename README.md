# CSR8675 I2S Host Stack (Unofficial)

This project turns the earlier starter toolkit into a **real host-side stack** for a CSR8675-based **I2S variant** module.

It is designed for the practical architecture that actually works without Qualcomm's proprietary ADK:

- **CSR8675 module** handles Bluetooth radio, HFP/A2DP firmware, and audio transport on the module side.
- **Host controller** handles keypad scanning, T9 texting UI, display rendering, LEDs, state transitions, and module control over UART / GPIO-style commands.
- **I2S audio path** is represented in configuration, diagnostics, and system integration notes.

## What this project is

- A VS Code-ready host stack for CSR8675 I2S modules that expose UART control or AT-like commands.
- A reusable Python state machine for calling, dialing, contacts/messages navigation, and T9 composition.
- A text-mode display renderer you can map to your real display firmware later.
- A transport layer for real serial links plus a realistic simulator for offline development.
- A profile-driven command mapping system for module-specific UART command sets.
- An I2S configuration model with validation and diagnostics.

## What this project is not

- Not a Qualcomm ADK replacement.
- Not guaranteed to work with every CSR8675 board.
- Not a firmware flasher or proprietary patcher.
- Not a bypass for closed Qualcomm application development.

## Main features

- Serial transport with line-oriented request/response support
- Module event parsing for common call/link status indications
- Host state machine for:
  - idle / dialing / incoming / active call / messages / contacts
  - caller ID display
  - volume control
  - pairing / link status display
- T9 text composition engine
- Keypad matrix model and key routing
- LED policy engine for call, link, and message states
- I2S config validation for common sample rates / formats / roles
- CLI commands for simulation and real-device control
- JSON module profiles for adapting to specific UART command sets
- Unit tests for parsing, T9, display, and state flow

## Quick start

### 1) Create a virtual environment and install

```bash
python -m venv .venv
source .venv/bin/activate
pip install -e .
```

### 2) Run the simulator

```bash
csr8675ctl simulate --script scripts/demo_keys.txt
```

### 3) Build and run the optional C++ demo renderer

```bash
cmake -S . -B build
cmake --build build
./build/csr8675_demo
```

### 4) Inspect serial ports

```bash
csr8675ctl ports
```

### 5) Connect to a real module profile

```bash
csr8675ctl --port COM5 --profile scripts/csr8675_i2s_generic.json info
```

## Suggested hardware split

- **CSR8675 I2S module**: Bluetooth HFP/A2DP endpoint
- **Host MCU or host SBC**: keypad, display, LED effects, UI logic
- **Audio codec / DAC / amp**: fed from the module's I2S output according to the actual module wiring

## Project layout

- `python/csr8675_i2s_toolkit/` core host stack
- `scripts/` JSON device profiles and sample key scripts
- `tests/` unit tests
- `.vscode/` tasks and launch configs
- `docs/` integration notes
- `src/`, `include/` small optional C++ display demo

## Good next steps

- Adapt `scripts/csr8675_i2s_generic.json` to your exact module's command set.
- Replace the simulated keypad with GPIO scanning on your chosen MCU.
- Swap the text display renderer for your SSD1351 or other display driver.
- Bind the LED policy outputs to your RGB LED driver.
