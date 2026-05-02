
# Office Hours for OpenRTX meeting, 24 April 2026

Held on 2026-04-24T17:00Z in [https://meet.jit.si/OpenRTX](https://meet.jit.si/OpenRTX), facilitated by Silvano.

**Participants:**

 * Edgetriggered
 * Jeff KF0CSJ
 * Marco DM4RCO
 * Ryan K0RET
 * Ryan N2BP
 * Silvano IU2KWO

**Discussion topics:**

 1. Introductions
 2. Code of conduct reminder
 3. Release managers update

### Release managers update

 * v0.4.4: incoming release
    * all features are landed, we're ready to cut the release.
    * investigating the off-frequency bug on CS7000, the fix may part of the release.

 * v0.5.0: next release
    * re-scoping of the release: we have M17 SMS, APRS RX, and spectrum scan all close to go
    * Make another batch of changes to settings
    * Persistence work that Morgan is leading is ongoing, we can approach this as a future minor release
    * Need Ryan+Ryan to do an additional review on packet queue:
        * [N2BP] we may be missing the concept of acknowledged packets that APRS has
        * [IU2KWO] probably not, this is an application layer feature in APRS; similar to how packet type on M17 should be set by higher layers;
          eventual goal of having these sorts of handlings move out of the RTX driver

### Lean coffee
* *[VO1RFX] UI Refactor / Revamp*
    * So far: made UI c code compatible with C++
        * [IU2KWO] UI in C++ thumbs up
            * From scratch re-write with C++ in parallel(?)
            * Preserve ui.h interface
            * Retain UI, change backend
    * Have a doc with some loose thoughts together, but it's an ambitious scope
    * C-based Refactor: https://github.com/anthonydotmoe/OpenRTX/tree/ui
    * Borrow concepts from LVGL, Ryan thinks we need to make a high level diagram since not everyone is aware / we should capture where our initial focus is
    * We see two stages here with isolated problem areas:
        * Backend changes: refactors to make the UI more flexible; UI is rewritten, but the requirements stay the same
        * Frontend changes: new UI and UX design
    * Some key architectural decisions we'll want to make:
    * Stack navigation?
        * [K0RET] thinks not now
        * [VO1RFX] will defer to experts :) neutral on this, no UI expertise
    * Migration path?
        * [IU2KWO] thoughts about having a UI feature freeze to avoid a constant rebase / chasing new contributions
    * UX designer -- who's leading that?
        * Can we borrow the LinHT designs, or ask for their help?
        * Yes -- they started with us actually! And Silvano can enlist his family to help provide guidance / direction to develop prototypes/mock-ups into robust designs.
        * [N2BP] also is offering to help, he has resources too

* *[VO1RFX] Side buttons*
    * https://gist.github.com/ranguli/d152d15e32ddff684a5b02d7393312af
    * Primary question: how do we remap the macro button without breaking the accessibility re-reading feature
    * [IU2KWO] Three features, two buttons:
        * Macro Menu (short and long press), accessibility reading, and now adding a monitor feature
        * Proposal: short press: replay last prompt, long press: open squelch (if on FM mode) or on M17 disable CAN match _and_ disable callsign filtering
        * [DM4RCO] we should get whatever solution we pick reviewed by an a11y tester/user; Silvano to ask the original VP author
        * TODO: remove or re-map boot time hash key check for enabling at boot time
            * (If remap, should we remap to the key used for voice prompt replay?)

* *[N2BP] Sampling rate for Tx: 48000 for M17? Does it need to be this high for 4FSK?*
    * [IU2KWO] The higher the better; it's 4.8ksps, don't fewer than 1 sample per symbol, right now we get 10 samples per symbol;
      RX is 24kHz due to system constraints, maybe this can be improved; maybe APRS TX can be lower though; today M17 handles this
      by splitting it into 2x 20ms preparations, taking advantage of the dma transfer time

* *[IU2KWO] C62?*

    * Silvano hasn't had time here, this is something others can take up and then transition the repo to OpenRTX org later if it interests them
    * MR is here, but the code needs to align with coding standards which is a bunch of stuff to do; Andrej doesn't have time at the moment though; also more to do here around mirroring devtool repositories
    * [K0RET] I could work on this starting June/July timeframe if nobody else is available as a secondary goal priority after UI
    * [IU2KWO] Will try to mirror repos if I end up bored

* *[Jeff KF0CSJ] T-TWR?*
    * What's the current status? Interested in buying a new radio, but trying to decide between C62 or T-TWR because they're not requiring mods to run M17
    * [IU2KWO] Nico and Edgetriggered made a working POC for the ADC (in order to compensate for a hardware issue in teh current revision); edgetriggered has adjustments that need to be merged now
    * We need to prove the DAC is capable for M17 transmission and get M17 reception working through the ADC support. Some UI changes would benefit the unusual hardware interface on this platform.
    * Persistence would be very helpful for configuring pre-defined channels to work around limitations of the physical interface.

* *[N2BP] OpMode_APRS initialization for MDUV380*
    * Opmode doesn't do any initialization (drivers, basebands); need to add this back so that packet decoding works properly
    * Expect another change to the APRS RX PR to add this
    * [IU2KWO] Idea: make opmodes generic at radio driver level, voice vs data mode?

* *[DM4RCO] MD9600: Who has a radio? Different Hardware versions*
    * Currently looking in bringing up the RF stage, all the rest is already working.
    * Similar to the UV380, may need some modifications for M17
    * Problem: it has three different hardware revisions, with differences in the RF stage. It'd be good to have multiple people testing.
