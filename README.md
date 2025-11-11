# ESPHome Deye Inverter

<!--
Copyright 2025 Lewa-Reka <lewareka.yt@gmail.com>

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

![Maintenance](https://img.shields.io/maintenance/yes/2025?style=for-the-badge)
![GitHub Actions Workflow Status](https://img.shields.io/github/actions/workflow/status/lewa-reka/esphome-deye-inverter/build-ci.yaml?style=for-the-badge)
![GitHub License](https://img.shields.io/github/license/lewa-reka/esphome-deye-inverter?style=for-the-badge)
![GitHub commit activity](https://img.shields.io/github/commit-activity/y/lewa-reka/esphome-deye-inverter?style=for-the-badge)

An ESPHome-based solution for monitoring and controlling Deye photovoltaic inverters via Modbus RTU communication. This project provides comprehensive integration with Home Assistant, enabling real-time monitoring and control of solar power systems.

Installation & Presentation: https://youtu.be/iJjsA_MzmnE [PL]

## ⚠️ Breaking Changes

### Version 0.4.0
- **Default Baud Rate Change for Deye SG0XLP3 and SG0XHP3 Inverters**: The default baud rate has been changed from **19200** to **9600** for better compatibility and stability.
- **Action Required**: If you're upgrading from a previous version and have a inverter configured with baud rate 19200, you **must** change it to 9600 before updating.

## 📋 Currently Supported Inverters

- **Deye SG0XLP1** *(Single Phase Low Voltage)*
   
  ***Single Phase Recommended baud rate: 9600 (default, nothing to change)***

- **Deye SG04LP3** *(Low Voltage)*
- **Deye SG05LP3** *(Low Voltage)*
- **Deye SG01HP3** *(High Voltage)*
- **Deye SG02HP3** *(High Voltage)*
   
  ***3 Phases Recommended baud rate: 9600 (default, nothing to change)***

#### Deye Inverter Configuration

To set the baud rate on your Deye inverter:
1. Navigate to: **Advanced Settings** → **Paral. Set 3** → **Boud Rate**
2. Set the value to **9600**
3. Save the settings


**Note**: After saving, the screen will turn off and a red LED alarm will flash - this is normal behavior. Everything should return to normal after a moment.

## 🚀 Features

- **Real-time Monitoring**: Monitor PV production, battery status, grid consumption, and load data
- **Remote Control**: Control inverter work modes, battery charging parameters, and time-of-use settings
- **Time of Use "All" Entities**: Convenient bulk control of all Time of Use windows simultaneously (disabled by default)
- **Home Assistant Integration**: Seamless integration with Home Assistant via ESPHome API
- **Safety Features**: Automatic fallback to safe operating parameters when connection to Home Assistant is lost
- **Modular Design**: Clean, maintainable code structure with separate packages for different system components
- **Multiple Inverter Support**: Currently supports both low voltage (SG04LP3) and high voltage (SG01HP3) Deye inverters

## 🛠️ Hardware Requirements

- **ESP Development Board** (e.g., ESP32-DevKit V1)
- **RS485 to TTL Converter**
- **Deye Inverter** with Modbus RTU support
- **Connecting Cables** (for RS485 communication)

## 🔌 Wiring

Connect your ESP32-DevKit V1 to the inverter's Modbus port:

```
ESP32          RS485 Converter    Inverter
GPIO17 (TX) -> DI/A+           -> A+ (Modbus)
GPIO16 (RX) -> RO/B-           -> B- (Modbus)
3.3V        -> VCC             
GND         -> GND             -> GND (Modbus)
```

**Note**: Some RS485 converters may require a flow control pin. Uncomment and configure the `flow_control_pin` in the main configuration if needed.

## ⚙️ Installation

### Method 1: Home Assistant ESPHome Add-on (Recommended)

This is the easiest way to get started if you're using Home Assistant.

#### Prerequisites
- [Home Assistant](https://www.home-assistant.io/) with Add-on store access
- ESP development board
- RS485 to TTL converter

#### Step 1: Install ESPHome Add-on
1. In Home Assistant, go to **Settings** → **Add-ons** → **Add-on Store**
2. Search for **"ESPHome"** and click **Install**
3. After installation, click **Start** and optionally enable **"Start on boot"**
4. Click **"Open Web UI"** to access ESPHome dashboard

#### Step 2: Add New Device
1. In ESPHome dashboard, click **"+ NEW DEVICE"**
2. Click **"CONTINUE"** on the welcome screen
3. Enter a name for your device (e.g., "Deye Inverter")
4. Select your **"ESP"** device as device type
5. Click **"NEXT"** and note down the encryption key (save it safely)
6. Click **"SKIP"** for now - we'll add the configuration manually

#### Step 3: Configure Device
1. **Create secrets file**:
   Create `secrets.yaml` in your ESPHome configuration directory:
   ```yaml
   wifi_ssid: "YourWiFiSSID"
   wifi_password: "YourWiFiPassword"
   pv_inverter_api_key: "your-32-character-api-key"
   pv_inverter_ota_password: "your-ota-password"
   pv_inverter_fallback_password: "your-fallback-password"
   ```
2. Click **"EDIT"** on your newly created device
3. **Replace the entire content** with the configuration from [`pv-inverter.yaml`](pv-inverter.yaml)
4. Click **"SAVE"** and then **"INSTALL"**

#### Step 4: Flash to ESP32
1. Connect your ESP32 to your computer via USB
2. Click **"Plug into this computer"**
3. Select the correct COM port
4. Click **"INSTALL"** and wait for the process to complete
5. Click **"STOP LOGS"** when installation is finished

#### Step 5: Connect Hardware
1. Disconnect ESP32 from computer
2. Connect RS485 converter according to wiring diagram above
3. Power up the ESP32 (via USB power adapter or external power)
4. The device should appear in Home Assistant automatically under **Settings** → **Devices & Services** → **ESPHome**

### Method 2: ESPHome CLI

If you prefer using command line or don't have Home Assistant:

1. **Install ESPHome CLI**:
   ```bash
   pip install esphome
   ```

2. **Download Configuration**:
   ```bash
   wget https://raw.githubusercontent.com/Lewa-Reka/esphome-deye-inverter/main/pv-inverter.yaml
   ```

3. **Create secrets.yaml**:
   ```yaml
   wifi_ssid: "YourWiFiSSID"
   wifi_password: "YourWiFiPassword"
   pv_inverter_api_key: "your-32-character-api-key"
   pv_inverter_ota_password: "your-ota-password"
   pv_inverter_fallback_password: "your-fallback-password"
   ```

4. **Flash to ESP**:
   ```bash
   esphome run pv-inverter.yaml
   ```

## 🔧 Configuration Options

### Main Configuration (`pv-inverter.yaml`)

| Parameter | Description | Default | Required |
|-----------|-------------|---------|----------|
| `name` | Device name in ESPHome | depends on inverter type | No |
| `friendly_name` | Friendly device name | depends on inverter type | No |
| `device_description` | Device description | depends on inverter type | No |
| `modbus_controller_id` | Modbus controller ID | depends on inverter type | No |
| `modbus_inverter_address` | Modbus address of your inverter | `0x01` | No |
| `baud_rate` | Baud rate for Modbus communication | `9600` | No |
| `update_interval` | How often sensor values are updated from inverter | 5s | No |

### Inverter Configuration

| Parameter | Description | Default | Required | Supported Inverters |
|-----------|-------------|---------|----------|---------------------|
| `safe_mode_delay` | Delay before activating safe mode when disconnected | `600s` | No | All (1P/3P) |
| `default_maximum_battery_charge_current` | Maximum battery charge current in safe mode | `140` | No | All (1P/3P) |
| `default_max_sell_power` | Maximum power export in safe mode | `12000` | No | All (1P/3P) |
| `default_system_work_mode` | System work mode in safe mode | `"Zero Export To Load"` | No | All (1P/3P) |
| `default_solar_sell` | Solar selling state in safe mode | `"on"` | No | All (1P/3P) |
| `default_force_off_grid` | Force Off Grid switch state in safe mode | `"off"` | No | 3P only (SG0XLP3, SG0XHP3) |
| `enable_sync_time` | Enable automatic time synchronization with Home Assistant | `false` | No | 3P only (SG0XLP3, SG0XHP3) |

### Default Hardware Configuration

- **UART Pins**: GPIO17 (TX), GPIO16 (RX)
- **Baud Rate**: 9600
- **Board**: ESP32-DevKit v1 (configurable)
- **Flow Control**: Optional GPIO4 (uncomment if needed)

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

### Safety Features
- **Automatic Safe Mode**: Transitions to safe parameters when Home Assistant connection is lost
- **Connection Monitoring**: Real-time status of communication links
- **Parameter Validation**: Ensures all settings remain within safe operating ranges
- **Emergency Access**: Fallback WiFi hotspot for emergency configuration

## 🔒 Safety Features

The system includes comprehensive safety mechanisms:

1. **Connection Monitoring**: Continuously monitors connection to Home Assistant
2. **Safe Mode Transition**: Automatically applies safe operating parameters when disconnected for more than 10 minutes
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

## 🐛 Troubleshooting

### Common Issues

1. **No Communication with Inverter**:
   - Check RS485 wiring (A+/B- connections)
   - Verify Modbus address matches inverter settings
   - Ensure correct baud rate (19200)
   - Check modbus_inverter_address in configuration

2. **WiFi Connection Issues**:
   - Verify SSID and password in configuration
   - Check WiFi signal strength at ESP32 location
   - Use fallback hotspot for emergency access
   - Check router firewall settings

3. **Home Assistant Integration Problems**:
   - Verify API key matches between configuration and Home Assistant
   - Check ESPHome add-on logs for connection errors
   - Ensure ESPHome add-on is running and accessible
   - Verify network connectivity between devices

4. **Sensor Reading Issues**:
   - Check ESPHome device logs for Modbus errors
   - Verify inverter is powered and responsive
   - Check for electromagnetic interference near RS485 cables
   - Ensure proper grounding of RS485 converter

### Debug Mode

Enable verbose logging by uncommenting in `pv_inverter.yaml`:
```yaml
logger:
  level: VERBOSE
```

Then check logs in ESPHome dashboard or Home Assistant ESPHome integration.

## 🔄 Updates

The configuration automatically updates packages from this repository every 12 hours. To force an update:
1. In ESPHome dashboard, click "CLEAN BUILD FILES" on your device
2. Click "INSTALL" to rebuild with latest packages

## 🤝 Contributing

Contributions are welcome! Areas where help is needed:

- **New Inverter Support**: Modbus register mappings
- **Feature Enhancements**: Additional monitoring capabilities
- **Documentation**: Translations, setup guides
- **Testing**: Validation with different hardware configurations

### Development Process
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-inverter-support`)
3. Make your changes with proper testing
4. Update documentation as needed
5. Submit a pull request with detailed description

## 📄 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

### Copyright Notice

```
Copyright 2025 Lewa-Reka <lewareka.yt@gmail.com>

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

### Third-Party Components

This project uses ESPHome and related components. Please refer to the [NOTICE](NOTICE) file for additional license information.

## 🙏 Acknowledgments

- [Solarman Stick Logger by David Rapan](https://github.com/davidrapan/ha-solarman) and [pilipphenkel](https://github.com/philipphenkel/esphome-config) for inspirations
- [ESPHome Team](https://esphome.io/) for the excellent home automation platform
- [Home Assistant Community](https://community.home-assistant.io/) for continuous support and inspiration
- All contributors and testers who helped improve this integration

## 📞 Support

- **GitHub Issues**: [Report bugs or request features](https://github.com/Lewa-Reka/esphome-deye-inverter/issues)
- **Discussions**: [GitHub Discussions](https://github.com/Lewa-Reka/esphome-deye-inverter/discussions) for questions and community support
- **Home Assistant Community**: [ESPHome section](https://community.home-assistant.io/c/esphome/) for general ESPHome help

## 🔗 Related Projects

- [HA-Solarman](https://github.com/davidrapan/ha-solarman) - Alternative WiFi stick integration
- [ESPHome Official Docs](https://esphome.io/) - ESPHome documentation

---

**⚠️ Disclaimer**: This project is not officially affiliated with Deye. Use at your own risk and ensure compliance with local electrical codes and regulations. Always consult with a qualified electrician for installation and safety verification. The authors are not responsible for any damage to equipment or injury resulting from the use of this software. 
