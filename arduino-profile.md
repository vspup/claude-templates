# Arduino Profile (Layer 1)

**Extends:** [project-bootstrap.md](./project-bootstrap.md) (Layer 0)
**GitHub:** https://github.com/vspup/claude-templates/blob/main/arduino-profile.md

---

## Purpose

This profile extends the core bootstrap with Arduino-specific structure and conventions.
It does **not** repeat Layer 0 principles — it only adds what is Arduino-specific.

---

## Critical naming rule

The `.ino` file **must** match the parent folder name exactly.
Arduino IDE will refuse to open the project otherwise.

```
MyProject/
└── MyProject.ino   ← must match
```

---

## Toolchain

Two supported toolchains with different structures:

| | Arduino IDE | PlatformIO |
|---|---|---|
| Entry point | `<Name>.ino` in root | `<Name>.ino` or `src/main.cpp` |
| Local libs | manual | `lib/` |
| Dependencies | Library Manager | `platformio.ini` |
| Build config | implicit | `platformio.ini` |

Decide at bootstrap. Record in `docs/decisions.md`.

---

## Minimal project

```
<ProjectName>/
├── <ProjectName>.ino
├── src/
├── docs/
│   └── decisions.md
└── .claude/
    ├── settings.json
    └── commands/
        ├── review.md
        └── commit.md
```

Start here. Nothing else until there is a real reason.

---

## Thin sketch principle

> The `.ino` file is the entry point for the Arduino ecosystem — not the architecture.

```cpp
// <ProjectName>.ino
#include "src/app/App.h"

App app;

void setup() { app.begin(); }
void loop()  { app.run(); }
```

**`.ino` must only contain:**
- `setup()` and `loop()`
- module instantiation
- wiring of top-level components

**`.ino` must not contain:**
- business logic
- protocol parsing
- state machines
- hardware drivers

---

## Responsibility layers

Add a layer only when its responsibility becomes real.

| Layer | Path | When to add |
|---|---|---|
| App logic | `src/app/` | Almost always — orchestration, modes, state |
| Protocol | `src/protocol/` | Packet formats, parsing, validation |
| Transport | `src/transport/` | Serial / CAN / I2C / SPI exchange |
| Board | `src/board/` | Pin mapping, hardware init, board config |
| Sensors | `src/sensors/` | Specific peripherals and sensor modules |
| Local libs | `lib/` | PlatformIO only — own reusable libraries |
| Device assets | `data/` | LittleFS / SPIFFS / web assets — only when FS exists |

**Rule:** create a layer when its responsibility first appears — not before.

---

## Docs and hardware materials

Two separate folders for two different origins:

| Folder | What goes here |
|---|---|
| `docs/` | What you write: architecture, pin mapping, flashing notes, calibration |
| `reference/` | What comes from outside: datasheets, vendor docs, PCB from supplier |

Create `reference/` only when external materials actually exist.
Add `reference/` to `.claudeignore` — large binaries should not consume AI context.

---

## Hardware-oriented extension

For measurement, device, or protocol-heavy projects — extend `src/` further:

```
src/
├── app/
│   ├── modes/       ← operating modes
│   └── orchestration/
├── protocol/
│   ├── parser/      ← incoming packet parsing
│   └── builder/     ← outgoing packet construction
├── transport/
│   ├── uart/
│   ├── can/
│   └── i2c/
├── board/
│   ├── pins.h       ← pin definitions
│   └── init.cpp     ← hardware initialization
└── sensors/
    └── <sensor_name>/
```

Add this depth only when the project genuinely has multiple protocols or transports.

---

## Arduino Assumptions extension

When bootstrapping an Arduino project, extend the standard Assumptions block with:

```
Assumptions:
- Project name: <name>          ← also the .ino filename
- Type: Single / Multi-Component
- Toolchain: Arduino IDE / PlatformIO
- Board: <board name>
- Stack profile (Layer 1): arduino-profile.md
- Framework / key libraries: <list or "none">
- Transport: <Serial / CAN / I2C / SPI or "none">
- Protocol: <custom / JSON / binary / "none">
- Run: <flash command or "Arduino IDE / PlatformIO upload">
- Tests: <command or "not configured yet">
- Layers present: <e.g. app, transport, board — discussed ones only>
- data/ needed: yes / no
- reference/ needed: yes / no
- Defaults applied: <list or "none">

Proceed with bootstrap?
```

---

## Arduino-specific anti-patterns

- Business logic in `.ino` — sketch becomes the architecture
- `.ino` filename not matching folder name — breaks Arduino IDE
- `src/` skipped — all code in `.ino` or root `.h` files
- `lib/` in Arduino IDE project — only valid in PlatformIO
- `data/` created without LittleFS/SPIFFS in use
- `reference/` added to `.gitignore` instead of `.claudeignore`
- Mixing protocol parsing with transport layer
- One `.h` file per sensor dumped in `src/` root without grouping
