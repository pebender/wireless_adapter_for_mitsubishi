# Firmware Development, Debug and Recovery

The wireless adapter has circuitry specifically for firmware development, debug and recovery. The design for the circuitry follows the design of the [ESP32-C6-DevKitC-1](https://dl.espressif.com/dl/schematics/esp32-c6-devkitc-1-schematics_v1.4.pdf). The exceptions are

- the USB to UART is not supported, and
- [power multiplexing](./power.md#power-multiplexing) of the indoor unit's power with the USB power is supported.

## Switches

The wireless adapter has a switch to initiate boot and a switch to initiate reset. For the switches, I chose the [XunPu TS-1088-AR02016](https://www.lcsc.com/datasheet/C720477.pdf) because

- they're compact,
- they're in the JLCPCB basic parts type so it's lower cost during PCB assembly.

## USB Connector

The wireless adapter has a 16-pin USB 2.0 Type-C connector providing USB 2.0 connectivity to the ESP32-C6-WROOM-1-N8's USB data lines. The ESP32-C6 supports 12Mbps transfer rate and implements USB serial and JTAG controller. I chose a Type-C connector rather than a Type-A connector because the Type-C is more compact and more forward looking. Any 16-pin USB 2.0 Type-C connector should work. I selected the [G-Switch GT-USB-7010ASV](https://www.lcsc.com/datasheet/C2988369.pdf).

## Protection

The wireless adapter has 5V bidirectional ESD diodes on the USB VBUS, D+ and D- lines to provide [ESD protection](./connection.md#signal-pin-voltage-surge-protection).

## Power Multiplexing

The wireless adapter [multiplexes the 12.7V from the IU connector and the 5V from the USB connector](./power.md#power-multiplexing) so that the wireless adapter remains powered when doing firmware development without the indoor unit being connected to the wireless adapter.
