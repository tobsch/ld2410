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
ota_password: "your-ota-password"
```

**Shared OTA password** works for all devices, or you can use device-specific passwords for extra security.

### Home Assistant Integration

Each device requires a **unique API encryption key** for Home Assistant. Generate one using:

```bash
esphome secrets generate-encryption-key
```

Add the unique key to each device YAML file:

```yaml
substitutions:
  devicename: living-room
  friendly: Living Room
  api_key: "U8Ua36gSoAv2nnzDeMWdU0eSuAqJ8SwpMW88qLRG6wo="  # Unique per device

packages:
  remote_package:
    url: https://github.com/tobsch/ld2410
    files: [common.yaml]
    refresh: 1d
```

**Best Practice:** Use a different `api_key` for each sensor for better security.

**Override additional settings** as needed:

```yaml
substitutions:
  devicename: bedroom
  friendly: Bedroom
  api_key: "xyz123anotherUniqueKey456..."  # Different key for this device
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
  api_key: "yourUniqueKeyHere..."

packages:
  common: !include common.yaml
```

### Deploy to Device

```bash
esphome run <device-name>.yaml
```

## Features

### Home Assistant Ready
Full Home Assistant integration with encrypted API communication and OTA updates.

### Bluetooth Proxy
ESP32-C3 acts as a Bluetooth proxy, extending Home Assistant's Bluetooth range throughout your house. Automatically relays BLE devices like temperature sensors, locks, and tracking tags.

### WiFi Fallback
If the device cannot connect to WiFi, it will automatically create a fallback access point with captive portal. Connect to the AP (named "[Device Name] Fallback") to reconfigure WiFi credentials.

### Available Sensors & Controls

**Presence Detection:**
- Target detection (presence/absence)
- Detection distance in meters

**Diagnostic Sensors:**
- WiFi signal strength
- Uptime
- Internal temperature
- IP address
- Connected SSID
- ESPHome version

**Controls:**
- Restart button
- Safe mode button (recovery mode for OTA failures)

## Pin Configuration

Default UART pins for ESP32-C3-DevKitM-1:
- **TX**: GPIO21
- **RX**: GPIO20

Override in device-specific YAML if using different pins:

```yaml
substitutions:
  devicename: custom-device
  friendly: Custom Device
  api_key: "your-key..."
  ld_uart_tx: GPIO4
  ld_uart_rx: GPIO5
```
