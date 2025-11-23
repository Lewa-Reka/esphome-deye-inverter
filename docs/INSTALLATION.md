# Installation Guide

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
3. **Replace the entire content** with the configuration from [`pv-inverter.yaml`](../pv-inverter.yaml)
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
