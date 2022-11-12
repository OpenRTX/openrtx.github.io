# Accessiblity

One of the goals of OpenRTX is to make its supported platforms accessible in all senses.
We do this not only by providing an open-source codebase on which to experiment,
but also by including accessibility features such as voice prompts.

OpenRTX now has a Voice Prompt mode for use by those who can't read the screen.
Enable this mode either from the Accessibility Menu, or by holding down the hash
key while powering on the radio. If the latter method is used, voice prompts
will be set to the most verbose level of 3 if they were previously off.
The voice prompt levels have the following settings:

- **Off**: no beeps or spoken prompts.
- **Beep**: Beeps are emitted for key presses, and a different beep is emitted for the first menu option of each menu.

Toggling settings on and off can be detected by a low beep for off and a high beep for on.
Levels 1 through 3: Three levels of spoken feedback increasing in verbosity with level 1 being the lowest, and 3 being the highest.

- **Level 1**: menus are spoken.
- **Level 2**: everything is spoken.
- **Level 3**: everything is spoken with extra information where appropriate. E.g. At level 3, the word mode clarifies the meaning of fm or M17 etc.

The top side key is the voice key. Press quickly to repeat the last prompt and press and hold to speak a full summary of the current VFO, channel or GPS screen.
You can also choose between normal and phonetic rendering of letters, choose this from the Accessibility menu.
