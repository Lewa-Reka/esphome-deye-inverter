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

### Grid Settings package (3P only, opt-in)

Optional package for grid protection and grid-support curves. Requires **ESPHome 2025.7.0** or newer (Home Assistant sub-devices). Not enabled in the default `pv-inverter.Deye-SG0XLP3.yaml` / `pv-inverter.Deye-SG0XHP3.yaml` files.

Add a second GitHub package next to the inverter package:

```yaml
packages:
  pv_inverter:
    url: https://github.com/Lewa-Reka/esphome-deye-inverter
    refresh: 12h
    files: pv_inverter/deye_hybrid_3p_lv.yaml  # or pv_inverter/deye_hybrid_3p_hv.yaml
  grid_settings:
    url: https://github.com/Lewa-Reka/esphome-deye-inverter
    refresh: 12h
    files: pv_inverter/packages/deye_hybrid_3p/grid_settings.yaml
```

In Home Assistant you get a device named `{friendly_name} Grid Settings`. Values are read continuously from the inverter. Changing them writes to the inverter **only** when **Allow Changes (AT YOUR OWN RISK)** is on. If that switch is off, Home Assistant may briefly show the value you entered, then reverts (~0.5 s) to the last reading from the inverter. After reboot or OTA the lock switch is off again.

Wrong values can disconnect the inverter from the grid or violate the local grid code. Do not copy register maps that use 184 or 381–399 as simple U/f protection — those addresses mean something else in V105.4.

Grid-support curve entities use inverter labels: **P(U)**, **Q(U)**, **P(f)**, **Q(P)**, **PF(P)**. P(f) Hz points use names such as `P(f) Start Freq Over`; drop and delay omit `Freq` (`P(f) Drop Under`, `P(f) Start Delay Over`). Frequency points are 0.01 Hz.

### Default Hardware Configuration

- **UART Pins**: GPIO17 (TX), GPIO16 (RX)
- **Baud Rate**: 9600
- **Board**: ESP32-DevKit v1 (configurable)
- **Flow Control**: Optional GPIO4 (uncomment if needed)

## 🔄 Updates

The configuration automatically updates packages from this repository every 12 hours. To force an update:
1. In ESPHome dashboard, click "CLEAN BUILD FILES" on your device
2. Click "INSTALL" to rebuild with latest packages
