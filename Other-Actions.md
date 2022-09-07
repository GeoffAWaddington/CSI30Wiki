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
* [[RestoreXTouchDisplayColors|Other Actions#setxtouchdisplaycolors-restorextouchdisplaycolors]]
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

## ToggleChannel
ToggleChannel allows you to define a widget, such as RotaryPush, to toggle functionality assigned to that action. Example: this allows you to toggle between TrackPanAutoLeft + TrackPanAutoLeftDisplay and TrackPanAutoRight + TrackPanAutoRightDisplay on the same channel on the same zone. To do this, first you would define "RotaryPush|" to the ToggleChannel action. Next, you would use Toggle+ as a modifier. 
```
Zone "Track"
    RotaryPush|                 ToggleChannel

    Rotary|                     TrackPanAutoLeft
    Rotary|			WidgetMode Dot
    Toggle+Rotary|              TrackPanAutoRight
    Toggle+Rotary|		WidgetMode Dot

    DisplayLower|      		TrackPanAutoLeftDisplay
    Toggle+DisplayLower|   	TrackPanAutoRightDisplay
ZoneEnd
```
This functionality can also be used in FX.zon's to group like-parameters (example: you can flip between the Frequency and Q control on an EQ plugin).

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

**Tip:** Another use-case is creating a "Help" SubZone that can be called to identify what each button and control does on a surface. To do this, first you would add a SubZone called "Help" in your BUttons.zon with an action to call it up as needed (in this case, the smpteBeats button). Thanks to Equitone for the use-case and the examples below!
```
Zone "Buttons"
    SubZones
           "Help"
    SubZonesEnd

    smpteBeats                  GoSubZone "Help"
```

Then you'd have the below Help.zon file (assuming you're using an MCU or X-Touch Universal surface) where each control has its function spoken to the user. As with all things CSI, you may need to customize or otherwise modify this zone to fit your particular mapping.
```
Zone "Help"
    OnZoneActivation                Speak "XTouch help : ON"
    smpteBeats                      Speak "XTouch Help : OFF"
    smpteBeats                      LeaveSubZone

    // Modifiers declaration
    Flip           Speak "Flip, volume on rotaries, pan on faders"
    Flip           Flip

    // Left panel
    RotaryPush1        Speak "RotaryPush1, Set track 1 center pan"
    RotaryPush2        Speak "RotaryPush2, Set track 2 center pan"
    RotaryPush3        Speak "RotaryPush3, Set track 3 center pan"
    RotaryPush4        Speak "RotaryPush4, Set track 4 center pan"
    RotaryPush5        Speak "RotaryPush5, Set track 5 center pan"
    RotaryPush6        Speak "RotaryPush6, Set track 6 center pan"
    RotaryPush7        Speak "RotaryPush7, Set track 7 center pan"
    RotaryPush8        Speak "RotaryPush8, Set track 8 center pan"
    Rotary1            Speak "Rotary1, Track 1 pan value"
    Rotary2            Speak "Rotary2, Track 2 pan value"
    Rotary3            Speak "Rotary3, Track 3 pan value"
    Rotary4            Speak "Rotary4, Track 4 pan value"
    Rotary5            Speak "Rotary5, Track 5 pan value"
    Rotary6            Speak "Rotary6, Track 6 pan value"
    Rotary7            Speak "Rotary7, Track 7 pan value"
    Rotary8            Speak "Rotary8, Track 8 pan value"
    Flip+Rotary1       Speak "Flip+Rotary1, Track 1 volume" 
    Flip+Rotary2       Speak "Flip+Rotary2, Track 2 volume" 
    Flip+Rotary3       Speak "Flip+Rotary3, Track 3 volume" 
    Flip+Rotary4       Speak "Flip+Rotary4, Track 4 volume" 
    Flip+Rotary5       Speak "Flip+Rotary5, Track 5 volume" 
    Flip+Rotary6       Speak "Flip+Rotary6, Track 6 volume" 
    Flip+Rotary7       Speak "Flip+Rotary7, Track 7 volume" 
    Flip+Rotary8       Speak "Flip+Rotary8, Track 8 volume" 
    RecordArm1         Speak "RecordArm1, Track 1 record arm"
    RecordArm2         Speak "RecordArm2, Track 2 record arm"
    RecordArm3         Speak "RecordArm3, Track 3 record arm"
    RecordArm4         Speak "RecordArm4, Track 4 record arm"
    RecordArm5         Speak "RecordArm5, Track 5 record arm"
    RecordArm6         Speak "RecordArm6, Track 6 record arm"
    RecordArm7         Speak "RecordArm7, Track 7 record arm"
    RecordArm8         Speak "RecordArm8, Track 8 record arm"
    Solo1              Speak "Solo1, Track 1 solo"
    Solo2              Speak "Solo2, Track 2 solo"
    Solo3              Speak "Solo3, Track 3 solo"
    Solo4              Speak "Solo4, Track 4 solo"
    Solo5              Speak "Solo5, Track 5 solo"
    Solo6              Speak "Solo6, Track 6 solo"
    Solo7              Speak "Solo7, Track 7 solo"
    Solo8              Speak "Solo8, Track 8 solo"
    Mute1              Speak "Mute1, Track 1 mute"
    Mute2              Speak "Mute2, Track 2 mute"
    Mute3              Speak "Mute3, Track 3 mute"
    Mute4              Speak "Mute4, Track 4 mute"
    Mute5              Speak "Mute5, Track 5 mute"
    Mute6              Speak "Mute6, Track 6 mute"
    Mute7              Speak "Mute7, Track 7 mute"
    Mute8              Speak "Mute8, Track 8 mute"
    Select1            Speak "Select1, Track 1 select"
    Select2            Speak "Select2, Track 2 select"
    Select3            Speak "Select3, Track 3 select"
    Select4            Speak "Select4, Track 4 select"
    Select5            Speak "Select5, Track 5 select"
    Select6            Speak "Select6, Track 6 select"
    Select7            Speak "Select7, Track 7 select"
    Select8            Speak "Select8, Track 8 select"
    Hold+Select1       Speak "Hold+Select1, Track 1 volume to 0 DB"  
    Hold+Select2       Speak "Hold+Select2, Track 2 volume to 0 DB"
    Hold+Select3       Speak "Hold+Select3, Track 3 volume to 0 DB"
    Hold+Select4       Speak "Hold+Select4, Track 4 volume to 0 DB"
    Hold+Select5       Speak "Hold+Select5, Track 5 volume to 0 DB"
    Hold+Select6       Speak "Hold+Select6, Track 6 volume to 0 DB"
    Hold+Select7       Speak "Hold+Select7, Track 7 volume to 0 DB"
    Hold+Select8       Speak "Hold+Select8, Track 8 volume to 0 DB"
    nameValue          Speak "nameValue (No action)"
    GlobalView         Speak "GlobalView (No action)"
    Fader1             Speak "Fader1, Track 1 volume"
    Fader2             Speak "Fader2, Track 2 volume"
    Fader3             Speak "Fader3, Track 3 volume"
    Fader4             Speak "Fader4, Track 4 volume"
    Fader5             Speak "Fader5, Track 5 volume"
    Fader6             Speak "Fader6, Track 6 volume"
    Fader7             Speak "Fader7, Track 7 volume"
    Fader8             Speak "Fader8, Track 8 volume"
    Flip+Fader1        Speak "Flip+Fader1, Track 1 pan value"
    Flip+Fader2        Speak "Flip+Fader2, Track 2 pan value"
    Flip+Fader3        Speak "Flip+Fader3, Track 3 pan value"
    Flip+Fader4        Speak "Flip+Fader4, Track 4 pan value"
    Flip+Fader5        Speak "Flip+Fader5, Track 5 pan value"
    Flip+Fader6        Speak "Flip+Fader6, Track 6 pan value"
    Flip+Fader7        Speak "Flip+Fader7, Track 7 pan value"
    Flip+Fader8        Speak "Flip+Fader8, Track 8 pan value"
    MasterFader        Speak "MasterFader, Master track volume"

    // Right panel
    Track                       Speak "Track, OSARA: Toggle Report changes made via control surfaces"
    Pan                         Speak "Pan, OSARA: Toggle Report track numbers"
    EQ                          Speak "EQ, OSARA: Toggle Report transport state (play, record, etc.)"
    Send                        Speak "Send, OSARA: Toggle Report markers during playback"
    Plugin                      Speak "Plugin, OSARA: Toggle Report time movement during playback/recording"
    Instrument                  Speak "Instrument, OSARA: Toggle Report position when scrubbing"
    MidiTracks                  Speak "MidiTracks, No action"
    Inputs                      Speak "Inputs, No action"
    AudioTracks                 Speak "AudioTracks, No action"
    AudioInstrument             Speak "AudioInstrument, No action"
    Aux                         Speak "Aux, No action"
    Busses                      Speak "Busses, No action"
    Outputs                     Speak "Outputs, No action"
    User                        Speak "User, No action"
    F1                          Speak "F1, Toggle selected track send and tracks (Osara don't works in track send zone)"
    F2                          Speak "F2, Toggle selected track receive and tracks (Osara don't works in track receive zone)"
    F3                          Speak "F3, All track visible"
    F4                          Speak "F4, Track folders only"
    F5                          Speak "F5, No action"
    F6                          Speak "F6, No action"
    F7                          Speak "F7, No action"
    F8                          Speak "F8, No action"
    Shift                       Speak "Shift, Shift modifier"
    Shift                       Shift
    Option                      Speak "Option, Option modifier"
    Option                      Option
    Control                     Speak "Control, Control modifier"
    Control                     Control
    Alt                         Speak "Alt, Alt modifier"    
    Alt                         Alt
    Read                        Speak "Read, Read mode automation"
    Write                       Speak "Write, Write mode automation"
    Trim                        Speak "Trim, Trim mode automation"
    Touch                       Speak "Touch, Touch mode automation"
    Latch                       Speak "Latch, Latch mode automation"
    Group                       Speak "Group, Group mode automation"
    Shift+Read                  Speak "Shift+Read, Global Read mode automation"      
    Shift+Write                 Speak "Shift+Write, Global Write mode automation"      
    Shift+Trim                  Speak "Shift+Trim, Global Trim mode automation"      
    Shift+Touch                 Speak "Shift+Touch, Global Touch mode automation"      
    Shift+Latch                 Speak "Shift+Latch, Global Latch mode automation"      
    Shift+Group                 Speak "Shift+Group, Global Group mode automation"      
    Save                        Speak "Save, NoAction "
    Undo                        Speak "Undo, NoAction"
    Cancel                      Speak "Cancel, NoAction""
    Enter                       Speak "Enter, NoAction""
    Marker                      Speak "Marker, Select master track"
    Hold+Marker                 Speak "Hold+Marker, Master track to 0 db"
    Nudge                       Speak "Nudge, NoAction""
    Cycle                       Speak "Cycle, NoAction""
    Drop                        Speak "Drop, NoAction""
    Replace                     Speak "Replace, NoAction""
    Click                       Speak "Click, Toggle click"
    Solo                        Speak "Solo, NoAction""
    Rewind                      Speak "Rewind, Go to previous bar"
    Hold+Rewind                 Speak "Hold+Rewind, Go to start of project"
    FastForward                 Speak "FastForward, Go to next bar"
    Hold+FastForward            Speak "Hold+Rewind, Go to end of project"
    Stop                        Speak "Stop, Stop playing or recording"
    Play                        Speak "Play, Play/Pause"
    Record                      Speak "Record, Start recording"
    BankLeft                    Speak "BankLeft, Shift faders 8 tracks to left"
    BankRight                   Speak "BankRight, Shift faders 8 tracks to right"
    ChannelLeft                 Speak "ChannelLeft, Shift faders 1 tracks to left"
    ChannelRight                Speak "ChannelRight, Shift faders 1 tracks to right"    
    Up                          Speak "Up, Go to previous track"    
    Down                        Speak "Down, Go to next track"    
    Left                        Speak "Left, Select and go to previous item"    
    Right                       Speak "Right, Select and go to next item"    
    Zoom                        Speak "Zoom, Toggle Track mode and FX mode"    
    JogWheelRotaryCW            Speak "JogWheelRotaryCW, Move cursor 1 pixel to left"    
    JogWheelRotaryCCW           Speak "JogWheelRotaryCCW, Move cursor 1 pixel to right"     
    Scrub                       Speak "Scrub, Toggle loop segment scrub at edit cursor"     
    Footswitch1                 Speak "Play/Pause"     
    Footswitch2                 Speak "Start recording"     
ZoneEnd
```