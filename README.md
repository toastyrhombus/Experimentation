# Experimentation
Public repository to store and sync experiments with data structures and other logic utilities

## ESPHome Configuration for M5Stack Tough

This repository includes a comprehensive ESPHome configuration for the M5Stack Tough device, configured as a **Zeversolar TLC5000 Solar Inverter Monitor** with real-time local display.

### Features

The configuration includes support for:

#### Hardware Features
- **Display**: 2.4" ILI9341 TFT LCD (320x240) with multi-page UI
- **Touchscreen**: FT6336U capacitive touch controller for page navigation
- **IMU**: MPU6886 (accelerometer and gyroscope)
- **Buttons**: Three hardware buttons for page switching
- **Speaker**: NS4168 audio output with RTTTL support
- **LED**: Built-in LED control
- **RS485/Modbus**: Communication with Zeversolar inverter

#### Solar Monitoring Features
- **Real-time Display**: Live solar production monitoring on the M5Stack display
- **PV String Monitoring**: Voltage, current, and power for two independent solar strings
- **Grid Output**: AC voltage, current, frequency, and power output
- **Energy Tracking**: Daily and lifetime energy production
- **Efficiency Calculation**: Real-time conversion efficiency with color-coded indicators
- **Temperature Monitoring**: Inverter temperature with warning colors
- **Status Indicators**: Connection status and system health
- **Home Assistant Integration**: All sensors available in Home Assistant

#### Display Pages
1. **Splash Screen**: Boot screen with system initialization
2. **Main Monitor**: Real-time solar production with graphs and statistics
3. **System Info**: Network status, inverter details, and uptime

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

- **RS485/Modbus** (Zeversolar):
  - TX: GPIO14
  - RX: GPIO13

- **Buttons**:
  - Power Button: GPIO39 (also switches pages)
  - Button 1: GPIO38 (previous page)
  - Button 2: GPIO37
  - Button 3: GPIO1 (next page)

- **LED**: GPIO19
- **Speaker**: GPIO25

### Hardware Requirements

For the Zeversolar monitoring functionality, you'll need:
- **M5Stack Tough** development board
- **RS485 to TTL converter module** (e.g., MAX485, MAX3485)
- Connection to Zeversolar TLC5000 inverter's RS485 port

#### RS485 Wiring

Connect the RS485 module as follows:
```
M5Stack Tough         RS485 Module
GPIO14 (TX)    --->   DI (Driver Input)
GPIO13 (RX)    <---   RO (Receiver Output)
5V             --->   VCC
GND            --->   GND

RS485 Module          Zeversolar Inverter
A              --->   A+
B              --->   B-
```

### Display Navigation

The M5Stack Tough display cycles through three pages:

1. **Splash Page** - Shown during boot
2. **Main Solar Monitor** - Real-time production data with:
   - Solar input (PV1 & PV2 voltage, current, power)
   - Grid output (voltage, frequency, power)
   - Efficiency calculation with color indicators
   - Daily energy production with progress bar
   - Temperature and status indicators
3. **System Info** - Network and device status

**Navigation:**
- Touch the screen anywhere to advance to the next page
- Press Button 1 (bottom left) to go to the previous page
- Press Button 3 (bottom right) to go to the next page
- Press the Power Button to cycle pages

### Monitored Sensors

The configuration provides the following sensors to Home Assistant:

**Solar Production:**
- PV1 Voltage, Current (calculated: PV1 Power)
- PV2 Voltage, Current (calculated: PV2 Power)
- Current Power Output
- Grid Voltage, Current, Frequency
- Today's Energy Production
- Total Lifetime Energy Production
- Inverter Temperature
- Inverter Status (Online/Offline)

**System Sensors:**
- WiFi Signal Strength
- Device Uptime
- IP Address and SSID

### Modbus Configuration

The Zeversolar TLC5000 uses Modbus RTU protocol over RS485:
- **Baud Rate**: 9600
- **Data Bits**: 8
- **Stop Bits**: 1
- **Parity**: None
- **Inverter Address**: 0x01 (configurable in substitutions)
- **Update Interval**: 30 seconds

Register mappings are pre-configured for the TLC5000 model. If you have a different Zeversolar model, you may need to adjust the register addresses.

### Troubleshooting

**Display shows "Offline" status:**
- Check RS485 wiring connections
- Verify the inverter is powered on and producing
- Check the Modbus address matches your inverter (default: 0x01)
- Ensure proper RS485 termination if needed

**No UART logging:**
- UART logging is disabled to allow Modbus communication
- Use WiFi-based logging instead (check Home Assistant logs or ESPHome logs)

**Sensor values show as 0 or NaN:**
- The inverter may be in standby mode (no solar production)
- Check Modbus communication in ESPHome logs
- Verify RS485 A/B wiring polarity

### Documentation

For more information about ESPHome, visit: https://esphome.io

For Zeversolar Modbus documentation, consult your inverter's technical manual.
