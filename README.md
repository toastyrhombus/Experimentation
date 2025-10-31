# Experimentation
Public repository to store and sync experiments with data structures and other logic utilities

## ESPHome Configuration for M5Stack Tough

This repository includes a comprehensive ESPHome configuration for the M5Stack Tough device.

### Features

The configuration includes support for:

- **Display**: 2.4" ILI9341 TFT LCD (320x240)
- **Touchscreen**: FT6336U capacitive touch controller
- **IMU**: MPU6886 (accelerometer and gyroscope)
- **Buttons**: Three hardware buttons
- **Speaker**: NS4168 audio output with RTTTL support
- **LED**: Built-in LED control
- **WiFi**: Connection with fallback AP
- **OTA Updates**: Over-the-air firmware updates
- **Home Assistant API**: Integration with Home Assistant

### Setup Instructions

1. **Install ESPHome**:
   ```bash
   pip install esphome
   ```

2. **Configure Secrets**:
   ```bash
   cp secrets.yaml.example secrets.yaml
   # Edit secrets.yaml with your credentials
   ```

3. **Compile and Upload**:
   ```bash
   esphome run m5stack-tough.yaml
   ```

### Configuration Files

- `m5stack-tough.yaml` - Main ESPHome configuration
- `secrets.yaml.example` - Template for credentials (copy to secrets.yaml)

### Pin Mappings

- **Display**:
  - CS: GPIO5
  - DC: GPIO15
  - Reset: GPIO4
  - Backlight: Built-in

- **I2C** (Sensors):
  - SDA: GPIO21
  - SCL: GPIO22

- **Buttons**:
  - Power Button: GPIO39
  - Button 1: GPIO38
  - Button 2: GPIO37
  - Button 3: GPIO1

- **LED**: GPIO19
- **Speaker**: GPIO25

### Documentation

For more information about ESPHome, visit: https://esphome.io
