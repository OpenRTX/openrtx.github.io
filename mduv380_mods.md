## M17 on the MD-UV380
The Tytera MD-UV380 uses a HR_C6000 DMR baseband, that cannot be repurposed to run M17.
M17 can be encoded and decoded by using the radio MCU, to do so we need to ensure to have the
necessary audio paths between MCU, RF part, speaker and microphone.

There are two audio paths that require some hardware modifications:
- MCU → Spk: Available
- MCU → RF: Available
- Mic → MCU: Requires hardware mod
- RF → MCU: Requires hardware mod

# __WARNING__
This guide is being updated, __do not perform the mod__ as long as this warning is here. \
For questions join [our Discord server](https://discord.gg/jZ9t8XTbmd), or write us an [email](https://openrtx.org/#/?id=the-openrtx-project).

### Equipment required
To perform the Mic → MCU and RF → MCU mods you need:
- Screwdrivers: Torx T6, Torx T8, Philips #0
- Plastic spudger like [this](https://it.aliexpress.com/item/32834353313.html) or similar
- 1x 200KΩ THT (through-hole) resistor with at least 1% tolerance and 1/8W rating
  ([example](https://it.rs-online.com/web/p/resistori-montaggio-a-foro-passante/0149054/))
- some 30AWG Kynar wire
- A small tip soldering iron
- A desoldering pump or solder wick
- A file to create a small groove on the aluminum heat sink
- (optional but recommended) 20x stereoscope like [this](https://www.amazon.it/BRESSER-8852000-Stereomicroscopio-Bresser-Junior/dp/B001UJJGV4)
- (optional but recommended) Hot glue gun to glue wires to PCB

### Disassembling the radio
- Remove the battery and the antenna
- Remove the belt clip if present
- Pull out the volume and channel knobs to remove them
- Unscrew the three nuts around the antenna connector, volume and channel knobs using a pair of
  pliers
- Unscrew the two upper screws (Torx T8) on the back and remove the plastic part they hold
- Unscrew the two lower screws (Torx T8) on the back
- Use the spudger to remove the bottom part of the aluminum heat-sink from the outer case
- Be careful when separating the heat sink from the outer case, the two are joined by the display
  flat cable and the speaker wires, which you should remove first.
- At this point you should have the internal radio assembly separated from the case\
(Ignore the green and blue wires, they were used to sniff the AT1846S I2C bus)\
![MD-UV380 internal assembly](_media/uv380_front_back.jpg)

### Accessing the RF side PCB
Both mods (Mic → MCU and RF → MCU) require accessing the RF side of the PCB, 
the one which faces the heat sink.

- Remove the 11 Philips #0 screws from the logic side of the PCB \
![MD-UV380 back screws](_media/uv380_back_screws.jpg)
- Remove the 2 Torx T6 screws from the side button PCB \
![MD-UV380 side screws](_media/uv380_side_screws.jpg)
- Desolder the antenna connector as shown in the picture \
The antenna connector is fixed to the heat sink with two screws,
removing them should NOT be necessary to perform the mod
![MD-UV380 antenna connector](_media/uv380_antenna.jpg)
- Use a pair of tweezers or a spudger to gently pull the side button PCB out of the heat sink. \
Be careful not to break the solder joints between the button PCB and the main PCB. \
![MD-UV380 side tweezers](_media/uv380_side_tweezers.jpg)
- Now carefully detach the heat sink from the PCB assembly, you should be able to see the RF side of
  the PCB. \
![MD-UV380 RF side before](_media/uv380_rf_before.jpg)


### Mic → MCU
The MD-UV380 like the MD380 has an audio path from the microphone to the MCU that is used for VOX
functionality on the stock firmware.

The Mic → MCU mod consists in bypassing/removing the peak detector diode and capacitor, and add a
resistor to add a bias offset to the mic input.

The mod is explained in the following picture \
![MD-UV380 Mic mod](_media/uv380_mic_mod.jpg)

Steps:
- Remove the capacitor shown in the picture, you can use the hot air gun from a rework station or \
  you can melt the solder in the two contacts and rotate slightly the capacitor to remove it.
  Be careful to not damange the PCB tracks.
- Prepare a short kynar wire, use some tweezers to bend it in a U shape and solder it on the diode \
  to short the two pins. \
  In alternative you can remove the diode (SOT-23) entirely but it will make more difficult
  reverting the mod. \
![MD-UV380 Mic mod diode short](_media/uv380_mic_short.jpg)
- Trim one end of the resistor and bend it in a P shape \
![MD-UV380 Mic mod bent resistor](_media/uv380_mic_resistor.jpg)
- Insert the bent pin of the resistor into the right hole as shown in the picture, and solder it. \
  Trim the other leg of the resistor so that it doesn't go over the golden section of the PCB \
  (otherwise it will make contact with the aluminum heat sink wall. \
- Trim the first leg of the PCB if it protrudes from the other side of the PCB. \
- Check that the resistor does not go in the way of the aluminum heat sink.
- The end result should look something like this, (or maybe better 😉). \
![MD-UV380 Mix mod finished](_media/uv380_mic_after.jpg)

### RF → MCU
The MD-UV380 lacks a direct path from the demodulated RF signal coming from the AT1856S
radio-on-a-chip to the STM32 MCU.
We are going to create it by connecting the `ADC_IVINP` of the HR_C6000 baseband to the `PC13` pin
of the STM32 which can be configured as an ADC input.

An inconvenient of this mod is that the HR_C6000 and STM32 are on opposite sides of the PCB. \
We will use an alignment notch on the PCB to pass the wire, usually this notch matches a pin on the
aluminum heat sink.\

Steps:
- We need to file down the pin and create a small groove in place to avoid pinching the wire. \
Note that on my radio for some reason the pin was removed at the factory, so you may have it or not. \
![MD-UV380 Heat sink modified](_media/uv380_heat_sink.jpg)
- Cut a section of 30AWG Kynar wire, strip and tin one end.
- The `ADC_IVINP` is on the upper side of the HR_C6000, and the wire can be soldered on two \
  pins of a resistor and capacitor that are connected together. This makes the soldering easier and \
  gives it more mechanical strenght. \
![MD-UV380 RF HR_C6000 wire](_media/uv380_rf_wire1.jpg)
- Use the tweezers to bend the wire to route it towards the notch in the PCB. 
  Make sure that the wire lays flat on the PCB. \
![MD-UV380 RF wire bent bottom](_media/uv380_rf_wire2.jpg)
- Bend the wire on the notch toward the logic side of the PCB. \
![MD-UV380 RF wire bent top](_media/uv380_rf_wire3.jpg)
- Use the tweezers to route the wire towards the left side of the STM32. \
![MD-UV380 RF wire routed top](_media/uv380_rf_wire4.jpg)
- Trim and tin the other end of the wire and solder it on pin 18 of the STM32. \
![MD-UV380 RF wire soldered top](_media/uv380_rf_wire5.jpg)
- Check that the wire does not get pinched by the heat sink, in case it does, file a deeper groove.

### Results
- The final results with both mods applied should look like this (ignore the green and blue wires).\
![MD-UV380 after mic rf mod](_media/uv380_mod_after.jpg)
- Close the heat sink over the PCB, paying attention to the side keys PCB.
- Make sure that the antenna connector is screwed exactly perpendicular to the rest of the radio
- Solder the antenna connector pin
- Reassemble the radio by following the disassembly instructions backwards.
- TIP: If you plan to disassemble the radio again shortly, reassemble it without the rubber gasket.\
  It will make disassembling it again easier.
