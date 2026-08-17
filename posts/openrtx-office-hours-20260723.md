
# Office Hours for OpenRTX meeting, 23 July 2026

Held on 2026-07-23T17:00Z in [https://meet.jit.si/OpenRTX](https://meet.jit.si/OpenRTX).

**Participants:**

 * Hannes DM3MAT
 * Marco DM4RCO
 * Max OE1KHZ
 * Morgan ON4MOD
 * Ryan K0RET
 * Silvano IU2KWO

**Discussion topics:**

 * News from Ham Radio Friedrichshafen fair:
     * FT65 platform support in progress?
     * Need to work on restoring voice prompt builds
         * But Piper TTS is bad
         * Ryan to switch to using a different voice generation model, circulate builds for review from a11y testers
     * Retevis
         * Demo'd to their reps OpenRTX
         * No specific follow ups or next steps at this point

 * Open PRs:
     * Ailunce HD2 Port PR
         * Interesting HRC7000 architecture
         * Excellent work, working on review right now
     * M17 message / message registry work PR
         * No review yet
         * Ryan has low confidence, but it works and he's prepared for rework when Silvano has time for a deep dive

 * Persistence:
     * Considered integrating the slicer and memory savings bits together, decided against this because the group believed that the slicer may have standalone value for special cases
     * Proposal that slicer should be able to take a map for variable slice size: e.g. one slice with M17, one with FM; decided instead that intentionally positioning high vs low churn entries would be enough
     * Morgan working on VFO structure, current approach is funky and coupled to md380 + their CPS expectations; going to reimagine this to be more consistent with
     * We \_don't\_ expect that every setting present in the codeplug should be editable via UI; e.g. Tmes, calibration editing would clutter the UI
     * Planning two CPS: one for constrained radios that will have read-only codeplug to the UI, another that will have littlefs and full editing capabilities

 *  CPS<->Codeplug:
     * Anticipate future working sessions focused just on this
     * Computer-aided transceivers (CAT interface)
         * Working on the new implementation currently based on some past learnings from a hackathon
         * Yaesu/kenwood compatible approach is being chosen
         * If a radio has USB, there is will be one with CAT (serial mode) and one with file transfer (file mode) (either way, CAT must enable file transfer)
         * Mass storage may be hard for qDMR based on the existing work done, dual port challenges too; maybe another approach like dfu would work more easily? To be researched

 * Github bot checking for stale issues:
     * Now that the majority of the issues are cleaned up is not really useful. In some cases is actually becoming a bit annoying: some issues and PRs progress quite slowly and the bot keeps marking them as stale.
     * Marco will try to see if it is possible to label those issues and PRs as "in progress" and have the bot skipping them.

 * Marco will look into setting up a wiki where to move all the hardware documentation
     * PostmarketOS wiki is a good example: for each supported device there is a table of the features and their development status. From each device page is also possible to cross-link to all the other devices which have the same chipset or other components.
     * Migrating the information from openrtx.org can be a bit time consuming and some of the information there is probably outdated. It can be interesting to try using an AI agent to perform the job.
     * After the meeting, Ryan K0RET +1'd this as a big improvement -- he thinks we should consider segmenting the website to be marketing/community info + blog, then wiki for hardware \_and\_ developer documentation

 * Max OE1KHZ is working on an OpenRTX port for the Yaesu FT-65
     * Currently bringing up the basic BSP, still some work to do because the device memory map of the GD32 used in the radio differs from the STM32 one. CMSIS BSP from Gigadevices is terrible, the approach is to take ST CMSIS files and update the memory addresses where necessary. Marco DM4RCO offered to help him with this.
     * His radio got seriously damaged due to an SWD programmer outputting 5V instead of 3.3V: the MCU and RF chip(s) are almost certainly gone. To be decided what to do, in the meantime Max is using a GD32 development board to go on with the work.
     * He sent the firmware binary to HA7DN, which started reverse engineering it to see how the display is managed. Work is in progress, no news for the moment.
     * There is need to find out how to flash the firmware on the radio without using the SWD interface: Yaesu does not seem to provide a firmware update tool, probably there is need to reverse engineer the communication protocol and add it to radio\_tool or other programs.
