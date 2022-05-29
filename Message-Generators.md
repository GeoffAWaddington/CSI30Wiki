Message Generators allow you to define what MIDI messages the surface sends to CSI:

* [[Press]] - a simple generator that sends a single MIDI message on press, and optionally sends another message when released. Often, but not limited to, a button.
* [[AnyPress|Press#what-is-anypress-]] - a variation of Press needed for some devices.
* [[Fader7Bit]] - sends a MIDI message in a range specifying the current absolute value of the control. 
* [[Fader14Bit]] - like [[Fader7Bit]], but has a larger (or more fine grained) set of values.
* [[Encoders]] - unlike the Faders, an Encoder sends a relative value (increase/decrease) 
* [[MFTEncoder]] - a special encoder only found on the Midi Fighter Twister controller


The general pattern here is to use the [[MIDI Monitor]] to observe what values are sent when your widget is manipulated. So for example, when I press one of the buttons on my controller I see this:

[[images/MIDIMonitor.png]]

The first line showed up when I pressed, the second one when I let go, so a reasonable assumption is the MIDI Generator in my Widget should be:

`Press 90 5d 7f 90 5d 00`

That said, please read the section on [[Press]] for some guidance/recommendations.


