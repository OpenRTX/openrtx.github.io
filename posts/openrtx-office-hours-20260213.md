
# Office Hours for OpenRTX meeting, 13 February 2026

Held on 2026-02-13T17:00Z in [https://meet.jit.si/openrtx](https://meet.jit.si/openrtx), facilitated by Morgan and Silvano.  

**Participants:**
 
 * Edgetriggered
 * Jim N1ADJ
 * Marc HB9SSB
 * Marco DM4RCO
 * Morgan ON4MOD
 * Niccolo IU2KIN
 * Rick KD0OSS
 * Ryan K0RET
 * Ryan N2BP
 * Silvano IUKWO

**Discussion topics:**

 1. Introductions
 2. Code of conduct reminder
 3. Release managers update

### Release managers update

 * v0.4.3: current release, released on 15-12-2025. Contains major updates and bugfixes to M17 demodulator.
 * v0.4.4: next intermediate release
     * Scope:
         * M17 metadata
         * Mic input oversampling
         * M17 packet?
         * M17 SMS?
 
     * Date:
         * ASAP
 
     * Next steps:
         * Complete delivering the scope

* v0.5.0 next
    * Scope:
        * APRS RX (PR #396); Silvano as reviewer
        * Spectrum view (PR #355); Silvano as reviewer
        * NVM, flash driver; Silvano/Morgan as collaborators, landing straight to master
        * Settings and VFO state persistence; Silvano plans to restructure settings overall, shift vfo state outside of chunks
            * Open question: in memory device or external, this shapes the approach that we need to take?
            * Lots of overlap here between VFO datastructures and codeplug datastructures 
            * Reminder of OpenRTX principle: we should be able to rollback to manufacturer's firmware
            * Decision: we need to refactor the backup tool for it to work well and we don't have time for this, so we are avoiding overwriting used sectors for the time being.
                * Silvano determining the proper place to store on DM1701 and DM1801 given this constraint
    * Date:
        * Not known or decided yet

    * Next steps:
        * Cutting a "next release" branch for integration

### Lean coffee
* *[IU2KWO] Path to codeplug feature*
    * Future refactor of CAT to add an aditional text based commands via serial
    * v0.6+ is when we would revisit this and try to improve backup/restore tooling
* *[IU2KWO] Voice prompts issue*
* *[IU2KIN] TTWR 2.1 support*
    * RTX works, GPS works, display works, flashing ?? firmware works
    * Remaining goal: M17 support, APRS; but hardware limitations need resolving
        * E.g. baseband input is on an ADC pin where DMA channel doesn't work; if we don't modify the hardware, to enable M17 would need to use other CPU as makeshift DMA
    * Nicco is intermittently working on this w/ Edgetriggered
    * Next updates will continue to be in Discord
* *[HB9SSB] CAT interface, RTXLink*
    * Continuing to work towards this slowly, goal is to have this work with his other existing software (e.g. [https://trx-control.msys.ch)](https://trx-control.msys.ch))
* *[HB9SSB] adding lua to openrtx*
    * Current experiment: could the radio be scriptable, could lua be added to openrtx?
    * Constraint: lua memory footprint is 256kb flash, VM is 64kb in ram; unclear which platforms could support this aside from CS7000M17+
    * See lua.msys.ch for info re lua C integration more broadly
    * Next step would be determine how scripts would be managed
    * Potential memory cutting step: exclude the compiler from the ortx build, at the expense of risking loading arbitrary bytecode
    * Perhaps this is a good Google Summer of Code project? By way of lab lua sponsorship
* *[N1ADJ] Status of C62 porting?*
    * Unfortunately there are both a tech roadblock and dev resourcing constraints
    * Andrej (author of this change) had some big issues on audiopath; C62 has audio dsp with separate firmware; without loading that firmware, it appears to be impossible to access; how do you start up the DSP? Using a forked zephyr kernel, and the public version doesn't match the official firmware and neither appear to work for us
    * Stalled here as some of the contributors have moved on from the project
    * Open question: how should we balance supporting this platform, especially given that linht now is many people's interest in this platform?
    * Opportunity for C62 remains having an M17-capable radio that requires no hardware mods
* *[IU2KWO] Eventually, we need new hardware platform support*
    * Some of our supported radios are already out of production
    * Our supported ones are aging and at risk of EOL
    * Risk: in some markets these radios are not compliant and users can get fined for importing these; we have some platforms that are compliant, why aren't we focusing on those
* *[HB9SSB] Funding models for OpenRTX*
    * What sort of support from hardware manufacturers can we get?
    * Can we get better support from these manufacturers in the form of donated devices to maintainers? Perhaps there's an opportunity for IARU to support
    * Perhaps a business entity is needed to ensure this is possible; offline discussion needed here with maintainers
    * E.g. Lua is accepted into SPI, this unlocks funding opportunities
    * Two main alternatives here:
        * Finding a legal umbrella org, pairing this with opencollective
        * Creating a legal entity (e.g. foundation) but this creates a lot of admin overhead
    * Next steps: Marc will sync with Silvano re opportunities for support from IARU
* *[K0RET] Adoption of codec2-mod to save cpu (and therefore battery)*
    * Silvano was considering this migration already
    * Expect already reduction in flash and CPU time, which would enable future exploration; address for instance mduv380 that audio+accessibility UI thread is struggling
    * There are challenges currently with codec2-mod, and it only makes sense to adopt if the non-static allocated memory is not a smaller footprint; it only makes sense to migrate if this is efficient on both flash and static memory (moving everything to heap)
* *[K0RET] we have a stale PR / issue problem; can we adopt a tool-driven policy to fix this?*
    * Datapoint: 38% of open PRs were opened > 6 months ago
    * [https://github.com/actions/stale](https://github.com/actions/stale) is a great starting point
    * It would be important for us to ensure that some issues are not closed, e.g. verified bug reports
    * Action: Ryan to setup a fork that shows this behavior and review with maintainers - Update: pr made 2026-02-13
* *[K0RET] Development on OpenRTX is hard; how can we make contributors more successful?*
    * A couple of ideas based on past feedback: apply clang-format to the whole project despite its shortcomings, migration from meson to cmake, Integration of testing library (e.g. catch2), better linux platform drivers (e.g. pulseaudio support, writing baseband output to file), add static analysis (e.g. clang-tidy, cppcheck) 
    * More documentation about the entire structure is needed; we have some AI generated docs that are good! Niccolo and Silvano are due to review the materials and consider integrating it to the dev site; also adding a dedicated page on the site about coding style, dev workflow hints (e.g. utilizing git `--fixup`)
    * Action: Ryan to share where in Github to auto-enable running GH Actions on external PRs - Update: Done 2026-02-13
* *[IU2KWO] Is the website still the best place for dev docs?*
    * Should we split to a wiki for dev docs? Static `docs` folder in the main repo?
    * Morgan to think about this alongside other actions post as part of his next big task
* *[???] Please make it possible to use M17 TX on the TYT VHF 380 radio.*
    * Still work required; nobody is actively working on this though
    * Silvano previously researched, seems like troubleshooting on other side of HC5000
    * Unclear how valuable this is at this point given that these devices are EOL
* *[???] Please add some information on a public website about the APRS development timelime*
    * Please refer to the release planning section at the top of the notes
    * No specific timeframe for APRS TX possible, need to look to community to contribute this as the core contributors are focusing on persistence
