# Wireless Adapter Firmware

## ESPHome Firmware

The ESPHome firmware is built using [ESPHome](https://esphome.io) and [mUART Project](https://muart-group.github.io)'s [mitsubishi_itp](https://github.com/muart-group/esphome-components/tree/dev/components/mitsubishi_itp)  ESPHome component.

Below is a basic ESPhome configuration file for running ESPHome and mitsubishi_itp on the wireless adapter. The only wireless adapter specific settings are the esp32 settings and the mapping between UART RX and TX pins and GPIOs in the uart settings.

```
substitutions:
  name: "heat-pump-family-room"
  friendly_name: "Family Room Heat Pump"
  ota_password: !secret heat_pump_family_room_ota_password
  api_key: !secret heat_pump_family_room_api_key
  wifi_ssid: !secret wifi_ssid
  wifi_password: !secret wifi_password
  
esphome:
  name: ${name}
  friendly_name: ${friendly_name}
  min_version: 2026.3.3

esp32:
  variant: esp32c6
  flash_size: 8MB
  framework:
    type: esp-idf

external_components:
  - source: github://muart-group/esphome-components@dev
    components: [ mitsubishi_itp ]

wifi:
  ssid: ${wifi_ssid}
  password: ${wifi_password}

ota:
  - platform: esphome
    password: ${ota_password}

api:
  encryption:
    key: ${api_key}

uart:
  - id: iu_uart
    baud_rate: 2400
    parity: EVEN
    rx_pin: GPIO21
    tx_pin: GPIO20
  - id: wr_uart
    baud_rate: 2400
    parity: EVEN
    rx_pin: GPIO18
    tx_pin: GPIO19

climate:
  - platform: mitsubishi_itp
    name: "Thermostat"
    uart_heatpump: iu_uart
    uart_thermostat: wr_uart
    icon: mdi:heat-pump
    visual:
      temperature_step:
        target_temperature: 1.0
        current_temperature: 1.0
    update_interval: 10s


# Default logging level
logger:
  baud_rate: 0
```

## Matter Firmware

The Matter firmware will be built using [esp-idf](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/index.html) and [esp-matter](https://docs.espressif.com/projects/esp-matter/en/latest/esp32/index.html).
