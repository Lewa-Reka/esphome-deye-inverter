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
- **Grid Settings** (3P only, opt-in): Grid protection, reconnect/trip limits, grid-support curves (P(U), Q(U), P(f), Q(P), PF(P)), and LVRT/HVRT as a separate Home Assistant device. Writes are locked until **Allow Changes (AT YOUR OWN RISK)** is turned on.

### Time of Use "All" Entities
The system includes convenient "All" entities that allow you to control all Time of Use windows simultaneously:

#### For Deye Inverters:
- **Time of Use All SoC**: Set battery charge level for all 6 windows at once
- **Time of Use All Out Power**: Set output power limit for all 6 windows at once  
- **Time of Use All Voltage**: Set voltage threshold for all 6 windows at once
- **Time of Use All Grid Charge**: Enable/disable grid charging for all 6 windows at once
- **Time of Use All Gen Charge**: Enable/disable generator charging for all 6 windows at once
- **Time of Use All Sell (currently only 3P and probably only working properly with HV)**: Enable/disable selling for all 6 windows at once

**Important Notes:**
- These entities are **disabled by default** and must be enabled in Home Assistant to use them
- When you change a value on any "All" entity, it automatically applies the same setting to all 6 Time of Use windows
- These entities are particularly useful for automations where you want to control all Time of Use windows simultaneously
- The "All" entities read their current state from the first Time of Use window (e.g., "Time of Use 1 SoC")

### Undocumented Time of Use Modes (1P, disabled by default)
The 1P Modbus protocol (since V116) defines extra bits whose behaviour Deye does not document:
- **Time of Use N GM Mode / BU Mode / CH Mode**: bits 2, 3 and 4 of the per-slot registers 274-279
- **Time of Use Mode 3 (Spain)**: bit 8 of register 248, described as "work mode 3 (Spanish customer requirement)"

They were added in the same protocol revision, so the GM/BU/CH modes probably belong to that Spain-specific work mode. The inverter stores the bits (the switches read back as enabled), but on EU firmware there is no matching setting on the inverter screen and no observed effect. These entities are **disabled by default**; enable them only if you know what your firmware does with them, and turn them off again if unsure.

## 🔒 Safety Features

The system includes comprehensive safety mechanisms:

1. **Connection Monitoring**: Continuously monitors connection to Home Assistant
2. **Safe Mode Transition**: Automatically applies safe operating parameters when disconnected for more than 10 minutes. The target values are editable under the **Config** sub-device in Home Assistant (`{friendly_name} Config`)
3. **Parameter Validation**: All settings are validated against safe operating ranges
4. **Fallback Access**: Emergency WiFi hotspot with configurable password
5. **Hardware Protection**: Modbus communication timeouts and error handling

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
