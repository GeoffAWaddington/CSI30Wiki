* [[NoAction|Other Actions#NoAction]]
* [[FixedTextDisplay|Other Actions#FixedTextDisplay]]
* [[FixedRGBColorDisplay|Other Actions#FixedRGBColorDisplay]]
* [[ClearAllSolo|Other Actions|ClearAllSolo]]
* [[NoFeedback|Other Actions#NoFeedback]]
* [[Broadcast|Broadcast and Receive]]
* [[Receive|Broadcast and Receive]]
* [[ToggleChannel|Other Actions#togglechannel]]
* [[WidgetMode|WidgetMode]]
* [[SetWidgetMode|WidgetMode#setwidgetmode]]
* [[SendMIDIMessage|Other Actions#sendmidimessage]]
* [[SetXTouchDisplayColors|Other Actions#setxtouchdisplaycolors-restorextouchdisplaycolors]]
* [[RestoreXTouchDisplayColors|Other Actions#restorextouchdisplaycolors]]
* [[Speak|Other Actions#speak]]

## NoAction
The cunningly named NoAction action, does nothing. I'll just pause for a second while that sinks in. Now, contrary to what you might be thinking, this can be really useful. 

Let's say you [[GoZone|Zone-Actions]] from "Home" to "FX". As discussed on the [[Zones]] page, this effectively overlays "FX" over the top of "Home". Widgets mapped in "FX" take over "Home" behaviour. Widgets mapped in "Home" but NOT in "FX" still work as they did before.

However, what if you want to stop this behavior? Maybe in "FX", we want to cancel the "Home" behaviour to avoid operator errors. For example, perhaps in "Home" I have the transport controls mapped to widgets, but in"FX" I want to "disable" these mappings?

In that case, map them to NoAction in "FX" and they will defiantly ignore your presses until you [[GoZone|Zone-Actions]] back to "Home".
```
Zone "VST: UAD Teletronix LA-2A Silver (Universal Audio, Inc.)" "LA2ASlv"
     Rotary1             FXParam 0 
     RotaryPush1         NoAction
     DisplayUpper1       FixedTextDisplay "Thresh"
     DisplayLower1       FXParamValueDisplay 0

     Rotary2             FXParam 3
     RotaryPush2         NoAction  
     DisplayUpper2       FixedTextDisplay "HF Emph"
     DisplayLower2       FXParamValueDisplay 3

     Rotary3             FXParam 1
     RotaryPush3         NoAction 
     DisplayUpper3       FixedTextDisplay "Output" 
     DisplayLower3       FXParamValueDisplay 1 

     Rotary4             NoAction
     RotaryPush4         FXParam 2 [ 0.0 1.0 ]
     DisplayUpper4       FixedTextDisplay "CompLim" 
     DisplayLower4       FXParamValueDisplay 2

     Rotary5             FXParam 4 [ 0.0 0.50 1.0 ]
     RotaryPush5         NoAction     
     DisplayUpper5       FixedTextDisplay "Meter" 
     DisplayLower5       FXParamValueDisplay 4

     Rotary6             NoAction
     RotaryPush6         NoAction     
     DisplayUpper6       NoAction
     DisplayLower6       NoAction

     Rotary7             NoAction 
     RotaryPush7         NoAction
     DisplayUpper7       NoAction 
     DisplayLower7       NoAction 

     Rotary8             FXParam 8
     RotaryPush8         ToggleFXBypass
     DisplayUpper8       FixedTextDisplay "Wet"
     DisplayLower8       FXBypassedDisplay
ZoneEnd
```

## FixedTextDisplay
Use the FixedTextDisplay action when you want to show static text within one of your displays. For instance, in this example, holding Shift and pressing RotaryPushA1 will reset (center) FXParam 1 on a particular fx.zon. But I want to leave myself a reminder of this functionality, so I added a message that will say "Press to Reset" on the lower display for that widget as soon as I engage the Shift modifier.
```
     Shift+RotaryPushA1        FXParam 1 [ 0.5 ]
     Shift+DisplayLowerA1      FixedTextDisplay "Press to Reset"
```


## FixedRGBColorDisplay
Use this if you want to have a supported display widget [Select buttons on FaderPort8/16] change color in a particular zone. The values in the squiggly brackets represent the RGB (red, green, blue) color values. 

In the below example, FixedRGBColorDisplay is being used to set the colors of certain buttons when entering this custom "NavigatorPan" SubZone. In this case, when the zone is active, the Pan button will turn blue { 0 55 255 }, and the other navigation buttons on this surface (Channel, Scroll) will go dark { 0 5 20 }.
```
Zone "NavigatorPan"
  Pan                   FixedRGBColorDisplay { 0 55 255 }

  Channel               FixedRGBColorDisplay { 0 5 20 }
  Channel               GoHome
  Channel               GoSubZone "NavigatorChannel"

  Scroll                FixedRGBColorDisplay { 0 5 20 }
  Scroll                GoHome
  Scroll                GoSubZone "NavigatorScroll"

  Shift+Scroll          FixedRGBColorDisplay { 0 5 20 }
  Shift+Scroll          GoHome
  Shift+Scroll          GoSubZone "NavigatorZoom"

  RotaryBigPush         Reaper _XENAKIOS_PANTRACKSCENTER // Xenakios/SWS: Pan selected tracks to center
  RotaryBig             TrackPan
ZoneEnd
```

## ClearAllSolo
ClearAllSolo is a CSI action designed to clear all solo'd tracks and also work with the global MCU 'Solo' widget to provide a visual indication that one or more tracks in your project may be in a solo'd state. 

Example: you have an MCU surface, and Reaper tracks 1-8 are currently mapped to the surface's 8 faders, but you've got track 32 solo'd. When assigned to a widget capable of displaying two-state feedback, your surface will display a light indicating there is a solo'd track anywhere in your project. Pressing the button assigned to this action, will then clear any and all solo'd tracks.

````
Zone "Buttons|"
        Solo            ClearAllSolo
ZoneEnd
````

## NoFeedback
The NoFeedback action is meant to turn off feedback to a specific widget. This is especially useful if you have a control surface that is designed to show feedback on buttons, but those buttons are assigned to Reaper actions where feedback doesn't make sense because there is no on or off state. 

In the below example, the Marker button is being used to insert a Marker at the current or edit position. On some control surfaces, this may cause the marker button to light up, which doesn't make sense. So to solve for this in our zone file, we need to use the NoFeedback action as shown below:

```
Zone "Buttons"
     Marker                   Reaper 40171     // Insert marker at current or edit position
     Property+Marker          NoFeedback       // Turns off feedback
ZoneEnd    
```

Here you see that the [[Property|Modifiers#Property]] modifier is combined with the widget name from the row **immediately above** followed by the NoFeedback action. If the **Property+** line doesn't immediately follow the widget+action row it's meant to apply to, this will not work.

This very same approach also works with modifiers as shown below:

```
Zone "Buttons"
     Marker                     Reaper 40171     // Insert marker at current or edit position
     Property+Marker            NoFeedback       // Turns off feedback
     Option+Marker              Reaper 40173     // Go to next marker or project end
     Property+Option+Marker     NoFeedback       // Turns off feedback	
     Shift+Marker               Reaper 40172     // Go to previous marker or project start
     Property+Shift+Marker      NoFeedback       // Turns off feedback
ZoneEnd
```

## Broadcast, Receive
See [[Broadcast and Receive]] for details on how to use these actions.

## WidgetMode, SetWidgetMode
See [[WidgetMode]] for more details on this unique set of actions.

## SendMIDIMessage
SendMIDIMessage allows you to send arbitrary MIDI message to any CSI device based on whatever conditions you'd like to setup. This is great for devices like the MIDIFighterTwister, the Launch Pads, and other MIDI surfaces that will change colors or functionality based on MIDI messages they receive. For example, I'm doing this in my Home.zon to turn on strobing and change colors of buttons on my MIDI Fighter Twister based on the playback and record states in Reaper.
```
Zone "Home"
OnRecordStart SendMIDIMessage "B1 0F 50"     // Makes button B8 red on record start
OnRecordStart SendMIDIMessage "B5 0F 04"     // Makes button B8 strobe on record start
OnRecordStop  SendMIDIMessage "B1 0F 5F"     // Makes button B8 pink on record stop
OnRecordStop  SendMIDIMessage "B5 0F 00"     // Makes button B8 stop strobing on record stop
OnPlayStart   SendMIDIMessage "B1 0E 2D"     // Makes button B8 green on play start
OnPlayStart   SendMIDIMessage "B5 0E 04"     // Makes button B8 strobe on play start
OnPlayStop    SendMIDIMessage "B1 0E 5F"     // Makes button B8 pink on play stop
OnPlayStop    SendMIDIMessage "B5 0E 00"     // Makes button B8 stop strobing on play start
     IncludedZones
          "SelectedTrack"r
          "Buttons"
          "SelectedTrackFXMenu"
          "SelectedTrackSend"
          "SelectedTrackReceive"
     IncludedZonesEnd
ZoneEnd
```

You can, of course, assign this to a button.
```
Zone "Buttons"
    SommeButton     SendMIDIMessage "B5 0E 04"     // Makes button B8 strobe on play start
ZoneEnd
```

## SetXTouchDisplayColors, RestoreXTouchDisplayColors
SetXTouchDisplayColors and RestoreXTouchDisplayColors are highly specialized actions for the X-Touch Universal and X-Touch Extenders to set the colors of all displays at once. When combined with the new OnZoneActivation, OnZoneDeactivation virtual widgets, these allow you to set all of the surface displays to the same color when you enter a SelectedTrackFXMenu zone and restore the prior colors when you exit that Zone...
```
Zone "SelectedTrackFXMenu"
	OnZoneActivation	SetXTouchDisplayColors Yellow
	OnZoneDeactivation	RestoreXTouchDisplayColors
...
ZoneEnd
```

On the X-Touch, you can also set all 8 colors to any arbitrary value you'd like, but it MUST be all 8 colors. You include the color name for each of the 8 channels in a string with quotes. The syntax for that is shown below:
```
      OnZoneActivation     SetXTouchDisplayColors "Red Red Magenta Blue Yellow Green Cyan Red"
```

**Important Note:** The X-Touch firmware only supports 8 track colors. These are not full RGB screens. CSI will translate the colors in Reaper to the nearest approximation of the colors supported on the X-Touch, which are...
```
Black
White
Red
Green
Blue
Cyan
Magenta
Yellow
```

## Speak
[OSARA](https://osara.reaperaccessibility.com/) is described as "a Reaper extension that aims to make Reaper accessible to screen reader users." CSI has added preliminary support for OSARA with the goal of improving CSI with these screen readers. A new "Speak" action was added that can be triggered in various scenarios. See the example below which would speak the phrase "UAD Fairchild 660 Compressor" when the FX.zon was activated.
```
Zone "VST: UAD Fairchild 660 (Universal Audio, Inc.)" "Fair660"
	OnZoneActivation	Speak "UAD Fairchild 660 Compressor"

	DisplayUpper1		FixedTextDisplay "HdRoom"
 	DisplayLower1		FXParamValueDisplay 9
	Rotary1			FXParam 9 [ 0.0 0.17 0.33 0.50 0.67 0.83 1.0 ]
   ...
ZoneEnd
``` 