Modifiers are a way to indicate that you want one control to perform different actions under different circumstances. For example, this could be when another button is pressed in combination with the control (like your keyboard behaving differently when you hold the Shift key down), or it could be when a button is held down for a longer period of time. 

# Global Modifiers

To avoid creating dependencies between zone files (where a control specified in one is referenced in another), there are a set of global modifiers available:
 
* Shift
* Option
* Control
* Alt

In your .mst file you can name one or more controls on your surface with whichever of these you wish to use:

````     
Widget Shift
	Press 90 46 7f 90 46 00
WidgetEnd

Widget Option
	Press 90 47 7f 90 47 00
WidgetEnd

Widget Control
	Press 90 48 7f 90 48 00
WidgetEnd

Widget Alt
	Press 90 49 7f 90 49 00
WidgetEnd  
````     

Now you can use them like this in any of your zone files, but for consistency, we should all put our modifiers in the "Buttons|" Zone, which is typically included in the "Home" Zone:

````   
Zone "Buttons|"
 	Shift Shift
        Option Option
        Control Control
        Alt Alt

 	Save Reaper 40026
	Shift+Save Reaper 40022

	Undo Reaper 40029
	Shift+Undo Reaper 40030
ZoneEnd
````    

Additionally, any widget can be defined to act as a modifier...

````   
Zone "Buttons|"
 	Zoom            Shift
	Rewind 		Rewind
	Shift+Rewind	Reaper 40042	//go to start of project
ZoneEnd
````   

...In the above example, the Zoom widget was defined within the zone to act as the Shift modifier. That Shift modifier can then be combined with other widgets like the Rewind button to trigger additional actions. In this case, pressing Zoom+Rewind will return to the start of the project.

Modifiers are global to a page, and as long as they are defined in a zone somewhere in that page, they can be used anywhere.

You can also combine them together, such as:

````    
Shift+Control+Option+Alt+SomeButton SomeAction    
````    
etc., but note that Shift+Control is the same as Control+Shift

**Tip:** If you've defined a Shift modifier, a quick press of that button will enter a latching mode where the Shift modifier will remain active until pressed again.

# Built-in Modifiers

## Touch
Touch messages can be used to tell CSI "when I'm touching this parameter, do this other thing." But Touch is a special kind of modifier in that a few things need to exist for it to work. 

First, you need a control surface that's capable of sending Touch and Touch Release messages for a given control. Examples: fader touch messages on an MCU device, rotary touch messages on a Eucon surface, or an OSC parameter set to work with touch messages. If you have a surface that supports touch message for certain controls, you need to define the touch messages in your .mst/.ost files. Here is an example using an MCU's fader1.

```
Widget Fader1
	Fader14Bit e0 7f 7f
	FB_Fader14Bit e0 7f 7f
	Touch 90 68 7f 90 68 00
WidgetEnd
```

Next, you will need to actually create the touch modifier in your .zon file. The modifier name is not actually **Touch**; it's variable. The syntax for a touch modifier is **[WidgetName]Touch**. So the widget name in the above example was Fader1 and therefore, the modifier name for the touch message is:

```
Fader1Touch+DisplayLower1
```

Which in the context of a SelectedTrackNavigator could allow us to do something like this where the track pan display always appears on the lower display, unless you're actually touching the fader, at which point, the track volume display will appear in the lower display.

```
Zone "SelectedChannel"
     SelectedTrackNavigator
     DisplayUpper1                      TrackNameDisplay
     DisplayLower1                      TrackPanDisplay
     Fader1Touch+DisplayLower1          TrackVolumeDisplay
     Fader1                             TrackVolume
ZoneEnd
```

Touch modifiers also work with the | character in a TrackNavigator context if you want all channels in the .zon to respond similarly. Here's the same zone as above with a TrackNavigator. Notice that the modifier is now **Fader|Touch**.

```
Zone "Channel"
     TrackNavigator
     DisplayUpper|                      TrackNameDisplay
     DisplayLower|                      TrackPanDisplay
     Fader|Touch+DisplayLower|          TrackVolumeDisplay
     Fader|                             TrackVolume
ZoneEnd
```

## InvertFB 
Up is down, down is up, on is off, and off is on. Example: Reaper EQ FX bypass On means control surface EQ Active light should be Off.

```` 
InvertFB+SomeButton FXParam 16 "Bypass"
```` 

## Hold
A one-second hold of a button will result in a different action. For instance, pressing the "Click" button toggles the metronome, but holding the Click button opens the metronome settings. **Important:** for the Hold modifier to work, your button/widget must transmit release messages (otherwise, CSI will not be able to decipher between a hold and a button press). 

```` 
Click				Reaper 40364 		//Toggle metronome
Hold+Click			Reaper 40363		//Show metronome settings
````    

## Flip
The Flip modifier was designed for MCU-style surfaces with a "Flip" button that is meant to put TrackPan on faders for easier pan adjustment. See the example below for most common use-case.
```
Zone "Track"
    Rotary|                     MCUTrackPan
    Fader|                   	TrackVolume 
    Flip+Fader|                	TrackPan 
```