# Wireless Adapter Hardware

I designed the wireless adapter hardware to meet [these requirements](./requirements.md).

The current design has the same circuit design and same PCB layout as the design I had fabricated. However, there are some changes

- I updated from KiCad 9.0.x to KiCad 10.0.x,
- I switched from a hierarchical schematic to a flat schematic,
- I removed the wall mounting passthrough hole from the PCB, and
- I added the name and revision to the PCB.

## Architecture

The high level block diagram below shows basic functions of the wireless adapter.

The [USB connector](./hardware/firmware.md#usb-connector) (along with the [boot and reset switches](./hardware/firmware.md#switches)) enables easy firmware development, debug and recovery.

The [IU connector](./hardware/connection.md#the-iu-connector) and [WR connector](./hardware/connection.md#the-wr-connector) connect to the indoor unit and wireless receiver respectively.

The [power multiplexer](./hardware/power.md#power-multiplexing) switches between the 5V power provided through the USB connector and the 12.7V power provided through the IU connector, with priority given to the IU connector's 12.7V power when present. This allows the USB connector to power the System on a Chip (SoC) during development and recovery.

The [power module](./hardware/power.md#33v-output-switching-regulator) converts the power from the output of the power multiplexer to the 3.3V needed to power the SoC as well as power the SoC side of the level shifter.

The [level shifter](./hardware/connection.md#level-shifter) converts the UART signals between the 3.3V signals expected by the SoC and the 5V signals expected by the indoor unit (connected via the IU connector) and the wireless receiver (connected via the WR connector).

The SoC provides the Wi-Fi, Thread and Bluetooth LE interfaces to the indoor unit and the wireless receiver.

```mermaid
block
columns 7

USB["USB Connector"]
space:3
IU["IU Connector"]
space
WR["WR Connector"]

space:7

space:2
PWR_MUX["Power Multiplexer"]
space:1

block:LS["Level Shifter"]:3
    columns 3
    LS_IU_5V["TX/RX @5V"]
    LS_5V["5V"]
    LS_WR_5V["RX/TX @5V"]
    space:3
    LS_IU_3V3["TX/RX @3.3V"]
    LS_3V3["3.3V"]
    LS_WR_3V3["RX/TX @3.3V"]
end

space:7

space:2
PWR_MOD["Power Module"]
space:4

space:7

block:SOC["System on a Chip"]:7
  columns 7
  USB_JTAG["USB Serial / JTAG"]
  space:1
  SOC_3V3["3.3V"]
  space:1
  UART_0["UART 0"]
  space:1
  UART_1["UART 1"]
  space:7
  CPU["CPU"]
  space:1
  WIFI["Wi-Fi"]
  space:1
  THREAD["Thread"]
  space:1
  BLE["Bluetooth LE"]
end

USB -- "5V" --> PWR_MUX
USB -- "D+/D-" --> USB_JTAG
USB_JTAG -- "D+/D-" --> USB

PWR_MUX --> PWR_MOD
IU -- "12.7V/5V/GND" --> WR
IU -- "12.7V" --> PWR_MUX

IU -- "5V" --> LS_5V
IU -- "UART TX/RX" --> LS_IU_5V
LS_IU_5V -- "UART TX/RX" --> IU
WR -- "UART RX/TX" --> LS_WR_5V
LS_WR_5V -- "UART RX/TX" --> WR

LS_IU_5V --> LS_IU_3V3
LS_IU_3V3 --> LS_IU_5V
LS_WR_5V --> LS_WR_3V3
LS_WR_3V3 --> LS_WR_5V

PWR_MOD -- "3.3V" --> LS_3V3
LS_IU_3V3 -- "UART TX/RX" --> UART_0
UART_0 -- "UART TX/RX" --> LS_IU_3V3
LS_WR_3V3 -- "UART RX/TX" --> UART_1
UART_1 -- "UART RX/TX" --> LS_WR_3V3

PWR_MOD -- "3.3V" --> SOC_3V3
```

# Fabrication and Assembly

This is the first time that I have had a PCBA fabricated and assembled.

I chose [JLCPCB](https://jlcpcb.com) because they are a inexpensive, one-stop shop. They

- provide [parts](https://jlcpcb.com/parts),
- provide [PCB fabrication](https://jlcpcb.com/pcb-fabrication/fr4-pcb),
- provide [PCB assembly](https://jlcpcb.com/pcb-assembly),
- are reasonably priced, and
- are reasonably fast.

Because I chose JLCPCB, I made sure the parts I used are available from [JLCPCB parts](https://hlcpcb.com/parts). When possible, I chose resistors, capacitors and diodes that were available as basic or promotional extended parts because they are cheaper during PCB assembly. In addition, I chose parts that support surface mounted technology (SMT) rather than through hole technology (THT) because they are cheaper during assembly. The one exception is the JST S05B-PASK-2(LF)(SN) connector used for the IU connector and WR connector because through hole connectors tend to be more securely attached than surface mount connectors.

I made two versions of the PCBA. The first version was the test version. The second version was the "production" version. The first version worked but the USB-C connector was poorly placed. The second version was similar to the first version except the switches and ESD diodes were replaced with basic or promotional extended parts, the LEDs were removed, and the layout was changed.

## The Completed Design

### The Parts List

 The PCBA contains the parts

- System on a Chip: [Espressif ESP32-C6-WROOM-1-N8](./hardware/soc.md) (U1)
- Level Shifter: [Texas Instruments TXS0104EPW](./hardware/connection.md#level-shifter) (U2)
- Power Multiplexer: [Texas Instruments TPS2121RUX](./hardware/power.md#power-source-multiplexing) (U7)
- Power Module: [Texas Instruments TPSM5601R5HRDA](./hardware/power.md#33v-output-switching-regulator) (U8)
- TVS Devices: [Texas Instruments TVS1400DRV](./hardware/connection.md#power-pin-voltage-surge-protection) (U3, U5) and [Texas Instruments TVS0500DRV](./hardware/connection.md#power-pin-voltage-surge-protection) (U4, U6)
- ESD Diodes: [R+O H5VL10B](./hardware/connection.md#signal-pin-voltage-surge-protection) (D1-D7)
- USB Connector: [G-Switch GT-USB-7010ASV](./hardware/firmware.md#usb-connector) (J3)
- IU Connector: [JST S05B-PASK-2](./hardware/connection.md#connectors) (J1)
- WR Connector: [JST S05B-PASK-2](./hardware/connection.md#connectors) (J2)
- Switches: [XunPu TS-1088-AR02016](./hardware/firmware.md#switches) (S1, S2)
- Capacitors: [Samsung Electro-Mechanics capacitors](./hardware/passive.md#capacitors) (C1-C15)
- Resistors: [Uni-royal thick film 0402/0603/0805/1206/1210/1812/2010/2512 series resistors](./hardware/passive.md#resistors) (R1-R13)
- Inductor: [Murata DFE201610E-R47M=P2 inductor](./hardware/passive.md#inductors) (L1)

When selecting parts, I selected

- Espressif parts because I am familiar Espressif's development tools,
- Texas Instruments parts because I am familiar with Texas Instruments' design documentation and tools, and
- JLCPCB basic and promotional extended parts because they are less expensive during assembly.

### The Schematic

![schematic](./hardware/schematic/wireless_adapter_for_mitsubishi.svg)

### The PCBA

![PCBA](./hardware/pcba/wireless_adapter_for_mitsubishi.png)

The completed PCBA is a two-layer, rigid PCBA. The bottom layer contains a ground plane. The top layer contains ground pours, the power pours, the power traces, the signal traces and the parts.