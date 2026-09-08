# Features & Capabilities

## 🚀 Features

- **Real-time Monitoring**: Monitor PV production, battery status, grid consumption, and load data
- **Remote Control**: Control inverter work modes, battery charging parameters, and time-of-use settings
- **Time of Use "All" Entities**: Convenient bulk control of all Time of Use windows simultaneously (disabled by default)
- **Home Assistant Integration**: Seamless integration with Home Assistant via ESPHome API
- **Safety Features**: Automatic fallback to safe operating parameters when connection to Home Assistant is lost
- **Modular Design**: Clean, maintainable code structure with separate packages for different system components
- **Multiple Inverter Support**: Currently supports both low voltage (SG04LP3) and high voltage (SG01HP3) Deye inverters

## 🏠 Home Assistant Integration

This project automatically integrates with Home Assistant through the ESPHome API. Once flashed and connected, you'll have access to:

### Sensors
- **Solar Production**: Panel voltage, current, and power for each string
- **Battery**: Voltage, current, state of charge, temperature, and charging status
- **Grid**: Voltage, current, frequency, power, and energy counters
- **Load**: Consumption data and UPS power information
- **Inverter**: Temperature, status, and operating parameters

### Controls
- **Work Modes**: Selling First, Zero Export to Load, Zero Export to CT
- **Battery Management**: Charging current limits, cut-off voltages
- **Time-of-Use**: Programmable charging/selling schedules
- **Power Limits**: Maximum selling and charging power settings
- **Generator**: Control and monitoring (if connected)
- **Time Synchronization** (3P only): Automatic and Manual time sync. Button available in Home Assistant
- **Grid Settings** (3P only, opt-in): Grid protection, reconnect/trip limits, grid-support curves (V-Watt, Volt-VAR, Freq-Watt, Watt-VAR, Watt-PF), and LVRT/HVRT as a separate Home Assistant device. Writes are locked until **Allow Changes (DANGEROUS)** is turned on.

### Time of Use "All" Entities
The system includes convenient "All" entities that allow you to control all Time of Use windows simultaneously:

#### For Deye Inverters:
- **Time of Use All SoC**: Set battery charge level for all 6 windows at once
- **Time of Use All Out Power**: Set output power limit for all 6 windows at once  
- **Time of Use All Voltage**: Set voltage threshold for all 6 windows at once
- **Time of Use All Grid Charge**: Enable/disable grid charging for all 6 windows at once
- **Time of Use All Gen (currently only 3P)**: Enable/disable generator charging for all 6 windows at once
- **Time of Use All Sell (currently only 3P and probably only working properly with HV)**: Enable/disable selling for all 6 windows at once

**Important Notes:**
- These entities are **disabled by default** and must be enabled in Home Assistant to use them
- When you change a value on any "All" entity, it automatically applies the same setting to all 6 Time of Use windows
- These entities are particularly useful for automations where you want to control all Time of Use windows simultaneously
- The "All" entities read their current state from the first Time of Use window (e.g., "Time of Use 1 SoC")

## 🔒 Safety Features

The system includes comprehensive safety mechanisms:

1. **Connection Monitoring**: Continuously monitors connection to Home Assistant
2. **Safe Mode Transition**: Automatically applies safe operating parameters when disconnected for more than 10 minutes
3. **Parameter Validation**: All settings are validated against safe operating ranges
4. **Fallback Access**: Emergency WiFi hotspot with configurable password
5. **Hardware Protection**: Modbus communication timeouts and error handling
6. **Grid Settings write lock** (3P opt-in): Protection and grid-support registers stay readable; Modbus writes are ignored until **Allow Changes (DANGEROUS)** is on. The switch always starts **off** after reboot or OTA. Safe mode does not change these registers.

### Grid Settings (3P, optional)

This package is **not** loaded by default. It exposes a second Home Assistant device named `{friendly_name} Grid Settings` (ESPHome 2025.7 or newer). Register map follows Deye **Modbus RTU V105.4** for three-phase hybrids (SG0XLP3 / SG0XHP3).

Included:

- Grid voltage/frequency protection (registers 185–188)
- Reconnect and trip 1/2 values (350–362) and trip delays (417–424)
- Grid-support curves: V-Watt, Volt-VAR, Freq-Watt, Watt-VAR, Watt-PF (363–412, excluding 391 and 394 which have no scale in the protocol)
- LVRT/HVRT enable, voltage points, and times (482–492). Times are in **milliseconds**, voltages as **% of rated voltage**

Incorrect community mappings that treat 381–399 as HV2/LV3 ride-through (or 184 as over-voltage) are **not** used. Those addresses are Volt-VAR / Freq-Watt / Grid Type in V105.4.

See [Configuration](CONFIGURATION.md) for how to enable the package.

## 📊 Monitoring Capabilities

### Real-time Data
- Solar panel performance (voltage, current, power per string)
- Battery status (SoC, voltage, current, temperature)
- Grid parameters (voltage, frequency, power flow)
- Load consumption (per phase and total)
- Inverter health (temperature, relay status)

### Historical Data
- Daily/total energy production and consumption
- Battery charging/discharging cycles
- Grid import/export statistics
- System efficiency calculations
