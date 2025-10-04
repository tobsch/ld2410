# LD2410 Presence Sensors - ESPHome Configuration

ESPHome configuration for LD2410 presence detection sensors deployed throughout the house.

## Overview

This repository contains a reusable ESPHome configuration for LD2410 radar presence sensors using ESP32-C3 boards. The common configuration can be used for multiple sensors by creating device-specific YAML files.

## Hardware

- **Board**: ESP32-C3-DevKitM-1
- **Sensor**: LD2410 (24GHz human presence detection radar)
- **Communication**: UART (256000 baud)

## Setup

### Prerequisites

1. ESPHome 2025.5.0 or newer
2. WiFi credentials configured in ESPHome secrets file

### Secrets Configuration

Add to your `secrets.yaml`:

```yaml
wifi_ssid: "your-wifi-ssid"
wifi_password: "your-wifi-password"
# fallback_ap_password: "custom-fallback-password"  # Optional, defaults to wifi_password
```

### Creating a New Sensor

Create a new YAML file for each sensor (e.g., `living-room.yaml`):

```yaml
substitutions:
  devicename: living-room
  friendly: Living Room
  board: esp32-c3-devkitm-1
  wifi_ssid: !secret wifi_ssid
  wifi_password: !secret wifi_password
  ld_uart_tx: GPIO21
  ld_uart_rx: GPIO20
  ld_baud: "256000"

packages:
  common: !include common.yaml
```

Or override only what's needed:

```yaml
substitutions:
  devicename: bedroom
  friendly: Bedroom

packages:
  common: !include common.yaml
```

### Deploy to Device

```bash
esphome run <device-name>.yaml
```

## Features

### WiFi Fallback
If the device cannot connect to WiFi, it will automatically create a fallback access point with captive portal. Connect to the AP (named "[Device Name] Fallback") to reconfigure WiFi credentials.

### Available Sensors

Each device provides:

- **Binary Sensor**: Target detection (presence/absence)
- **Sensor**: Detection distance in meters

## Pin Configuration

Default UART pins for ESP32-C3:
- TX: GPIO21
- RX: GPIO20

Override in device-specific YAML if using different pins.
