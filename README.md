# A Wireless Adapter for a Mitsubishi Heat Pump Indoor Unit

This project is neither associated with nor endorsed by Mitsubishi Electric. By using this project, you may void your warranty or break your heat pump.

## The Wireless Adapter

The wireless adapter is a device for connecting an indoor unit of [our Mitsubishi Electric heat pump system](./docs/requirements.md#existing-system) to [our home automation system](./docs/background/automation.md). The wireless adapter is similar to [Mitsubishi Electric's PAC-USWHS002-WF-2 Wireless Interface 2](https://www.mitsubishipro.com/products/PAC-USWHS002-WF-2), except the wireless adapter is Open Source and doesn't depend on a cloud service.

The wireless adapter has three distinct parts

- the Open Source [hardware](./docs/hardware.md),
- the Open Source [enclosure](./docs/enclosure.md) and
- the Open Source [firmware](./docs/firmware.md).

### The Wireless Adapter Hardware

The [hardware](./docs/hardware.md) is likely both overly and poorly designed. Relative to many other people's designs, my design pays (more) attention to safety, including correct logic levels, safe power consumption and transient voltage suppression.

The hardware is a two-layer printed circuit board assembly (PCBA). I did the design and layout using the Open Source PCB design suite [KiCad](https://www.kicad.org). In addition, I did the step down power supply design using [Texas Instrument's WEBENCH&reg; Power Designer](https://webench.ti.com/power-designer/). I paid [JLCPCB](https://jlcpcb.com) to fabricate and assemble the hardware. I licensed the hardware design and layout under the [MIT license](./LICENSE.txt).

### The Wireless Adapter Enclosure

The [enclosure](./docs/enclosure.md) is likely overly conservative. I had no experience designing 3D printed enclosures, and I wanted to be sure the enclosure wasn't too fragile.

I did the enclosure design using the Open Source 3D parametric modeler [FreeCAD](https://www.freecad.org). I paid JLCPCB to 3D print the enclosure. I licensed the enclosure design under the MIT license.

### The Wireless Adapter Firmware

I contributed nothing to the [firmware](./docs/firmware.md). I built it from the Open Source [ESPHome](https://esphome.io) and the Open Source [mUART Project](https://muart-group.github.io)'s [mitsubishi_itp](https://github.com/muart-group/esphome-components) ESPHome component,

## The Motivation

I undertook this project because

- I needed a project,
- I wanted to learn what it takes to develop hardware today,
- I wanted to learn what it takes to develop enclosures, and
- I wasn't satisfied with our current solution for connecting the indoor units of our Mitsubishi Electric heat pump system to our home automation system. It depends on two [Honeywell THM6000R7001 RedLINK&reg; Internet Gateways](https://www.resideo.com/us/en/pro/products/air/thermostats/thermostat-accessories/redlinkr-internet-gateway-with-ethernet-cable-and-power-cord-thm6000r7001-u/) and the [Honeywell Home Total Connect Comfort](https://mytotalconnectcomfort.com) cloud. Because of its reliance on the RedLINK&reg; wireless protocol, it's relatively slow. In addition, because its reliance on the cloud it's relatively unreliable.

I wouldn't have undertaken this project had others not already done the work to connect their Mitsubishi Electric heat pumps to their home automation systems, including

- [Hadley Rich's hacking of his Mitsubishi heat pump)](https://nicegear.nz/blog/hacking-a-mitsubishi-heat-pump-air-conditioner/),
- [SwiCago's HeatPump software](https://github.com/SwiCago/HeatPump),
- [Geoff Davis (esphome-mitsubishiheatpump software)](https://github.com/geoffdavis/esphome-mitsubishiheatpump), and
- [the mUART project's software](https://muart-group.github.io).

In addition, I wouldn't have undertaken this project without the knowledge and confidence I gained as

- a high school student in [John McCollum](https://www.eetimes.com/john-mccollum-hs-teacher-taught-electronics-with-love/)'s electronics class at [Homestead High School](https://en.wikipedia.org/wiki/Homestead_High_School_(California)),
- a hardware intern at [NCR](https://www.ncr.com),
- a PhD student under [Jack Wolf](https://en.wikipedia.org/wiki/Jack_Wolf) at the [University of California San Diego (UCSD)](https://ucsd.edu), and
- a 20+ year employee at [Qualcomm](https://www.qualcomm.com).

Finally, I wouldn't have undertaken this project had an existing design contained

- a level shifter for the UART signals,
- a power module designed to meet the required efficiency and the EMI standards, and
- a reasonable level of Transient Voltage Suppression (TVS).

My [background](./docs/background.md) provides more insight into why I undertook this project.

## The Evolution

I didn't design the hardware and the enclosure as a unified project. Initially, I designed the hardware with the intent of using an off-the-shelf enclosure. So, I designed the hardware to fit in the off-the-shelf enclosure. However, I ran into problems mounting the hardware in the off-the-shelf enclosure as well as cutting the necessary openings in the off-the-shelf enclosure. So, I decided to make a custom enclosure. However, I already had the hardware fabricated and assembled. Therefore, I designed the enclosure around the existing hardware. Had I designed them together, both the hardware and the enclosure would have been different.

## References

[References](./docs/references.md)
