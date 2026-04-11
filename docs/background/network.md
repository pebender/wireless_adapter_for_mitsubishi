# Our Networked Home

## Wired and Wireless Ethernet

Even though it was just the mid/late 1990s, it was obvious to me that distributing multimedia and home automation control in the home would be would be done over an IP network. [UCSD](https://www.ucsd.edu) where I attended university had been on the Internet while it was still the ARPANET. Qualcomm had been on the Internet since before I started there in 1992. So my believe everything would be carried over an IP network was natural.

Initially, our house relied on dial-up service to connect to the internet. Our phone lines were in bad shape so we regularly only got 9,600bps. So, when Time Warner Cable offered Internet connectivity we quickly switch from a dial-up modem to a cable modem.

In the late 1990s, I bought an [Aironet Wireless Communications](https://en.wikipedia.org/wiki/Aironet) AP4800 access point and PC4800 PCMCIA card. Being able to sit on my couch with my company-issued laptop and access the Internet was freeing.

I could do this before using the [cdmaOne (TIA/EIA/IS-95)](https://en.wikipedia.org/wiki/CdmaOne) and [1xEV-DO (TIA/EIA/IS-856)](https://en.wikipedia.org/wiki/Evolution-Data_Optimized) my company had been developing because the local cellular company offered cdmaOne async data and because my house was in the coverage area of our test 1xEV-DO network. However, compared to the the PC4800 PCMCIA card, the cdmaOne hardware and 1xEV-DO prototype hardware was bulky.

After buying and using the Aironet Wireless Communications wireless LAN devices, a few of us attempted to convince Qualcomm to develop 802.11 modems. However, after a rather limited effort, the project was canceled. Instead, Qualcomm purchased Airgo Networks in 2006 and Atheros Communications in 2011.

In 2001, I had my house wired with Category 5 UTP so that I could have wired Ethernet throughout the house. Some people thought I was crazy and told me I should either install more RJ6 coax or install fiber. I just told them that everything, including audio and video would be carried over IP soon and that residential IP would be transported by Ethernet over Cat 5/5e/6 UTP copper and Wi-Fi, not coax or fiber.

## Network Server

I had a server on the network. The server was computer running Red Hat Linux with hard-drives in a [RAID 1](https://en.wikipedia.org/wiki/Standard_RAID_levels#RAID_1) configuration. The network server performed various functions over the years, including

- a [DHCP server](https://www.isc.org/dhcp/),
- a [DNS server](https://www.isc.org/bind/),
- an [SMB file server](https://www.samba.org) for documents, audio and video,
- an [LDAP server](https://www.openldap.org),
- a [RADIUS server](https://www.freeradius.org),
- an [IMAP server](https://www.cyrusimap.org) including SPAM filtering,
- a [MisterHouse server](https://github.com/hollie/misterhouse/wiki),
- a [SlimServer (aka Squeezebox server)](https://wiki.lyrion.org/index.php/Squeezebox_Server.html)
- a [MythTV](https://www.mythtv.org) backend, and
- a [Minecraft server](https://www.minecraft.net/en-us/download/server)

## Migrating Away from DIY Solutions

By the late 2000s, work and my two wonderful boys were consuming more on my time. In addition, what has since been diagnosed as [autistic burnout](https://en.wikipedia.org/wiki/Autistic_burnout) was affecting my mental health. I no longer had the time and or the motivation to maintain my DIY network.

In 2010 we moved and I started simplifying the network.

- I didn't install any home automation hardware in the new house,
- I stopped working on MiniMyth in 2013,
- I stopped using LDAP for account management,
- I stopped using RADIUS for Wi-Fi access control,
- I migrated the DHCP, DNS, and SMB file server functions to a dedicated
[Synology&reg; DiskStation DS214play](https://global.download.synology.com/download/Document/Hardware/DataSheet/DiskStation/14-year/DS214play/enu/Synology_DS214play_Data_Sheet_enu.pdf),
- I started using
[Plex](https://www.plex.tv)
rather than MythTV for pictures, music, videos and DVDs, and
- I started using Roku along side my MiniMyth frontends.

In 2016, I moved again.

The inconvenient location of some light switches in the new house motivated a return to home automation.

- I started using the SmartThings Hub V2 as the center of my home automation,
- I started using Amazon Echos for voice control of devices exported by SmartThings and for music playback,
- I started using [Nuvyyo TabloTV](https://www.tablotv.com) rather than MythTV for recording television,
- I started using [Orbit B-Hyve](https://www.orbitonline.com/pages/b-hyve-the-smart-way-to-water) for irrigation control,
- I started using [Flume](https://flumewater.com) for tracking water usage in order monitor irrigation water usage and detect leaks (Flume has notified my a multiple dripping faucets, running toilets, irrigation leaks and a hot water pipe leak under the slab of the house. I believe, both water companies and insurance companies should provide a Flume to each of their customers.),
- I replaced the front door lock with a Schlage Connect Z-Wave Smart Deadbolt so that my wife and I would not have to take our house keys when we went on walks, and
- I replaced inconveniently located light switches with [Caséta by Lutron](https://www.casetawireless.com/us/en) light switches (the light switches in the house do not have neutral wires so my choice was limited.)
