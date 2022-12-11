Modifiers are a way to indicate that you want one control to perform different actions under different circumstances. For example, this could be when another button is pressed in combination with the control (like your keyboard behaving differently when you hold the Shift key down), or it could be when a button is held down for a longer period of time. 

The full list of available modifiers is:
* [[Shift|Modifiers#global-modifiers]]
* [[Option|Modifiers#global-modifiers]]
* [[Control|Modifiers#global-modifiers]]
* [[Alt|Modifiers#global-modifiers]]
* [[Touch|Modifiers#Touch]]
* [[InvertFB|Modifiers#InvertFB]]
* [[Hold|Modifiers#Hold]]
* [[Flip|Modifiers#Flip]]
* [[Marker|Modifiers#Marker]
* [[Nudge|Modifiers#Nudge]
* [[Scrub|Modifiers#Scrub]
* [[Zoom|Modifiers#Zoom]
* [[Toggle|Other Actions#togglechannel]]
* [[Increase|Modifiers#Increase-Decrease]]
* [[Decrease|Modifiers#Increase-Decrease]]

## Global Modifiers

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

## Local Modifiers
You can configure any Global Modifiers to impact only the local surface by setting up "Local Modifiers" in your CSI.ini. See [[Page and Surface Configuration Options|CSI.ini#page-and-surface-configuration-options]] for instructions.

## Latching Modifiers
If your surface sends release messages on button press, then you can do a quick press and release to "latch" a modifier. Example: let's say you want to use a few actions that require a Shift modifier. Quickly pressing and releasing the Shift button will engage the latch mode, which is the same as continuing to hold down the Shift button. A quick press and release turns the latching off.

## Chaining Multiple Modifiers
You're not limited to one modifier like Shift or Control or Alt. You can combine them to add additional capabilities to your surface. Here are some examples of what that might look like...

```
Shift+Control+Button
Control+Alt+Button
Shift+Control+Alt+Button
Shift+Control+Alt+Option+Button
```

## Modifiers Work on Displays Too!
Did you know that you can use modifiers on your displays? Here is an example where the lower displays show the track volume, until you hold down Shift, when the TrackPans are displayed. 

```
     DisplayLower|                      TrackVolumeDisplay
     Shift+DisplayLower|                MCUTrackPanDisplay
```

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

Next, you will need to actually create the touch modifier in your .zon file. 

```
Touch+DisplayLower1
```

Which in the context of a SelectedTrackNavigator could allow us to do something like this where the track pan display always appears on the lower display, unless you're actually touching the fader, at which point, the track volume display will appear in the lower display.

```
Zone "SelectedChannel"
     SelectedTrackNavigator
     DisplayUpper1                      TrackNameDisplay
     DisplayLower1                      TrackPanDisplay
     Touch+DisplayLower1                TrackVolumeDisplay
     Fader1                             TrackVolume
ZoneEnd
```

Touch modifiers also work in a TrackNavigator context if you want all channels in the .zon to respond similarly. Here's the same zone as above with a TrackNavigator. 

```
Zone "Channel"
     TrackNavigator
     DisplayUpper|                      TrackNameDisplay
     DisplayLower|                      TrackPanDisplay
     Touch+DisplayLower|                TrackVolumeDisplay
     Fader|                             TrackVolume
ZoneEnd
```

Similarly, Touch can be used in other .zon files/types, including fx.zon's!

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

## Marker
The Marker modifier was designed for MCU-style surfaces with a "Marker" button, allowing you to combine that button to create a range of marker-related actions similar to the examples below. 
```
    Marker+Up                   Reaper 40613       // Delete marker near cursor                         
    Marker+Down                 Reaper 40157       // Insert marker at current or edit position                  
    Marker+Right                Reaper 40173       // Go to next marker or project end                      
    Marker+Left                 Reaper 40172       // Go to previous marker or project start
```

## Nudge
The Nudge modifier was designed for MCU-style surfaces with a "Nudge" button, allowing you to combine that button to create a range of nudge-related actions similar to the examples below. 
```
    Nudge+Up                    Reaper 41925     // Item: Nudge items volume +1dB
    Nudge+Down                  Reaper 41924     // Item: Nudge items volume -1dB
    Nudge+Left                  Reaper 41279     // Item edit: Nudge left by saved nudge dialog settings 1
    Nudge+Right                 Reaper 41275     // Item edit: Nudge right by saved nudge dialog settings 1
```

## Scrub
The Scrub modifier was designed for MCU-style surfaces with a "Scrub" button, allowing you to combine that button to create a range of scrub-related actions similar to the examples below. 
```
    Decrease+Scrub+Jogwheel     Reaper 40084     // Transport: Rewind a little bit
    Increase+Scrub+Jogwheel     Reaper 40085     // Transport: Fast forward a little bit
```

## Zoom
The Zoom modifier was designed for MCU-style surfaces with a "Zoom" button, allowing you to combine that button to create a range of zoom-related actions similar to the examples below. 
```
    Zoom+Up                     Reaper 40111     // Zoom in vertical                                            
    Zoom+Down                   Reaper 40112     // Zoom out vertical                                                       
    Zoom+Right                  Reaper 1012      // Zoom in horizontal                                      
    Zoom+Left                   Reaper 1011      // Zoom out horizontal         
```

## Toggle
See [[ToggleChannel|Other Actions#togglechannel]] for details on how to use the toggle modifier.

## Increase, Decrease
Now you can assign two different Reaper actions to a single encoder based on which way the encoder is being turned, Counter-Clockwise (CCW) or Clockwise (CW). We do this via the Decrease and Increase modifiers. Note: these modifiers only work with Encoders. The examples below all show the JogWheel but this functionality will work with any encoder.

```
Zone "Zoom"
    Decrease+JogWheel      Reaper 41326   // Decrease track height
    Increase+JogWheel      Reaper 41325   // Increase track height
ZoneEnd
```

**Important Note:** for this to work on your JogWheel, you will need to update your .mst file. If you're coming from a prior version of CSI, you probably have two or more separate JogWheel widgets and those widgets probably are defined using Press instead of Encoder. So replace your JogWheel widgets to look like this (assuming MCU-style surface):
```
Widget JogWheel JogwheelWidgetClass
	Encoder b0 3c 7f
WidgetEnd
```

The following native CSI actions support this same syntax:
```
TrackBank
VCABank
FolderBank
SelectedTrackSendBank
SelectedTrackReceiveBank
SelectedTrackFXMenuBank
TrackSendBank
TrackReceiveBank
TrackFXMenuBank
```

Example:
```
Zone "Buttons"
    Decrease+JogWheel      TrackBank -1
    Increase+JogWheel      TrackBank  1
ZoneEnd
```