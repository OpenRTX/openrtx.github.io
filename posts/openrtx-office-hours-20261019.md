
# Office Hours for OpenRTX meeting, 19 October 2026

Held on 2026-10-19T17:00Z on Jitsi.

**Participants:**

 * Hannes DM3MAT
 * Marco DM4RCO
 * Morgan ON4MOD
 * Ryan K0RET
 * Silvano IU2KWO

**Discussion topics:**

 * Releases:
     * v0.4.5 released on 2026-10-04
     * v0.5.0 planned to be released before Christmas, ideally in around a month.
 * Persistence:
     * Work on persistence of device settings and VFO state almost ready to be mainlined. Silvano is finishing some refactoring and testing. Once done he'll move on to bringing up USB support to the CS7000 Plus with PR #439.
 * Ryan is working on bugfixes and improvements to increase the reliability of the M17 demodulator. Work tracked on taiga epic [https://tasks.openrtx.org/project/openrtx/epic/862](https://tasks.openrtx.org/project/openrtx/epic/862)
     * Some work has alredy been mainlined, soft Viterbi decoder and another PR are currently waiting for review.
     * Ryan estimates the work remaining to be mainlined in around 10 PRs. The changes bringing the biggest advantages will be mainlined first and included in v0.5.0, the rest will come at a later stage.
 * Retevis C62:
     * first set of commits merged into a dedicated branch. Some fixup commits addressing review comments are touching more than one file at a time and need to be untangled. Silvano will work on this.
     * Full device support to be mainlined when all the features are ready, at the current status the commits in the main repository support only FM and there is no real advantage in including the port into the upcoming release(s).
     * Waiting for the second PR bringing in M17 support and the remaining parts. It seems that there is still something to be fixed with the DSP firmware
     * Marco is waiting for the BSP to be mainlined to be able to reuse the BK4819 driver in the Radtel RT-4D radio driver
 * Mailing list:
     * mailing lists is now up and running with three public mailing lists available: users, developers and ptscd
 * Codeplug:
     * Morgan ON4MOD will start working on the PTSCD specification with the help of Hannes DM3MAT
     * On consumer radios a change in the channel data structures across firmware updats is usually handled by the PC software: channel data is read before the update and then wrote back with the new format after the firmware update. To be considered to do the same, as this will give flexibility on how to lay down the data structures.
     * Changes in channel data structures are not a big deal for qdmr, provided that the interface to upload/download data from the radio is stable.
     * Handling file transfer can be an issue for qdmr: to make things easier OpenRTX can map files to a set of virtual memory addresses (32-bit wide, adequately spaced), mimiking a flash memory
 * Wiki:
     * Marco DM4RCO agrees on having a look on how to set-up the wiki in the next weeks.
