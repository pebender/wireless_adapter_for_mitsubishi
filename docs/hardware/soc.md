# The System on a Chip

I chose a System on a Chip (SoC) from the Espressif ESP32 family because

- the ESP32 family contains SoCs that meet the requirements,
- I am familiar with the ESP32 family,
- [ESPHome](https://esphome.io) has good support for the ESP32 family, and
- the ESP32 family contains pre-certified SoC modules.

I chose the [ESP32-C6-WROOM-1-N8](https://documentation.espressif.com/esp32-c6-wroom-1_wroom-1u_datasheet_en.pdf) because

- it has two UARTs,
- it has integrated radios,
- it has an integrated antenna,
- it supports 2.4GHz Wi-Fi 6,
- it supports [Thread 1.4](https://www.espressif.com/en/news/ESP32-C6_Thread_1.4_Certificate),
- it supports Bluetooth LE 5.3, and
- it appears to meet the power budget according to the [ESP32-C6 Hardware Design Guidelines](https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32c6/esp-hardware-design-guidelines-en-master-esp32c6.pdf) (Section 1.3.2 recommends a 500mA power supply)

I would have chosen the [ESP32-C5-WROOM-1-N8](https://documentation.espressif.com/esp32-c5-wroom-1_wroom-1u_datasheet_en.pdf) because in addition what is supported by the ESP32-C6-WROOM-1-N8,

- it supports 5GHz Wi-Fi 6, and
- it supports Bluetooth LE 6.0.

Unfortunately, it doesn't appear to meet the power budget according to the [ESP32-C5 Hardware Design Guidelines](https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32c5/esp-hardware-design-guidelines-en-master-esp32c5.pdf) (Section 1.3.2 recommends a 600mA power supply).

Both the ESP32-C6-WROOM-1-N8 and the ESP32-C5-WROOM-1-N8 are overkill for a Wi-Fi only device. However, in addition to running ESPhome and connecting over Wi-Fi, I wanted to experiment with

- running ESPHome and connecting over Thread
- running Matter and connecting over Wi-Fi, and
- running Matter and connecting over Thread.

As a result, I needed the Thread and Bluetooth LE radios and the extra flash memory reduces the need to optimize the firmware.

I am particularly interested in Thread because I believe that a Thread radio at every indoor unit would create a solid backbone for a Thread network. Once I have a solid backbone, I can begin to move devices from Wi-Fi to Thread.
