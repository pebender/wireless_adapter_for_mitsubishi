# Wireless Adapter Requirements

## Goal

The goal of this project is to create an Open Source wireless adapter for connecting the indoor units of our Mitsubishi Electric heat pump system to our [Home Assistant](https://www.home-assistant.io) centered home automation system without depending on the cloud.

## Existing System

Our Mitsubishi Electric heat pump system consists of eight indoor units

- MSZ-GL06NA-U1 (study),
- MSZ-GL09NA-U1 (office),
- MSZ-GL09NA-U1 (guest bedroom),
- MSZ-GL09NA-U1 (back bedroom),
- MSZ-GL15NA-U1 (family room),
- MSZ-GL18NA-U1 (kitchen),
- MSZ-GL18NA-U1 (living room),
- MSZ-GL18NA-U1 (master bedroom),

connected to three outdoor units

- MUZ-GL15NA-U1 (family room),
- MXZ-4C36NA2-U1 (kitchen, study, master bedroom), and
- MXZ-5C42NA2-U1 (living room, office, guest bedroom, back bedroom).

Each indoor unit is connected to a [Mitsubishi MHK1 controller kit](https://www.mitsubishipro.com/products/MHK1), consisting of a MIFH1 wireless receiver and a MRCH1 remote controller.

In addition to being paired with its MRCH1 remote controller, the MIFH1 wireless receiver connected to each indoor unit is paired with one of two [Honeywell THM6000R7001 RedLINK&reg; Internet Gateways](https://www.resideo.com/us/en/pro/products/air/thermostats/thermostat-accessories/redlinkr-internet-gateway-with-ethernet-cable-and-power-cord-thm6000r7001-u/). The THM6000R7001 internet gateways enable control of the indoor units from our phones and our home automation system. Unfortunately, they have two shortcomings. First, they have a slow response time because the RedLINK&reg; wireless protocol between the THM6000R7001 internet gateway and the MIFH1 wireless receiver is slow. Second, they can be unreliable because all their traffic is sent through the [Honeywell Home Total Connect Comfort](https://mytotalconnectcomfort.com) cloud. It's the THM6000R7001 internet gateways that this project will replace.

## Top Level Requirements

### Freedom Requirements

1. The wireless adapter hardware design must be Open Source.
    - The hardware components (e.g. the SoC) need not be Open Source.
2. The wireless adapter enclosure design must be Open Source
3. The wireless adapter firmware must be Open Source.
    - This prevents the owner of the wireless adapter from being subject to the whims of the firmware provider.
4. The wireless adapter must not require a cloud.
    - This prevents the owner of the wireless adapter from being subject to the whims of the cloud provider. For example, I was burned by a Genie Aladdin Connect when they changed their cloud API effectively blocking Home Assistant from using the cloud API.
5. The wireless adapter development tools must be Open Source.
    - This makes it easier to modify the wireless adapter design because 

### Connected Hardware Requirements

1. The wireless adapter must support the indoor units
    - the Mitsubishi Electric MSZ-GL06NA-U1,
    - the Mitsubishi Electric MSZ-GL09NA-U1,
    - the Mitsubishi Electric MSZ-GL15NA-U1 and
    - the Mitsubishi Electric MSZ-GL18NA-U1.
    These are my indoor unit models.
2. The wireless adapter must support the MIFH1 wireless receiver.
    - Each of my indoor units has an MIFH1 wireless receiver attached.
3. The wireless adapter must support the MIFH2 wireless receiver (aka MIFH2 receiver).
    - I can't test this as I don't have an MHK2 wireless remote controller kit.
4. The wireless adapter must allow normal operation of the Mitsubishi Electric MHK1 remote controller kit.
5. The wireless adapter must allow normal operation of the Mitsubishi Electric MHK2 remote controller kit.

### Radios

1. The wireless adapter must support a wireless connection using 2.4 [Wi-Fi&reg;](https://www.wi-fi.org).
    - ESPHome supports Wi-Fi.
    - Matter supports Wi-Fi.
2. The wireless adapter must support a wireless connection using [Thread&reg;](https://threadgroup.org) 1.4.
    - ESPHome supports Thread.
    - Matter supports Thread.
    - A Thread radio at each indoor unit should form a good backbone for a Thread network.
3. The wireless adapter must support [Bluetooth&reg;](https://www.bluetooth.com/) LE.
    - The Matter provisioning protocol relies on Bluetooth LE.

### Firmware

1. The wireless adapter hardware must support [ESPHome](https://esphome.io) firmware.
2. The wireless adapter hardware must support ESPHome over Wi-Fi.
3. The wireless adapter hardware must support ESPHome over Thread 1.4.
4. The wireless adapter hardware running ESPHome firmware must support the [Home Assistant](https://www.home-assistant.io) home automation system.
5. The wireless adapter hardware may support [Matter](https://csa-iot.org/all-solutions/matter/) firmware.

### Firmware Development

1. The wireless adapter must provide a USB-C connector.
    - The move has been toward the USB-C connector and away from other types of USB connectors.
2. The wireless adapter must support wireless adapter firmware download, debug and recovery over USB.
3. The wireless adapter must support being powered over USB.
4. The wireless adapter should provide transient voltage suppression (TVS) or electrostatic discharge (ESD) protection on the USB's VBUS, D+ and D- pins.

### IU Connector

1. The wireless adapter must provide a connector that is physically and electrically the same as the indoor unit's CN105 connector except that the UART RX and UART TX pins are reversed.
    - See [the IU connector](#the-iu-connector).
2. The wireless adapter must provide transient voltage suppression (TVS) for the connector's 12.7V and 5V pins.
3. The wireless adapter must provide electrostatic discharge (ESD) protection for the connector's UART RX and UART TX pins.
4. The wireless adapter must provide current limiting resistors for the connector's UART RX and UART TX pins.
5. The wireless adapter must control the indoor unit through a wired connection between connector to the indoor unit's CN105 connector.

### WR Connector

1. The wireless adapter must provide a connector that is physically and electrically the same as the indoor unit's CN105 connector.
    - See [the WR connector](#the-wr-connector).
2. The wireless adapter must provide transient voltage suppression (TVS) for the connector's 12.7V and 5V pins.
3. The wireless adapter must provide electrostatic discharge (ESD) protection for the connector's UART RX and UART TX pins.
4. The wireless adapter must provide current limiting resistors for the connectors UART RX and UART TX pins.

### Power

1. The wireless adapter must require no other power source than the indoor unit's CN105 connector.
2. The wireless adapter must be powered by the 12.7V pin of the indoor unit's CN105 connector.
    - See [Maximum Allowed Power Consumption](#maximum-allowed-power-consumption).
3. The wireless adapter must draw no more than 2W from the 12.7V pin of the indoor unit's CN105 connector.
    - See [Maximum Allowed Power Consumption](#maximum-allowed-power-consumption).
4. The wireless adapter may power the 5V side of the level shifter using the 5V pin of the indoor unit's CN105 connector.

## Maximum Allowed Power Consumption

I found no documentation covering the power supplied by the indoor unit's CN105 connector.

However, the wireless adapter is functionally the same as the [Mitsubishi Electric's PAC-USWHS002-WF-2 Wireless Interface 2](https://www.mitsubishipro.com/products/PAC-USWHS002-WF-2) except that the PAC-USWHS002-WF-2 requires a cloud and is not Open Source. Therefore, using the PAC-USWHS002-WF-2, I have been able to determine a power the wireless adapter can consume while still allowing my indoor unit to safely power the wireless adapter and the MIFH1/MIFH2 wireless receiver.

The [PAC-USWHS002-WF-2 Installation/Instruction Manual from 2021](https://s3.amazonaws.com/enter.mehvac.com/DAMRoot/Original/10006/PAC-USWHS002-WF-2_Install_Manual.pdf) states that the PAC-USWHS002-WF-2 consumes a maximum of 2W at 12.7V. So, a device that consumes no more than 2W from the CN105's 12.7V pin could be powered by any indoor unit compatible with the PAC-USWHS002-WF-2.

The [PAC-USWHS002-WF-2 Compatibility Submittal from 2021](https://s3.amazonaws.com/enter.mehvac.com/DAMRoot/Original/10009/M_Submittal_PAC-USWHS002-WF-2.pdf) states that simultaneous use of the PAC-USWHS002-WF-2 and the MIFH1/MIFH2 wireless receiver is only compatible with a subset of indoor units individually compatible with the PAC-USWHS002-WF-2 and the MIFH1/MIFH2 wireless receiver due to power requirements. My indoor units are compatible with simultaneous use of the PAC-USWHS002-WF-2 and the MIFH1 wireless receiver. Therefore, my indoor units could simultaneously power the MIFH1 wireless receiver and a device that consumes no more than 2W from the CN105 connector's 12.7V pin.

I have found no information showing that any of my indoor units could simultaneously power the wireless adapter and the MIFH1 wireless receiver were the wireless adapter to consume more than 2W from the CN105 connector's 12.7V pin. Therefore, the power requirement is that the device must consume no more then 2W of power from the CN105 connector's 12.7V pin.

## The WR Connector

[Airzone](https://www.airzonecontrol.com/) enables connection of both its [AZAI6WSCMEL Aidoo WiFi controller](https://www.airzonecontrol.com/na/en/support/technical-details/aidoo-wi-fi-mitsubishi-electric/) and a MIFH1/MIFH2 wireless receiver to connect to an indoor unit using its separate [AZX6ACCSPLMEL CN105 port splitter](https://www.airzonecontrol.com/na/en/support/technical-details/CN105-port-splitter-mitsubishi-electric/). However, [Mitsubishi Electric's PAC-USWHS002-WF-2 Wireless Interface 2](https://www.mitsubishipro.com/products/PAC-USWHS002-WF-2) integrates a port for connecting a MIFH1/MIFH2 wireless receiver rather than relying on a separate device. In addition, the [mUART Project](https://muart-group.github.io)'s [mitsubishi_itp](https://github.com/muart-group/esphome-components/tree/dev/components/mitsubishi_itp) ESPHome component expects the MIFH1/MIFH2 wireless receiver to connect to the same hardware on which it is running.

Therefore, the wireless adapter must provide a connector for connecting a MIFH1/MIFH2 wireless receiver.

Since both the MIFH1 wireless receiver and the MIFH2 wireless receiver come with a cable with a connector that is expected to be plugged into the indoor unit's CN105 connector, the wireless adapter must provide a connector that is physically and electrically the same as the indoor unit's CN105 connector, with the exceptions that there may be less power available on the 12.7V and 5V pins.

 Therefore, the wireless adapter must provide a connector that is physically and electrically the same as the indoor unit's CN105 connector.

## The IU Connector

The wireless adapters's indoor unit (IU) connector could be either the same as or different from the WR connector.

Advantages of the IR and WR connectors being the same include

- fewer component variants in the design, and
- pre-made PAP-05V-S cables are available.

Disadvantages of the IR and WR connectors being the same include

- installer could confuse the two connectors.

If the wireless adapter is connected to the indoor unit using a rollover cable (a.k.a. same direction cable), then the IU and WR connectors will both have 12.7V, GND and 5V on pins 1, 2 and 3 respectively, and the IU connector will have its UART RX and TX pins reversed relative to the WR connector.

This can be addressed either by using a cable that swaps the the UART RX and TX pins or by protecting the UART pins when RX is connected to RX and TX is connected to TX.

Protocol agnostic straight-through cables (a.k.a. reverse direction cables) and rollover cables are available for a wide variety of connectors. However, crossover cables (cables that reverse the communication protocols RX and TX pins) are only available for common protocols with normalized (standardized) connectors and pin-outs (e.g. cables for ethernet over twisted-pair), which this is not. In addition, the current-limiting resistors on the UART pins of both connectors would protect the wireless adapter, the indoor unit and the MIFH1/MIFH2 wireless receiver from potential damage caused by the UART RX and TX pins being crossed when the user confuses the connectors.

Both Mitsubishi Electric's PAC-USWHS002-WF-2 wireless interface 2 and Airzone's AZX6ACCSPLMEL CN105 port splitter use the same type of connector to connect both the indoor unit and MIFH1/MIFH2 wireless receiver. Both connect to the indoor unit using a rollover cable.

 Therefore, the wireless adapter must provide a connector that is physically and electrically the same as the indoor unit's CN105 connector except that the UART RX and UART TX pins are reversed.
