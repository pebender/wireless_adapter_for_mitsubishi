# Wireless Adapter Enclosure

## Design

I took a conservative approach to the design. The design has a top and bottom. The bottom has four holes for mounting the enclosure to the wall using the drywall anchors. The bottom has four risers with M3 threaded holes for mounting the PCBA to the bottom. The PCBA is attached to the risers using standoffs so that top can be attached to the bottom using four M3 screws that go through the top and screw into the standoffs.

I chose to print using a SLA (resin) printer rather than a FDM (plastic) printer because

- the printing cost of SLA is less than printing cost of FDM at JLCPCB, and
- the finish of SLA is smoother than the finish of FDM.

### PCBA Mounting Holes

The bottom has four risers for attaching the PCBA to the bottom. Each riser has an M3 threaded hole. These four M3 threaded holes align with the four M3 holes in the PCBA.

Because the enclosure is made from resin not plastic, methods for adding strong M3 threaded holes that require melting the enclosure won't work.

I tried printing each riser with a hole to receive an M4 to M3 thread repair insert. It turned out it didn't work because the SLA printer could not reliably print M4 threads. So, I tried three different methods for creating strong M3 threaded holes.

1. print each riser with a hole to receive a 10mm long M6 to M3 thread repair insert ("bottom_T" in the FreeCAD design),
2. print each riser with a hole to receive a 10mm long M6 to M3 thread repair insert and to receive an anchor screw from the underside ("bottom_TWA" in the FreeCAD design), and
3. print each riser with a hole to receive a 10mm long M3 hex coupling nuts and to receive an anchor screw from the underside ("bottom_HWA" in the FreeCAD design).

The 10mm long M6 to M3 thread repair insert are standardized because they are essentially a M6 screw. I used the [HARFINGTON 10mm long M6 to M3 thread repair insert](https://www.amazon.com/dp/B0FDB66YRK). Unfortunately, the 10mm long M3 hex coupling is not standardized because the outer diameter is manufacturer dependent. I designed the riser hole around the [uxcell 10mm long M3 hex coupling nut](https://www.amazon.com/dp/B09MWGV53L). For anchor screws, I used the [uxcell 8mm long flat countersunk head, hex drive M3 screws](https://www.amazon.com/dp/B07WJLFW2W).

To add extra strength, I thought I could add thick viscosity CA glue.

- for (1), I thought I could add glue to the M6 threads
- for (2), I thought I could add glue to the M6 threads and to the M3 threads of the anchor screw, and
- for (3), I thought I could add glue to the outside of the M3 hex coupling nuts and to the M3 threads of the anchor screw.

However, adding glue to (1) proved problematic. Glue got into some of the M3 threads, which made it difficult or impossible to screw in an M3 screw. Perhaps I could have avoided this had I put a M3 screw while screwing in the glue-coated thread repair insert. I tried to force the screw in hopes it would clear the glue. Instead, when I tried to remove the forced screw, I removed the thread repair insert. I discovered that the tight fit of the M6 threads had forced the glue out of the threads, making the glue useless. So, I abandon the idea of gluing in the thread repair inserts and the anchor screws.

Since I destroyed too many of (1), I will use either (2) or (3). Since I have an affinity for the thread repair inserts, I will likely use (2).

### Top and Bottom Attachment

I didn't attempt to design the top and bottom to snap together because I didn't know how to design a snap mechanism that's flexible enough to snap together but not so flexible as to break. Instead, I chose to use screws to hold the top and bottom together. Because of restrictions imposed by my different indoor unit locations, using a single design necessitated that the screw(s) be on top of the top rather than the sides of the top and that the top lift straight off the bottom. Therefore, I attached the PCBA to the bottom using four M3 standoffs and attached the top to the bottom by using using M3 screws through the top into the standoffs. The standoffs are 10mm long. The screws are 15mm long, have flat countersunk heads, and are the same color as the top. As a result, the screws blend in with the top reasonably well. The standoffs are from [JUNCHEN assorted M3 standoffs and spacers](https://www.amazon.com/dp/B0B2MPSH6T). The screws are from [Glarks assorted flat countersunk heads, phillips drive M3 screws](https://www.amazon.com/dp/B0DLZTHS4R).

### Antenna Keep-out

Per section 1.4.7 of [ESP32-C6 Hardware Design Guidelines](https://documentation.espressif.com/esp-hardware-design-guidelines/en/latest/esp32c6/esp-hardware-design-guidelines-en-master-esp32c6.pdf), the antenna should have a keep-out of 15mm. To help ensure the keep-out, the x direction and y direction keep-outs are inside the enclosure. The z direction keep-out is not completely inside the enclosure. However, there is unlikely to be metal in the z direction as the z-direction is into the wall and into the room.

## Manufacture

This is the first time that I have had an enclosure 3D printed.

I chose [JLCPCB](https://jlcpcb.com) because they

- Are reasonably priced, and
- Are timely.

I made two runs of the enclosure. The first run was a test version. The second run contained the "production" version. The first version worked except for the M4 to M3 thread repair inserts. The second run replaced the M4 to M3 thread repair inserts with three options: M6 to M3 thread repair inserts, M6 to M3 thread repair inserts and anchor screws, and M3 hex coupling nuts and anchor screws. Since the three bottom versions in the second run shared the same top and the top was more expensive than the bottom, it made sense to try multiple methods for creating M3 holes.