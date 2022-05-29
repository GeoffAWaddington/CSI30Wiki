# MCUTrackPan
MCUTrackPan exists to allow a single rotary or encoder to control Pan, PanWidth, Pan Left and Pan Right (if using Dual Pan). And all without having to create multiple pan zones!

## Setup Your .MST
Before you can use MCUTrackPan, you'll need to make a special modification to your .mst files. 

Let's say you've got RotaryPush and a rotary encoder defined in your .mst...
```` 
Widget RotaryPush1
	Press 90 20 7f 90 20 00
WidgetEnd

Widget Rotary1
	Encoder b0 10 7f [ < 41-48 > 01-08 ]
	FB_Encoder b0 10 7f
WidgetEnd
```` 

...We'll need to **add** one line to the Rotary encoder widgets to make MCUTrackPan work as expected. The RotaryPush message "press" message needs to be added as a "Toggle" as in the example below. This gets used to swap between Pan and PanWidth or PanL and PanR. **Important:** this line gets added in addition to the RotaryPush widgets, it does not replace them.

```` 
Widget RotaryPush1
	Press 90 20 7f 90 20 00
WidgetEnd

Widget Rotary1
	Encoder b0 10 7f
	FB_Encoder b0 10 7f
	Toggle 90 20 7f
WidgetEnd
```` 

## Setup MCUTrackPan and MCUTrackPanDisplay in your .Zon
Now that the rotary widgets we'll be using for Pan have the added toggle message in place, we just need to add the MCUTrackPan message to the correct action. Of course, if your surface has displays, you'll probably want to see those pan values so we also have MCUTrackPanDisplay to cover that. Here's an example below from a typical MCU type setup.

```` 
Zone "Channel"
     TrackNavigator
     DisplayUpper|                      TrackNameDisplay
     DisplayLower|               	MCUTrackPanDisplay Rotary|
     Rotary|                    	MCUTrackPan  
ZoneEnd
```` 

Those two lines of code control Pan and PanWidth, as well as Pan L and Pan R if you prefer to use Dual Pan. No zone switching, multiple widgets, or special rotary metering modes required. 