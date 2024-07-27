# Software Modes

Ham radios supporting digital modes like DMR or D-Star usually perform the decoding and encoding of
these modes by using a digital signal processing (or DSP) chip.

The DSP chip found in DMR radios from Tytera, Retevis, Baofeng or Radioddity is the Hongrui HR_C6000,
it is specialized for DMR digital mode and cannot be repurposed for other modes.

OpenRTX opens the possibility of decoding digital radio modes which the radio does not support with
the factory firmware. \
Decoding modes in software is done by using the microcontroller to decode and encode signals coming from the RF part of the radio.

The encoding and decoding digital modes with a DSP has its own advantages because the DSP takes care of the complete protocol implementation, this means
that the radio manufacturer will not have to implement it in the radio firmware, also the baseband follows the protocol
timings and fires an interrupt to the microcontroller when a certain protocol event happens (like the start of a DMR timeslot).

Unfortunately decoding digital modes in software has an hardware requirements: it requires the target radio to have some 
__audio paths__ which are mandatory:
- RF to MCU: Used to receive signal to be decoded
- MCU to RF: Used to transmit encoded signal
- MIC to MCU: Used to capture audio to be encoded
- MCU to SPK: Used to reproduce decoded audio
