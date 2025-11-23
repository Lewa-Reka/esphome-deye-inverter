# Troubleshooting

## 🐛 Common Issues

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

## Debug Mode

Enable verbose logging by uncommenting in `pv_inverter.yaml`:
```yaml
logger:
  level: VERBOSE
```

Then check logs in ESPHome dashboard or Home Assistant ESPHome integration.
