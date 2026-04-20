# Development

The wireless adapter has circuitry specifically for development. The design for this circuitry follows the design of the [ESP32-C6-DevKitC-1](https://dl.espressif.com/dl/schematics/esp32-c6-devkitc-1-schematics_v1.4.pdf).

## Switches

The wireless adapter has a switch to initiate chip reset and a switch to select the boot method. For the switches, I chose the [XunPu TS-1088-AR02016](https://www.lcsc.com/datasheet/C720477.pdf) because

- they're compact,
- they're in the JLCPCB basic parts type so it's lower cost during PCB assembly.

I used an RC filter to debounce each switch. When the switch is open, the resistor pulls the output high. When the switch is closed, the switch pulls the output low. The time constant of the reset switch's RC filter is ~10x the time constant of the boot switch's RC filter so that on power up the boot switch has time to be pulled high before the reset switch is pulled high.

## USB Connector

The wireless adapter has a 16-pin USB 2.0 Type-C connector providing USB 2.0 connectivity to the ESP32-C6-WROOM-1-N8's USB data lines. The ESP32-C6 supports 12Mbps transfer rate and implements USB serial and JTAG controller. I chose a Type-C connector because the Type-C connector is compact and has become the dominant USB connector. Any 16-pin USB 2.0 Type-C connector should work. I selected the [G-Switch GT-USB-7010ASV](https://www.lcsc.com/datasheet/C2988369.pdf).

## Protection

The wireless adapter has 5V bidirectional ESD diodes on the USB VBUS, D+ and D- lines to provide [ESD protection](./connectors.md#signal-pin-voltage-surge-protection).

## Power Multiplexer

The wireless adapter [multiplexes the 12.7V from the IU connector and the 5V from the USB connector](./power.md#power-multiplexer) so that the wireless adapter remains powered when doing development without the wireless adapter being connected to the indoor unit.