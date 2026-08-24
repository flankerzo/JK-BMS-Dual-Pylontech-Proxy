# JK BMS Dual Pylontech Proxy — V1.0

An ESPHome configuration for using one **JK-BMS JK-PB2A16S20P** as two
independent, virtual Pylontech-compatible batteries.

It is built for an **ESP32-S3 N16R8** and two **PowMr POW-HVM6.2K-48V-LIP**
inverters. The JK BMS is read through its RS485 Modbus interface, while the
ESP32-S3 emulates a Pylontech battery over a separate RS485 bus for each
inverter.

## Hardware

- ESP32-S3 N16R8
- JK-BMS JK-PB2A16S20P — hardware V19A, software V19.21, RS485 Modbus V1.0,
  address 1
- Three UART-to-RS485 converter modules: one for JK Modbus and one for each
  PowMr inverter
- Two PowMr POW-HVM6.2K-48V-LIP inverters

The two inverters are connected only through the common battery and N
(neutral) terminal. They operate independently; this setup is used for two
house phases rather than a three-phase installation.

## What it does

- Reads JK BMS telemetry and publishes it to Home Assistant through ESPHome.
- Emulates one Pylontech battery per inverter on fully independent RS485 UARTs.
- Shares physical BMS telemetry while allowing separate charge/discharge
  current budgets and voltage policy for each inverter.
- Sends Pylontech-compatible battery values, current limits, alarms and
  protection states.
- Fails silent when JK data becomes stale rather than serving old values.
- Never requests Pylontech force/grid charging; disable any grid-charge
  schedule directly in each inverter as well.

## Installation

1. Copy `jkbms-s3-dual-proxy.yaml` into Home Assistant's `/config/esphome/`.
2. Configure Wi-Fi and add the API encryption key to ESPHome `secrets.yaml`.
3. Verify all UART GPIO assignments, RS485 A/B polarity, battery voltage
   limits and current-share limits before flashing.
4. Compile and upload through the ESPHome Dashboard.

The configuration references current upstream components. Their source code is
also preserved in `external_components_backup/` for reproducibility.

## Safety and disclaimer

This is a personal community configuration, supplied **as is**, with no
warranty. High-current DC battery systems and inverter configuration can cause
equipment damage, fire, injury or death. You are solely responsible for
checking wiring, fuse and breaker sizing, grounding, battery chemistry limits,
BMS settings, inverter limits, and local electrical requirements before use.

This project is not affiliated with or endorsed by JK-BMS, PowMr, Pylontech,
ESPHome, Syssi, or Fahmula. It does not replace the JK BMS hardware protections
or a qualified electrical installation. Use and modify it at your own risk.

## Credits and references

This repository assembles established open-source work; it is not a new BMS or
Pylontech protocol implementation.

- [ESPHome](https://esphome.io/)
- [Syssi ESPHome JK-BMS component](https://github.com/syssi/esphome-jk-bms)
- [Fahmula ESPHome Pylontech RS485 emulator](https://github.com/Fahmula/esphome-pylontech-rs485)

See the upstream repositories for their respective licenses and attribution.
