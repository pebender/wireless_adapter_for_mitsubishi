# Power

## Overview

The power design uses

- a [Texas Instruments TPS2121RUX](https://www.ti.com/product/TPS2121) power multiplexer to select whether the wireless adapter is powered by the IU connector's 12.7V pin or the USB connector's VBUS pins with priority given to power from the IU connector's 12.7V pin when present,
- a [Texas Instruments TPSM5601R5HRDA](https://www.ti.com/product/TPSM5601R5H) switching power module to convert the multiplexed voltage to 3.3V,
- capacitors at the input and output of the TPSM5601R5HRDA to reduce output voltage ripple caused by the power module's switching nature and the load's variable current demand, and
- a filter before the input capacitors to reduce the conducted EMI caused by the power module's switching nature to a level that meets the CISPR 22 Class B requirements.

In addition, the power design uses power from the IU connector's 5V pin to power the 5V side of the operational connectors' level shifter.

## Power Multiplexer

In order to power the wireless adapter from the IU connector's 12.7V pin during normal operation and from the USB connector's VBUS pins during firmware development, there must be a way to safely and reliably arbitrate between the two potential power sources. To accomplish this, the design uses the [Texas Instruments TPS2121RUX](https://www.ti.com/product/TPS2121) power multiplexer to multiplex the 12.7V supply from the UI connector and the 5V supply from the USB connector so that the wireless adapter is powered by the 12.7V supply from the indoor unit during normal operation and by the 5V supply from the USB host during firmware development. The 12.7V supply from the UI connector takes priority over the 5 supply from the USB connector. This enables firmware development while connected to the indoor unit.

I chose the Texas Instruments TPS2121RUX power multiplexer because it handles up to a 24V input, which is more than the 19.3V limited to by the [surge protection](./connection.md#power-pin-voltage-surge-protection).

### Rejected Power Multiplexer Options

The design could have assumed that the IU connector and the USB connector would never be connected at the same time, which would have eliminated the need for the power multiplexer. However, that would put the indoor unit, the USB host and the wireless adapter at risk. In addition, it would make firmware more difficult Therefore, I did not take this approach.

The design could have used a mechanical solution such as a jumper or a switch to select the power source. However, jumpers can come loose and both jumpers and switches must be put in the correct position by the user. So, a mechanical solution is susceptible to user error. Therefore, I did not take this approach.

The design could have used Schottkey diodes for power ORing. However, the ~0.5V voltage drop across the diode translates to ~4% power loss when being powered by the 12.7V supply, which is significant when the ESP32-C6-WROOM-1 recommendation for a power supply of 1.65W (3.3V @ 500mA) and the device must draw no more than 2W from the 12.7V supply. Therefore, I did not take this approach.

The design could hae used discrete transistors and resistors for power ORing. As the Texas Instruments TPS2121RUX costs ~$1.00 in small quantities, the discrete solution would very likely be cheaper. However, the ASIC requires fewer components and is more likely to work correctly on the first try. Maybe in the next iteration, I will replace the TPS2121RUX with discrete transistors and resistors. But for this iteration, I did not take this approach.

The design could have done power ORing after conversion to 3.3V. Doing this would have allowed the use of the less expensive [Texas Instruments LM66200DRL](https://www.ti.com/product/LM66200). However, ORing after the conversion to 3.3V would have required separate step down converters for the UI connector's 12.7V supply and the USB connector's 5V supply. This would cut into the cost savings from being able to use the less expensive Texas Instruments LM66200DRL, and would have required more wireless adapter area. Therefore, I did not take this approach.

## Step Down Converter

### Power Requirements

Per Section 1.3 of [ESP32-C6 Hardware Design Guidelines (dated 2026.03.03)](https://documentation.espressif.com/esp-hardware-design-guidelines/en/latest/esp32c6/esp-hardware-design-guidelines-en-master-esp32c6.pdf) the recommended power supply has a voltage of 3.3V and an output current of no less than 500mA.

Per Section 6.4 of [ESP32-C6-WROOM-1 & ESP32-C6-WROOM-1U Datasheet (version 1.4)](https://www.espressif.com/sites/default/files/documentation/esp32-c6-wroom-1_wroom-1u_datasheet_en.pdf), the maximum current required by the ESP32-C6-WROOM-1-N8 processor and radio is 382mA, which is needed when transmitting the IEEE 802.11b 1Mbps DSSS waveform at full power. This does not include any power required by any non-radio peripherals (e.g., the UARTs and their associated GPIOs) that might be in use. In addition, this does not include any power required by hardware other than the ESP32-C6-WROOM-1-N8 (e.g., the UART level shifter).

Based on information about the Mitsubishi Electric's PAC-USWHS002-WF-2 Wireless Interface 2, the wireless adapter [must not draw more than 2W from indoor unit's 12.7V supply](../requirements.md#maximum-allowed-power-consumption). Therefore, the step down converter that converts 12.7V to 3.3V must be at least 82.5% efficient to meet the 500mA requirement when the indoor unit is powering the wireless adapter.

I have found no information about the maximum power the wireless adapter can draw from the indoor unit's 5V supply.

### Power During Normal Operation

During normal operation, nothing is connected to the USB connector. Therefore, the wireless adapter is powered by the indoor unit through the wireless adapter's IU connector during normal operation.

#### 3.3V

The ESP32-C6-WROOM-1-N8 and the ESP32-C6-WROOM-1-N8 side of level shifter uses 3.3V. The ESP32-C6-WROOM-1-N8's radios use most of the power. 

From the PAC-USWHS002-WF-2's documentation, [I concluded that the PAC-USWHS002-WF-2 is powered by the indoor unit's 12.7V power source](../requirements.md#maximum-allowed-power-consumption). Therefore, I chose to power most of the wireless adapter from the indoor unit's 12.7V power source during normal operation.

From the PAC-USWHS002-WF-2's documentation, [I concluded that the PAC-USWHS002-WF-2 draws no more than 2W from the indoor unit's 12.7 power source](../requirements.md#maximum-allowed-power-consumption). From this, I concluded that it's safe for the wireless adapter to draw up to 2W from the indoor unit's 12.7 power source but it might not be safe to draw more. Therefore, the step down converter that converts 12.7V to 3.3V must be at least 82.5% efficient to meet the ESP32-C6 Hardware Design Guidelines' 500mA requirement.

Since a linear step down regulator that converts from 12.7V to 3.3V would be 26.0% efficient at best, the design uses a switching step down regulator for converting from 12.7V to 3.3V.

#### 5V

The indoor unit and wireless receiver side of the level shifter uses 5V.

Converting the 12.7V power source to both 3.3V and 5V would require more circuitry than just converting to 3.3V. One solution is to use an LM7805 to convert from 12.7V to 5V [as appears to be done in at least some indoor units](https://cuttlefishblacknet.wordpress.com/2019/05/). Another solution is to convert from 3.3V to 5V using a boost converter. The level shifting circuit requires relatively little power from either the 3.3V source of the 5V source. Therefore, the wireless adapter uses the 5V source from the indoor unit to power the 5V side of the level shifting circuitry.

### Power During Recovery

A failed over-the-air (OTA) firmware load could put the wireless adapter in a state where OTA firmware loading does not work. If this happens, then the end user should be able to load the wireless adapter's firmware through the USB connector. There is less risk of harm to the end user and the indoor unit, if the wireless adapter is disconnected from the indoor unit when loading firmware using the USB connector. Therefore, I designed the wireless adapter to be powered by either the IU connector or the USB connector.

Since the wireless adapter is not connected to the indoor unit during recovery, the level shifting circuitry does not require power. Therefore, while the USB connector provides power for the wireless adapter's 3.3V circuitry, it does not provide power to the 5V side of the level shifting circuitry.

### 3.3V Output Switching Regulator

The power multiplexer may feed the switching regulator from the USB connector's VBUS pins or from the IU connector's 12.7V pin.

The wireless adapter's USB connector is a USB 2.0 full speed (12Mbps), high power (1.5A @ 5V) connection. Per the USB 2.0 specification, the voltage on the supplied to the device must be at least 4.35V for low power devices and 4.75V for high power devices. Since all high power devices must be able to operate as low power devices, the switching regulator must operate normally with an input voltage as low as 4.35V.

I don't know the indoor unit's 12.7V source characteristics. However, it's voltage is unlikely to drop as low as the lower limit set by the USB connector. In addition, the Texas Instruments TPS2121RUX operates normally with input voltages up to 22V and survives transient input voltages up to 24V. Finally, the Texas Instruments TVS1400DRV I selected for TVS protection on the 12.7V IU and WR connector pins, has a maximum clamping voltage of 19.3V. Therefore, as long as the switching regulator operates normally with an input voltage as high as 22V and survives a transient input voltage as high as 24V, the Texas Instruments TPS2121RUX and not the switching regulator will be the limiting factor.

#### Selecting the Switching Regulator

Compared with linear regulators, switching regulators tend to generate more radiated and conducted electromagnetic interference (EMI) and tend to have greater output voltage ripple. And, improper wireless adapter layout can unnecessarily increase the switching regulator's EMI and output voltage ripple. Conversely, proper input and output filters can decrease the switching regulator's EMI and output voltage ripple.

Many companies provide switching regulator solutions with various levels of integration. I have never designed a power supply circuit. However, during my career, I have seen how hard it can be to get power supply designs right. As a result, I have a strong preference for solutions that

- are highly integrated,
- meet radiated EMI requirements without additional components,
- use shielded components,
- have detailed test results, and
- have tools for designing proper power supplies including the input capacitors, the output capacitors and the conducted EMI filter.

Finally, I have a preference for a solution that does not necessitate a wireless adapter that has more than two layers or components on both sides of the wireless adapter because each of these things can increase the cost of wireless adapter manufacture and assembly as well as limit where the wireless adapter can be manufactured and assembled.

After looking at the offerings from various companies, I settled on [power modules from Texas Instruments](https://www.ti.com/power-management/dcdc-power-modules/power-modules-integrated-inductor/overview.html). Texas Instruments provides good datasheets for their power modules, including recommendations for supporting passive components and recommendations for wireless adapter layout. In addition, they provide [WEBENCH&reg; Power Designer](https://webench.ti.com/power-designer/switching-regulator) to assist with choosing supporting components and designing input EMI filters. Finally, they provide various application notes that explain how components are chosen and EMI filters are designed.

After eliminating offerings that did not

- support the required input voltage range,
- support the output current range,
- achieve the necessary efficiency,
- meet radiated EMI requirements,
- use either shielded inductors or shielded packaging,
- recommend a wireless adapter layout with four-layer, and/or
- recommend a wireless adapter layout with components on both sides,

I settled on the [Texas Instruments TPSM5601R5HRDA](https://www.ti.com/product/TPSM5601R5H). It is in a family that includes

- TPSM560R6: 600mA output, 400kHz switching frequency,
- TPSM560R6H: 600mA output, 1MHz switching frequency,
- TPSM5601R5: 1.5A output, 400kHz switching frequency,
- TPSM5601R5H: 1.5A output, 1MHz switching frequency,
- TPSM5601R5S: 1.5A output, 400kHz switching frequency, spread spectrum, and
- TPSM5601R5HS: 1.5A output, 1MHz switching frequency, spread spectrum

Any one in the family meet the requirements. However, the TPSM5601R5HRDA has the best availability and price at JLCPCB. Since the 400kHz switching frequency versions appear to have somewhat higher efficiency and the spread spectrum versions should have better EMI characteristics, if availability and cost of other members of the family improve, then I may change the design to use one of the other parts in the family.

#### Choosing External Components

The [TPSM5601R5H datasheet](./references/parts/Texas_TPSM5601R5H.pdf), the [TPSM5601R5H evaluation wireless adapter](https://www.ti.com/tool/TPSM5601R5HEVM), and [WEBENCH&reg; Power Designer](https://webench.ti.com/power-designer/switching-regulator) provide recommended/suggested values for external components required by the TPSM5601R5H. Sometimes the values they suggest differ.

##### Choosing the Feedback Resistors

The TPSM5601R5HRDA needs a top feedback resistor and a bottom feedback resistor to set the output voltage. Section 8.3.1 of the TPSM5601R5HRDA datasheet provides an equation that relates the ratio of the two resistors to the desired output voltage, recommends the value of the top feedback resistor should 10kΩ, and recommends the value of the top feedback resistor should not exceed 1MΩ. Figure 8-1 of the TPSM5601R5HRDA evaluation wireless adapter shows that the evaluation wireless adapter uses a 10kΩ to feedback resistor. However, the design recommended by WEBENCH&reg; Power Designer uses a 100kΩ top feedback resistor.

I chose a top feedback resistor of 51kΩ and bottom feedback resistor of 22kΩ. I chose these values because

- their ratio yields an output voltage of 3.3V,
- the top feedback resistor is below the datasheet's maximum recommended value of 1MΩ,
- the values of the resistors are large enough that their current requirements do not noticeably reduce the current output at typical or high loads, and
- they are resistors that are in JLCPCB's basic parts tier: [51kΩ](https://jlcpcb.com/partdetail/26537-0402WGF5102TCE/C25794) and [22kΩ](https://jlcpcb.com/partdetail/26511-0402WGF2202TCE/C25768).

##### Choosing the Feedforward Capacitor

THe datasheet and the evaluation wireless adapter make no mention of a feedforward capacitor in parallel with the top feedback resistor. However, WEBENCH&reg; Power Designer recommends a 91pF feedforward capacitor.

I chose no feedforward capacitor because

- the datasheet does not mention a feedforward capacitor,
- the evaluation wireless adapter does not have a feedforward capacitor, and
- the WEBENCH&reg; Power Designer recommended capacitor value is not available in the JLCPCB basic tier.

In the future, I may

- take a closer look at [Optimizing Transient Response of Internally Compensated
DC-DC Converters With Feedforward Capacitor [Texas Instruments SLVA289B)](https://www.ti.com/lit/an/slva289b/slva289b.pdf),
and/or
- perform more simulations,

to decide whether or not the wireless adapter would benefit sufficiently from a feedforward capacitor.

##### Choosing the Output Capacitors

Section 8.3.3 of the TPSM5601R5H datasheet provides a graph of minimum output capacitance as a function of output voltage. According to this graph, the minimum output ceramic capacitance for a 3.3V output is approximately 80uF. The TPSM5601R5H evaluation wireless adapter follows the datasheet and has two 47μF ceramic output capacitors. Just like the TPSM5601R5H evaluation wireless adapter, the design recommended by WEBENCH&reg; Power Designer has two 47μF ceramic output capacitors.

Likewise, I chose two 47μF because

- they are the values recommended by the datasheet, the evaluation wireless adapter and WEBENCH&reg; Power Designer, and
- they are capacitors that are in JLCPCB's basic tier: [47μF](https://jlcpcb.com/partdetail/97327-CL31A476MPHNNNE/C96123).

##### Choosing Input Capacitors

Section 8.3.2 of the TPSM5601R5H datasheet states that "a minimum input capacitance of 9.4μF (2 × 4.7μF) of ceramic type" are required. The TPSM5601R5H evaluation wireless adapter follows the datasheet and has two 4.7μF ceramic input capacitors. However, the design recommended by WEBENCH&reg; Power Designer is one 4.7μF and two 22nF capacitors.

The TPSM5601R5H is designed for symmetric placement of input and output capacitors. Therefore, the single 4.7μF input capacitor recommended by WEBENCH&reg; Power Designer is not ideal. In addition, the 4.7μF capacitors available in JLCPCB's basic tier suffer a significant loss in capacitance at a DC bias of 12.7V.

If the WEBENCH&reg; suggested 4.7μF capacitor is replaced with two 2.2μF, then they can be placed symmetrically. In addition, [JLCPCB has a 2.2μF capacitor in the basic tier](https://jlcpcb.com/partdetail/51263-CL31B225KBHNNNE/C50254) that does not suffer significant loss in capacitance at a DC bias of 12.7V.

For input capacitors, I chose two 22nF capacitors and two 2.2μF because

- it is in line with the values suggested by WEBENCH&reg; Power Designer, and
- JLCPCB's basic tier has a [22nF](https://jlcpcb.com/partdetail/21834-CL10B223KB8NNNC/C21122)
capacitor and a [2.2μF](https://jlcpcb.com/partdetail/51263-CL31B225KBHNNNE/C50254)

If 9.4μF input capacitance is needed, then two additional 2.2μF would close the gap. However, because the EMI filter adds to the input capacitance, the two additional 2.2μF capacitors may not be needed.

#### Choosing the EMI Filter Design and Components

Switching regulators generate electromagnetic interference (EMI). Care must be taken in the design to minimize EMI. The Texas Instruments' TPSM5601R5HRDA was designed to meet the requirements of CISPR 22 Class B (and EN 55022 Class B) for radiated emissions. However, it does not meet CISPR 22 Class B conducted emissions without an external filter.

Thankfully, [Texas Instruments WEBENCH&reg; Power Designer](https://webench.ti.com/power-designer/switching-regulator) to help with picking the external components needed by the TPSM5601R5H, including the components for the input filter needed to meet CISPR 22 Class B conducted emissions requirements. In addition, Texas Instruments provides application notes and datasheet to help with understanding the various EMI requires as well as to help with understanding and designing the EMI filter,

The components and area required by the EMI filter add cost to the wireless adapter. In addition, it's likely the wireless adapter would not cause problems were it built without the EMI filter. Many switching regulator modules sold to hobbyists like myself don't have a filter to meet CISPR 22 Class B conducted emissions requirements, and I suspect many of them do not meet either CISPR 22 Class B conducted emissions requirements or CISPR 22 Class B radiated emissions requirements. However, I would rather be safe than sorry.

For the EMI filter, I accepted the WEBENCH&reg; design but

- I changed the components from the recommended by WEBENCH&reg; to components available from JLCPCB, and
- I changed the inductor to a shielded inductor.

The filter inductor and dampening capacitor were in the WEBENCH&reg; list of alternate components. The filter capacitor and dampening resistor were not in the WEBENCH&reg; list of alternate components. However, there are similar components on the list that are JLCPCB basic components.

Just as I chose a power module with a shielded inductor because shielding the inductor helps minimize radiated EMI, I chose a shielded filter inductor.
