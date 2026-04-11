# Our Automated Home

## Home Assistant

Currently, [Home Assistant](https://www.home-assistant.io) is at the center of our home automation system.

## Before Home Assistant there was SmartThings

Before switching to Home Assistant, [SmartThings](https://en.wikipedia.org/wiki/SmartThings) was at the center of my home automation system. I liked SmartThings because

- the SmartThings hub supported local control over Zigbee, Z-Wave and IP,
- SmartThings supported a wide selection of locally controllable automation hardware,
- SmartThings supported a wide selection of cloud controllable automation hardware for when there wasn't a reliable locally controllable alternative,
- SmartThing encouraged people to write and distribute custom SmartThings software.

However, after SmartThings acquisition by Samsung, SmartThings changed how software is written and appeared to shift its focus to cloud based functionality. So, I migrated to Home Assistant.

For a while I kept my SmartThings cloud account and SmartThings hub around because of my [Aladdin Connect&reg;](https://www.geniecompany.com/aladdin-connect-/aladdin-connect) Wi-Fi Retrofit Kit from Genie and my [Schlage Connect&trade;](https://www.schlage.com/en/home/products/smart-deadbolts-levers.html#tab-item-0d633d3781-tab) smart deadbolt from Schlage.

### Fixing My Genie Aladdin Connect by Replacing It with ratgdo32

I had been using a Genie Aladdin Connect retrofit kit to connector our two Genie garage door openers to our home automation system. Unfortunately, Genie Aladdin Connect uses the cloud. Some time ago, Genie decided to make incompatible changes the Aladdin Connect cloud API and to release a Aladdin Connect cloud API SDK for the changed API under an OSI incompatible license. Therefore, until either Genie releases the SDK (or the API) under an OSI compatible license or someone who has not been tainted by the SDK successfully reverse engineers the API, it appears Home Assistant will not be able to use the Genie Aladdin Connect cloud API to integrate with Aladdin Connect.

Genie promised to resolve the issue with connecting Home Assistant to the Aladdin Connect cloud. As a stop-gap until Genie made good on their promise, I paired my Genie Aladdin Connect cloud account with my SmartThings cloud account so that SmartThings cloud could control my garage doors though the Genie Aladdin cloud. Then I paired my SmartThings cloud account with my local Home Assistant so that Home Assistant could control my garage doors.

However, after months of apparent inactivity it became clear to me that Genie was unlikely to come through on their promise. This gave me the motivation to look for an open and cloud-free replacement for Genie Aladdin Connect. While I found multiple DIY and non-DIY solutions that would likely have worked, I settled on
[ratgdo32](https://ratcloud.llc/pages/about) because it appeared that it would meet my needs and it allowed me to support someone whose work is helping to make possible home automation that is both open and cloud-free. After some months, I can say that I prefer the ratgdo32 to the Aladdin Connect retrofit kit because

- ratgdo32's Home Assistant integration has been more responsive than Aladdin Connect's Home Assistant integration had been, likely because the Aladdin Connect uses the cloud, and
- ratgdo32's door status information has been more reliable than Aladdin Connect's door status information had been, likely because the Aladdin Connect retrofit kit relies on a wireless tilt sensor to determine the door status whereas ratgdo32 relies on my garage door's open and close limit switches.

Genie has provided a textbook example of why requiring the cloud for a device to provide a local service is never in the customer's best interest. I purchased the Genie Aladdin Connect retrofit kit so that I could integrate my Genie garage door openers with my home automation system. When I purchased the kit, it worked with home automation system through the Aladdin Connect cloud. After I purchased the kit, Genie decided to make incompatible changes to cloud API as well as release the SDK for the new cloud API under an OSI incompatible license. Essentially, Genie decided to force changes that broke the Genie Aladdin Connect retrofit kit. They did so without compensating me of the harm they did to me. The did so without giving me no way to reject the changes and maintain the current functionality.

This is another example of why we should stop buying devices that require a cloud. This is another example of why we should pass laws that make it illegal for companies to make cloud changes that cause a loss of functionality to unless they provide owners a full refund. If they want to run some of the software for the hardware they sell in a cloud, then they should be required to run the cloud in a manner that never reduces the functionality of hardware they have sold.

### Fixing My Schlage Connect Smart Deadbolt By Waiting

Home Assistant in combination with the [Zooz Z-Wave 800 Long Range USB stick (ZST39)](https://www.getzooz.com/zooz-zst39-z-wave-long-range-usb-stick/) drained the batteries in my Schlage Connect smart deadbolt in under a few weeks. As a result, I paired the lock with my SmartThings hub instead. Then I paired my SmartThings cloud account with my local Home Assistant so that Home Assistant could control the lock.

Because removing the lock from one Z-Wave network and adding it to another is somewhat of a hassle, I did not test the lock with each new ZST39 firmware update much less with each Home Assistant Z-Wave update. However, as of [ZST39 firmware v1.50 (SDK v7.22.1)](https://www.support.getzooz.com/kb/article/1352-zst39-800-long-range-z-wave-stick-change-log/), connecting the lock to Home Assistant using the ZST39 no longer drains the lock's battery.

With those two problems solved, I retired my SmartThings hub and my SmartThings cloud account.

## Before SmartThings there was MisterHouse

Before switching to SmartThings, I used [MisterHouse](https://github.com/hollie/misterhouse/wiki). I replaced MisterHouse with SmartThings because I was finding less time to tinker with projects so a turnkey solution was more appealing, and because we moved to a new house so existing configuration was not an issue.

MisterHouse controlled

- my X10 devices
- my plasma TV, and
- my irrigation system.

I wrote software to control the TV through its RS-232 port. There was really no reason to control the TV through its serial port other than the fact that I could control the TV through its serial port. Controlling home hardware through a serial port has been a recurring theme as serial ports are prevalent.

I bought the irrigation controller over the Internet. It was a black metal box with an Ethernet connection and relays for controlling the irrigation valves. It lacked any local control but since I was the only one using the irrigation controller, that was not a problem.

## Before MisterHouse there was X10

Before switching to MisterHouse, the only smart home hardware I used was [X10](https://en.wikipedia.org/wiki/X10_(industry_standard)). It worked most of them and it was relatively affordable.

However, this was before wired and wireless ethernet took over my house.
