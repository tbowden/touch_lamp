# Ethernet Irrigation Controller Project

## Project Overview

Design a **wired Ethernet irrigation controller** based on a Raspberry Pi Pico/Pico 2 that controls:

- A 3-phase irrigation pump (via an existing oversized contactor)
- Multiple 24 V irrigation solenoids
- Future sensors (float switches, pressure switches, etc.)
- Home Assistant integration over Ethernet
- Completely local operation (no cloud, no Wi-Fi)

The controller will live in an enclosure adjacent to the pump.

---

# Current Architecture

                 Home Assistant
                       │
                  Ethernet LAN
                       │
               10/100 Ethernet
                       │
              Pico 2 + W5500
                       │
                GPIO outputs
                       │
        ┌──────────────┴──────────────┐
        │                             │
   Pump contactor coil         Irrigation valves

Power is expected to come from **240 VAC**, not PoE.

---

# Why PoE Was Rejected

PoE was initially attractive because it provides both power and networking over one cable.

However:

- Site already has mains available.
- Solenoids and contactor require an external power source anyway.
- A 24 V control system is more expandable.
- Eliminates PoE power-budget constraints.
- Better matches industrial control practice.

Ethernet remains for communications only.

---

# Proposed Power System

## Preferred

- 240 VAC feed into enclosure
- DIN rail 24 V DC power supply
- 24 V → 5 V buck converter
- Pico powered from 5 V
- Field devices powered directly from 24 V

240 VAC
    │
24 V DIN PSU
    ├──────── 24 V outputs
    └──► Buck (24→5 V)
             │
           Pico 2

---

# Hardware Candidates

## Microcontroller

- Raspberry Pi Pico 2 (already owned)
- Original Pico also suitable

## Ethernet

Preferred chipset:

- WIZnet W5500 (SPI Ethernet)

Options:

- Pico W5500 Ethernet HAT
- Generic W5500 SPI module
- Design own PCB later using W5500

Avoid Wi-Fi entirely.

## Power Supply

DIN rail 24 V PSU:

- 30–60 W range
- Mean Well HDR series is the reference design
- Plenty of capacity for future expansion

## 5 V Supply

- MP1584 buck converter (preferred)
- LM2596 acceptable

## Outputs

If loads are **24 V DC**:

- Logic-level N-channel MOSFET
- Flyback diode
- One MOSFET per output

If loads are **24 VAC**:

- Opto-isolated triac outputs
- AC solid-state switching
- Different PCB entirely

---

# Planned I/O

## Outputs

| Device | Qty | Notes |
|----------|----|------|
| Pump contactor | 1 | Existing contactor |
| Irrigation valves | 4–8 | Expandable |

## Inputs (future)

- Float switches
- Pressure switch
- Tank level
- Flow sensor
- Manual override switches

Use opto-isolated 24 V inputs.

---

# Networking

- Wired 10/100 Ethernet
- MQTT preferred for Home Assistant
- Alternative: small HTTP REST API
- Static IP or DHCP reservation
- No cloud dependencies

---

# Enclosure Concept

DIN rail enclosure containing:

- 24 V PSU
- 24→5 V converter
- Pico + Ethernet board
- Output driver PCB
- Input isolation PCB
- RJ45 connector
- Terminal blocks for field wiring

Aim for serviceable screw terminals rather than flying leads.

---

# Open Questions

## High Priority

### 1. Contactor coil voltage

Determine whether the existing contactor coil is:

- [ ] 24 VDC
- [ ] 24 VAC
- [ ] Other

Photograph or read the coil label.

### 2. Solenoid voltage

Determine whether irrigation valves are:

- [ ] 24 VDC
- [ ] 24 VAC

Measure across an energized valve with a multimeter.

---

## Medium Priority

### Ethernet hardware

Choose between:

- [ ] Pico Ethernet HAT
- [ ] Generic W5500 module
- [ ] Custom PCB later

### Power sizing

Determine:

- Number of valves
- Maximum simultaneously energized valves
- Contactor coil current

This sets the required PSU wattage.

---

# Design Principles

- Local-first
- Ethernet only
- No Wi-Fi
- No cloud
- Modular PCBs
- DIN rail mounting
- 24 V control system
- Home Assistant integration via MQTT

---

# Shopping List (Current)

## Core

- [ ] W5500 Ethernet board/HAT
- [ ] 24 V DIN rail PSU (30–60 W)
- [ ] MP1584 24→5 V buck module
- [ ] DIN rail enclosure
- [ ] RJ45 panel connector
- [ ] Terminal blocks

## If 24 VDC outputs

- [ ] Logic-level MOSFETs
- [ ] Flyback diodes
- [ ] Gate resistors
- [ ] TVS diode for 24 V rail

## If 24 VAC outputs

- [ ] Opto-triac drivers
- [ ] Triacs / AC SSRs
- [ ] Zero-cross components (if required)

---

# Immediate Next Step

Inspect the existing installation and record:

1. Contactor coil voltage
2. Solenoid voltage
3. Number of valves
4. Whether multiple valves operate simultaneously

Those answers determine the output stage and finalize the hardware design.


