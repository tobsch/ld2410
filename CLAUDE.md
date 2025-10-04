# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains ESPHome configuration for ld2410 presence detection sensors using ESP32-C3 boards.

## Architecture

- **common.yaml**: Base ESPHome configuration template that uses substitutions for device-specific values
  - Configures ESP32-C3 board with ESP-IDF framework
  - Sets up UART communication with ld2410 sensor (configurable TX/RX pins and baud rate)
  - Provides binary sensor for target detection
  - Provides sensor for detection distance measurement
  - Includes Home Assistant API with encryption and OTA updates
  - WiFi fallback with captive portal for reconfiguration

## Key Configuration Points

- UART pins and baud rate are configurable via substitutions (`ld_uart_tx`, `ld_uart_rx`, `ld_baud`)
- Default baud rate: 256000
- WiFi credentials reference `!secret` values that must be defined in ESPHome secrets file
- **Each device requires unique `api_key`** substitution for Home Assistant encryption (generate with `esphome secrets generate-encryption-key`)
- OTA password configured via `ota_password` substitution (can be shared or per-device)
- Minimum ESPHome version required: 2025.5.0
- Device names use substitutions for easy customization per device

## Expected Usage Pattern

This configuration is designed to be inherited/included by device-specific YAML files that override the substitutions for each physical sensor deployment.
