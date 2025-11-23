# Configuration

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

## 🔄 Updates

The configuration automatically updates packages from this repository every 12 hours. To force an update:
1. In ESPHome dashboard, click "CLEAN BUILD FILES" on your device
2. Click "INSTALL" to rebuild with latest packages
