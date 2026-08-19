# Calibrated Appliance Monitor

A Home Assistant custom integration for appliance-specific cycle detection from
smart-plug power and cumulative energy data. The detection algorithms are
derived by ChatGPT from recorded appliance traces.

Currently calibrated for:

- Indesit D2IHL326UK dishwasher
- Hoover HBDOS695TAMCE-80 washer-dryer

## Installation

### HACS

Add this repository to HACS as a custom repository of type **Integration**:

    https://github.com/sargant/HomeAssistant-CalibratedApplianceMonitor

Install **Calibrated Appliance Monitor**, restart Home Assistant, then add it from
**Settings → Devices & services**.

### Manual

Copy:

    custom_components/calibrated_appliance_monitor

to:

    /config/custom_components/calibrated_appliance_monitor

Restart Home Assistant, then add **Calibrated Appliance Monitor** from
**Settings → Devices & services**.

Select the smart-plug device and appliance calibration. The integration discovers
power and cumulative-energy sensors from the selected smart-plug device. To
change the plug or appliance calibration, remove the config entry and add it
again.

## Entities

Every appliance monitor exposes:

- **Cycle phase**
- **Running**

Dishwasher phases: `Idle`, `Running`, `Finished`.

Washer-dryer phases: `Idle`, `Washing`, `Drying`, `Finished`.

**Cycle phase** also carries `last_started`, `last_finished`, and
`last_cycle_energy_kwh`. Washer-dryer entries additionally expose
`last_cycle_outcome`: `Washing`, `Drying`, or `Washing + drying`.

## Diagnostics

Both calibrations provide:

- **Cycle start candidate**
- **Cycle start energy candidate**
- **Cycle start energy**

The washer-dryer also provides:

- **Drying start candidate**
- **Drying start time**
- **Cycle finish candidate**

All diagnostic entities are hidden by default but remain enabled and recorded.
Visibility defaults only affect newly created registry entries.

## Repository scope

This repository contains the reusable custom integration only. Household-specific
notification automations, tariff entities, dashboards, entity IDs, secrets, and
other Home Assistant configuration intentionally live elsewhere.

Appliance-specific thresholds and state machines live in
`custom_components/calibrated_appliance_monitor/algorithms/`. Notifications and
pricing policy are deliberately outside the detector.
