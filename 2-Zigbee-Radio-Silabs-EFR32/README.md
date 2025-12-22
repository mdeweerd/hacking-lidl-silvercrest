# Zigbee Radio — Silabs EFR32MG1B

This section covers the **Zigbee coprocessor** embedded in the gateway: a Silabs EFR32MG1B chip running dedicated wireless firmware.

## Overview

The EFR32MG1B handles all Zigbee radio communication. The stock firmware uses the Tuya protocol, but it can be replaced with open-source alternatives to work with **Zigbee2MQTT**, **ZHA**, or **Matter/Thread**.

## Contents

| Directory | Description |
|-----------|-------------|
| [20-EZSP-Reference](./20-EZSP-Reference/) | Introduction to EZSP protocol and EmberZNet stack |
| [21-Backup-Flash-Restore](./21-Backup-Flash-Restore/) | Backup, flash, and restore the Zigbee chip firmware |
| [22-Build-Zigbee-Firmware](./22-Build-Zigbee-Firmware/) | Build firmware with slc-cli or Simplicity Studio |
| [23-Bootloader-UART-Xmodem](./23-Bootloader-UART-Xmodem/) | UART XMODEM bootloader for firmware updates |
| [24-NCP-UART-HW](./24-NCP-UART-HW/) | NCP firmware for Zigbee2MQTT and ZHA |

## Firmware: NCP (Network Co-Processor)

- The Zigbee stack runs on the EFR32
- Simple setup: just flash and connect to Zigbee2MQTT or ZHA
- Recommended for most users who want a Zigbee coordinator
