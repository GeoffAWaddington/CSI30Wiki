A Message Generator that represents a control that measures its current position using a 14 bit value (ie. 16384 possible values). 

It's defined using the following syntax:     
```
Widget Fader1
	Fader14Bit e0 7f 7f
	FB_Fader14Bit e0 7f 7f
WidgetEnd
```
A message will be sent between e0 00 00 and e0 7f 7f representing the current position of the fader.

## Adding Touch Messages
If your control surface supports "Touch" messages for faders and encoders, these should also be defined in the widget as shown in the example below:

```
Widget Fader1
	Fader14Bit e0 7f 7f
	FB_Fader14Bit e0 7f 7f
	Touch 90 68 7f 90 68 00
WidgetEnd
```

In this example, touching Fader1 generates a message of 90 68 7f, and releasing Fader1 generates a second message of 90 68 00. 