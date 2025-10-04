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

packages:
  remote_package:
    url: https://github.com/tobsch/ld2410
    files: [common.yaml]
    refresh: 1d
```

Override additional settings as needed:

```yaml
substitutions:
  devicename: bedroom
  friendly: Bedroom
  ld_uart_tx: GPIO4  # Custom pins
  ld_uart_rx: GPIO5

packages:
  remote_package:
    url: https://github.com/tobsch/ld2410
    files: [common.yaml]
    refresh: 1d
```

**Alternative: Local configuration**

If you've cloned this repository, you can use local includes instead:

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
