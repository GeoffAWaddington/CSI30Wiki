# September 6, 2022 - EXP Builds
This is what is currently floating around in the CSI Exp builds as of September 5th, 2022. 

## Two-Way Encoder Behavior (Requires Updates to Your JogWheel in the .mst File)
Now you can assign two different Reaper actions to a single encoder based on which way the encoder is being turned, Counter-Clockwise (CCW) or Clockwise (CW). We do this via the Decrease and Increase modifiers. Note: these modifiers only work with Encoders. The examples below all show the JogWheel but this functionality will work with any encoder.

```
Zone "Zoom"
    Decrease+JogWheel      Reaper 41326   // Decrease track height
    Increase+JogWheel      Reaper 41325   // Increase track height
ZoneEnd
```

**Important Note:** for this to work on your JogWheel, you will need to update your .mst file. If you're coming from a prior version of CSI, you probably have two or more separate Jogwheel widgets, and those widgets probably are defined using Press instead of Encoder. So replace your JogWheel widgets to look like this (assuming MCU-style surface):
```
Widget JogWheel 
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

## EZFXZones: New FX Zone Syntax Option
**Note: consider this feature extra-experimental. This is not ready for production projects yet. Also, some actions described below are currently still being coded.**

EZFXZones are a new way of writing FX Zones that uses far fewer lines, and saves a lot of the tedious repetition of the legacy fx.zon format (which is not going away). These new FX zones follow a spreadhseet-like format where you read both across and down.

Diving right into a real-world example, the first and last lines of any fx.zon remain unchanged. After that, the first block of text starting with "FXParams" is saying FXParam 9, which, reading down, we are giving the alias "HeadRoom", gets assigned to Rotary1. You will see the FXParamName on DisplayUpper1, and you will see the FXParamValue on DisplayLower1. Reading across on line 2 again, FXParam 1, "Input", will get assigned to Rotary2 (we're reading down again), with the FXParamName name showing up on DisplayUpper2 and FXParamValueName on DisplayLower2. And on and on.
```
Zone "VST: UAD Fairchild 660 (Universal Audio, Inc.)" "Fair660"
     FXParams                 9      1     2      3     6    7      0     8
     FXParamNames             HdRoom Input Thresh Ratio Knee Output Meter WetDry
     FXValueWidgets           Rotary|
     FXParamNameDisplays      DisplayUpper|
     FXParamValueDisplays     DisplayLower|
ZoneEnd
```
**Note:** using the legacy FX.zon syntax, this simple mapping would've required something like 40 lines of code. The new EZFXZone syntax accomplishes all the same functionality in only 5 lines! If you wanted, you could stop right there; that could be an entire fx.zon!

Now, if we want to add a modifier, we add the next block of text shown below. Very similar layout, except there is one more row, with the addition of the **FXWidgetModifiers Shift** line. This says, show FXParam 5, "DC Bal", on Shift+Rotary1 and the corresponding Shift-Displays. Also note that "DC Bal" is in quotes - these are only required because there is a space in what we want to appear on the display. Next, if you continue reading across, you see a series of -1 entries in the FXParams row and double quotes in the corresponding FXParamNames rows. This tells CSI, "No Action here and clear out the displays".
```
Zone "VST: UAD Fairchild 660 (Universal Audio, Inc.)" "Fair660"
     FXParams                 9      1     2      3     6    7      0     8
     FXParamNames             HdRoom Input Thresh Ratio Knee Output Meter WetDry
     FXValueWidgets           Rotary|
     FXParamNameDisplays      DisplayUpper|
     FXParamValueDisplays     DisplayLower|

     FXParams		      5	 -1 -1 -1 -1 -1 -1 4
     FXParamNames	      "DC Bal" "" "" "" "" "" "" SidChn
     FXValueWidgets  	      Rotary|
     FXParamNameDisplays      DisplayUpper|
     FXParamValueDisplays     DisplayLower|
     FXWidgetModifiers	      Shift
ZoneEnd
```

Now, let's say I want RotaryPush8 to toggle the FX Bypass state, but it's the only toggle-style action I need and I don't necessarily need to see that on a display. You can simply add another block with that specific instruction on the one widget:
```
Zone "VST: UAD Fairchild 660 (Universal Audio, Inc.)" "Fair660"
     FXParams                 9      1     2      3     6    7      0     8
     FXParamNames             HdRoom Input Thresh Ratio Knee Output Meter WetDry
     FXValueWidgets           Rotary|
     FXParamNameDisplays      DisplayUpper|
     FXParamValueDisplays     DisplayLower|

     FXParams		      5	 -1 -1 -1 -1 -1 -1 4
     FXParamNames	      "DC Bal" "" "" "" "" "" "" SidChn
     FXValueWidgets  	      Rotary|
     FXParamNameDisplays      DisplayUpper|
     FXParamValueDisplays     DisplayLower|
     FXWidgetModifiers	      Shift

     FXParams                 11
     FXValueWidgets           RotaryPush8
ZoneEnd
```

Now, how do we deal with stepped params, encoder acceleration, step sizes, colors, etc.? We simply add those to a new block of text at the bottom and define them on a per-parameter basis (except DefaultAcceleration, which is not at a per-parameter level). 
```
Zone "VST: UAD Fairchild 660 (Universal Audio, Inc.)" "Fair660"
     FXParams                 9      1     2      3     6    7      0     8
     FXParamNames             HdRoom Input Thresh Ratio Knee Output Meter WetDry
     FXValueWidgets           Rotary|
     FXParamNameDisplays      DisplayUpper|
     FXParamValueDisplays     DisplayLower|

     FXParams		      5	 -1 -1 -1 -1 -1 -1 4
     FXParamNames	      "DC Bal" "" "" "" "" "" "" SidChn
     FXValueWidgets  	      Rotary|
     FXParamNameDisplays      DisplayUpper|
     FXParamValueDisplays     DisplayLower|
     FXWidgetModifiers	      Shift

     FXParams                 11
     FXValueWidgets           RotaryPush8

     Left                     FXParamsBank -1
     Right                    FXParamsBank 1

     DefaultAcceleration      0.001 0.002 0.003 0.004 0.005 0.006 0.0075 0.01 0.02 0.035 0.05   
     FXParamAcceleration 7    0.001 0.002 0.003 0.004 0.005 0.006 0.0075 0.01 0.02 0.025 0.03 
     FXParamStepSize     0    0.5
     FXParamStepValues   1    0.0 0.05 0.11 0.16 0.21 0.26 0.32 0.37 0.42 0.47 0.53 0.58 0.63 0.68 0.74 0.79 0.84 0.89 0.95 1.0
     FXParamStepValues   3    0.0 0.20 0.40 0.60 0.80 1.0 
     FXParamStepValues   9    0.0 0.17 0.33 0.50 0.67 0.83 1.0
     FXParamTickCounts   3    3
     FXWidgetModes       1    BoostCut
     FXWidgetModes       7    BoostCut  
     FXParamColors       11   #ff0000 #00ff007f
ZoneEnd
```
So let's look at the new additions: what happens if you have more than 8 FXParams you want to assign to your 8-channel unit? You can continue adding additional FXParams in lines 2 and 3, then CSI will allow you to bank to the next FXParam via the **FXParamsBank** actions. Banking will also allow for mapping fx on 1-fader surfaces.

**DefaultAcceleration** is optional but allows you to set a custom acceleration curve to all encoders (there are per-parameter curves as well). 

The next set of actions are all on per-parameter basis. Notice how the syntax is the action type, the FXParam #, then the instructions with no need for brackets as used in traditional fx.zon files. So you can further still create parameter-level custom acceleration curves using the **FXParamAcceleration** action. **FXParamStepSize** is used to set the step size for an encoder tick on a FX Param. **FXParamStepValues** is used for stepped params. **FXParamTickCounts** sets the number of encoder ticks CSI must receive before advancing the FXParamValue. **FXWidgetModes** allow you to set display [[WidgetModes|WidgetMode]] on a per-parameter baseis. Lastly, **FXParamColors** can be used by supported surfaces to send RGBA colors (in hex format) to a supported widget. **Note:** CSI will be moving away from RGB to Hex everywhere.

At the time of this writing, the following actions have been implemented
```
FXParams
FXParamNames
FXValueWidgets
FXParamNameDisplays
FXParamValueDisplays

FXParamAcceleration
FXParamRange
FXParamStepSize
FXParamStepValues
FXParamTickCounts
FXParamColors (now uses Hex colors)
DefaultAcceleration
```

The following actions are coming soon.
```
FXParamsBank
FXWidgetModes
```

## New Implementation for Track, Folder, VCA Modes With New Actions
There is a new implementation and set of corresponding zones (see the X-Touch folder in the CSI Support Files for examples), of Folder and VCA modes. Track is your basic Track.zon, there's no change there. When you use a GoFolder action to activate Folder mode, only Reaper's Folder tracks become visible, and you can press the Select [or any user-defined] button to drill down into the folder. After drilling down, the parent folder track is the first track on the left, and child tracks are on the right. There's an identical implementation for VCA leaders and followers via the GoVCA navigation action.

The new actions for these modes are:
```
    GoTrack
    GoVCA
    GoFolder
    TrackVCALeaderDisplay
    TrackFolderParentDisplay
    TrackToggleVCASpill
    TrackToggleFolderSpill
```

And here is an example of the new Folder.zon:
```
Zone "Folder"
    DisplayUpper|                  TrackNameDisplay
    Fader|Touch+DisplayLower|      TrackVolumeDisplay
    DisplayLower|                  TrackFolderParentDisplay
    Shift+DisplayLower|            TrackAutoModeDisplay
    Toggle+DisplayLower|           TrackPanAutoRightDisplay
    Alt+DisplayLower|              TrackInputMonitorDisplay
    VUMeter|                       TrackOutputMeterMaxPeakLR
    Fader|                         TrackVolume 
    Flip+Fader|                    TrackPan 
    Rotary|                        TrackPanAutoLeft
    Rotary|                        WidgetMode Dot
    Toggle+Rotary|                 TrackPanAutoRight
    Toggle+Rotary|                 WidgetMode Dot
    RotaryPush|                    ToggleChannel
    Shift+RotaryPush|              TrackPan [ 0.5 ]
    Option+RotaryPush|             TrackPanWidth [ 1.0 ]
    Alt+RotaryPush|                CycleTrackInputMonitor
    RecordArm|                     TrackRecordArm
    Shift+RecordArm|               CycleTrackAutoMode
    Solo|                          TrackSolo
    Mute|                          TrackMute
    Select|                        TrackToggleFolderSpill
    Shift+Select|                  TrackRangeSelect
    Control+Select|                TrackSelect
                                   
    BankLeft                       FolderBank -8
    BankRight                      FolderBank 8
    ChannelLeft                    FolderBank -1
    ChannelRight                   FolderBank 1

ZoneEnd
```

And here's a sample VCA.zon:
```
Zone "VCA"
    DisplayUpper|               TrackNameDisplay
    Fader|Touch+DisplayLower|   TrackVolumeDisplay
    DisplayLower|               TrackVCALeaderDisplay
    Shift+DisplayLower|         TrackAutoModeDisplay
    Toggle+DisplayLower|        TrackPanAutoRightDisplay
    Alt+DisplayLower|           TrackInputMonitorDisplay
    VUMeter|                    TrackOutputMeterMaxPeakLR
    Fader|                      TrackVolume 
    Flip+Fader|                 TrackPan 
    Rotary|                     TrackPanAutoLeft
    Rotary|                     WidgetMode Dot
    Toggle+Rotary|              TrackPanAutoRight
    Toggle+Rotary|              WidgetMode Dot
    RotaryPush|                 ToggleChannel
    Shift+RotaryPush|           TrackPan [ 0.5 ]
    Option+RotaryPush|          TrackPanWidth [ 1.0 ]
    Alt+RotaryPush|             CycleTrackInputMonitor
    RecordArm|                  TrackRecordArm
    Shift+RecordArm|            CycleTrackAutoMode
    Solo|                       TrackSolo
    Mute|                       TrackMute
    Select|                     TrackToggleVCASpill
    Shift+Select|               TrackRangeSelect
    Control+Select|             TrackSelect

    BankLeft                    VCABank -8
    BankRight                   VCABank 8
    ChannelLeft                 VCABank -1
    ChannelRight                VCABank 1

ZoneEnd
```

## Depreciated CycleTrackVCAFolderModes Action
With the addition of the GoTrack, GoVCA, GoFolder actions, the CycleTrackVCAFolderModes was removed. Please transition to using the new zones and zone activation actions.

## Hex Color Support
CSI has been expanded to support Hex colors on actions. One can currently use RGB (legacy) or Hex (do not mix and match). For instance, if you wanted a particular button on your OSC surface to change colors based on whether or not the new VCA zone was active, you could do that as shown below.
```
     ButtonM31     GoVCA      { #6437017f #FA95017f }
```

## AnyPress Widget Now Available for OSC Surfaces
The [[AnyPress|Message-Generators#anypress]] Message Generator is now available to be used in .ost files for OSC surfaces such as the Behringer X32/Midas series.

# August 31, 2022

## Bug Fixes for Zone On/Off Colors on OSC, Track Selection, ScrollLink
There were some recent fixes to Zone on/off colors on OSC surfaces, as well as fixes to track selection and ScrollLink introduced due to recent changes to Track Visibility.

## Preliminary Test Implentation for OSARA Integration
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

## New Virtual Widgets: OnPlayStart, OnPlayStop, OnRecordStart, OnRecordStop, OnZoneActivation, OnZoneDeactivation
A slew of new virtual widgets have been added to allow you to automatically fire off actions based on Play or Record state, or when Zones are activated or deactivated. This adds a LOT of flexibility to Zones and can create a "smarter" experience.

Here are some example use-cases:
```
Zone "Home"
OnRecordStart SendMIDIMessage "B5 0F 04"     // Makes button B8 strobe on record start
OnRecordStop  SendMIDIMessage "B5 0F 00"     // Makes button B8 stop strobing on record stop
OnPlayStart   SendMIDIMessage "B5 0E 04"     // Makes button B8 strobe on play start
OnPlayStop    SendMIDIMessage "B5 0E 00"     // Makes button B8 stop strobing on play start
...
ZoneEnd
```

```
Zone "SelectedTrackSend"
	OnZoneActivation	    SetXTouchDisplayColors Cyan
	OnZoneDeactivation	    RestoreXTouchDisplayColors
 ...
ZoneEnd
```

```
Zone "SelectedTrackFXMenu"
	OnZoneActivation	SetXTouchDisplayColors Yellow
	OnZoneDeactivation	RestoreXTouchDisplayColors
...
ZoneEnd
```

## New X-Touch Exclusive Actions: SetXTouchDisplayColors, RestoreXTouchDisplayColors
SetXTouchDisplayColors and RestoreXTouchDisplayColors are highly specialized actions for the X-Touch Universal and X-Touch Extenders to set the colors of all displays at once. When combined with the new OnZoneActivation, OnZoneDeactivation virtual widgets, these allow you to set all of the surface displays to the same color when you enter a SelectedTrackFXMenu zone, and restore the prior colors when you exit that Zone...
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

See [[FB_XTouchDisplayUpper|Feedback-Processors#fb_xtouchdisplayupper]] for a list of available X-Touch colors. 

## New Action: SendMIDIMessage
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

## CSI No Longer Saves the Bank Location in Your Reaper Project
Due to changes in track visibility, Reaper state, etc., saving the banking information in the Reaper .rpp could lead to issues and was eliminated.

## Improvements to Track Visibility
To improve Track Visibility and update behavior, CSI now recalculates the Track list on every Run, about 33 times a second -- don't worry, the recalculation takes microseconds.

## Follow TCP Functionality
By default CSI will follow Reaper's MCP view. ou can override this behavior if you add FollowTCP to the line in the csi.ini with the page name as shown below.
```
Version 2.0

MidiSurface "XTouchOne" 7 9
MidiSurface "MFTwister" 6 8 
OSCSurface "iPad Pro" 8003 9003 10.0.0.146 

Page "HomePage" FollowTCP
"XTouchOne" 1 0 "X-Touch_One.mst" "X-Touch_One_SelectedTrack"
"MFTwister" 8 0 "MIDIFighterTwisterEncoder.mst" "FXTwisterMenu"
"iPad Pro" 8 0 "FXTwister.ost" "FXTwisterMenu"

Page "MixPage" 
"XTouchOne" 1 0 "X-Touch_One.mst" "X-Touch_One_SelectedTrack"
"MFTwister" 8 0 "MIDIFighterTwisterEncoder.mst" "FXTwisterFocusedFX"
"iPad Pro" 8 0 "FXTwister.ost" "FXTwisterFocusedFX"
```

## New Action: GlobalAutoModeDisplay
If you wanted to dedicate a display to showing the global auomation mode within Reaper (example: on an OSC device), there is now a CSI action that will display that.
```
Zone "Buttons"
     AutoModeDisplay     GlobalAutoModeDisplay
ZoneEnd
```

## TrackVCAFolderModeDisplay, ToggleFXOffline, and ToggleFXBypass Now Display String and Integer (Based on Feedback Processor Type)
Depending on what type of Feedback Processor these actions are assigned to, CSI will either return the surface an integer (e.g. 0 for off, 1 for on) or return a string value (e.g. 'Offline'). Example: on an OSC surface, you may want ToggleFXBypass to return "Bypased/Active" if using the FB_Processor, or 0/1 if using the FB_IntProcessor.

## Bug-Fix: ToggleFXBypass Now Respects FX Chain Bypass Status
ToggleFXBypass would return the incorrect status if the plugin was enabled by the entire FX Chain was bypassed. This has been corrected.

## New Action: OSCTimeDisplay
Use OSCTimeDisplay for displaying Reaper's time, including the various modes, on an OSC surface. This is basically the OSC equivalent of MCUTimeDisplay. Use the same, pre-existing, CycleTimeDisplayModes action to change modes.
```
Zone "Buttons"
    TimeDisplay                 OSCTimeDisplay
    SomeButton                  CycleTimeDisplayModes    
ZoneEnd
```

## New Action: ToggleFXOffline
Use ToggleFXOffline to change the FX status to "offline" in Reaper. Offline FX is similar to Bypass, but it removes the plugin from memory and additional processing. In the below example, it's assigned to Shift+Mute. **Note:** This one CSI action is both an action AND a display. Notice the Shift+DisplayLower| line!
```
Zone "SelectedTrackFXMenu"
        DisplayUpper|         FXMenuNameDisplay
        DisplayLower|         FXBypassDisplay
        Shift+DisplayLower|   ToggleFXOffline
        Rotary|               NoAction
        RotaryPush|           GoFXSlot
        Mute|                 ToggleFXBypass
        Shift+Mute|           ToggleFXOffline
        Left                  SelectedTrackFXMenuBank -1
        Right                 SelectedTrackFXMenuBank 1

        RecordArm|            NoAction
        Solo|                 NoAction
        Select|               NoAction
     Fader|                   NoAction    
ZoneEnd
```

## Additions to Broadcast/Receive Functionality
[[Broadcast and Receive|Broadcast-and-Receive]] has been expanded with some additional functioanlity:

* **ToggleEnableFocusedFXMapping** is automatically included if **ToggleEnableFocusedFXMapping** is in Broadcast/Receive list
* **SelectedTrackSendBank** is automatically included if **SelectedTrackSend** is in Broadcast/Receive list
* **SelectedTrackReceiveBank** is automatically included if **SelectedTrackReceive** is in Broadcast/Receive list
* **SelectedTrackFXMenuBank** is automatically included if **SelectedTrackFX** is in Broadcast/Receive list

## New Actions: CycleTrackInputMonitor, TrackInputMonitorDisplay
Use CyleTrackInputMonitor to cycle through the input monitoring modes in a Track or SelectedTrack zone. The corresponding display action in TrackInputMonitorDisplay.
```
Zone "Track"
    Alt+RotaryPush|        	CycleTrackInputMonitor
    Alt+DisplayLower|   	TrackInputMonitorDisplay
ZoneEnd
```

## Subzones Can Now Contain | Character
In prior CSI builds, a SubZone called from a Track context with a | character could not inherit the full track context with all the channels (example: channels 1-8). Now they can. For instance, here's a basic use-case using a DualPan SubZone that wasn't possible prior to this build.
```
Zone "Track"
    SubZones
        "DualPan"
    SubZonesEnd
     RotaryPush|                        ToggleChannel

     DisplayLower|                      TrackPanDisplay
     Rotary|                            TrackPan
     Rotary|                            WidgetMode Dot
     
     Toggle+Rotary|                     TrackPanWidth
     Toggle+Rotary|                     WidgetMode BoostCut
     Toggle+DisplayLower|               TrackPanWidthDisplay
     
     Shift+RotaryPush|                  GoSubZone "DualPan"
ZoneEnd
```

And the DualPan zone itself...
```
Zone "DualPan"
     RotaryPush|                        ToggleChannel
     
     DisplayLower|                      TrackPanLDisplay
     Rotary|                            TrackPanL
     Rotary|                            WidgetMode Dot
     
     Toggle+DisplayLower|               TrackPanRDisplay
     Toggle+Rotary|                     TrackPanR
     Toggle+Rotary|                     WidgetMode Dot

     Shift+RotaryPush|                  LeaveSubZone
ZoneEnd
```

## New Action: ToggleChannel
ToggleChannel allows you to define a widget, such as RotaryPush, to toggle functionality assigned to that action. Example: this allows you to toggle between TrackPan + TrackPanDisplay and TrackPanWidth + TrackPanWidth display on the same channel. To do this, first you would define "RotaryPush|" to the ToggleChannel action. Next, you would use Toggle+as a modifier. 
```
    RotaryPush|                 ToggleChannel

    Rotary|                     TrackPan
    Rotary|			WidgetMode Dot
    DisplayLower|      		TrackPanDisplay

    Toggle+Rotary|              TrackPanWidth
    Toggle+Rotary|		WidgetMode BoostCut
    Toggle+DisplayLower| 	TrackPanWidthDisplay
```

You can now also modify your .mst files and remove the Toggle part of a rotary definition. So this...
```
Widget Rotary1
	Encoder b0 10 7f [ > 01-0f < 41-4f ]
	FB_Encoder b0 10 7f
        Toggle 90 20 7f
WidgetEnd
```

Becomes...
```
Widget Rotary1
	Encoder b0 10 7f [ > 01-0f < 41-4f ]
	FB_Encoder b0 10 7f
WidgetEnd
```

## New Actions: TrackPanAutoLeft, TrackPanAutoRight, TrackPanAutoLeftDisplay, TrackPanAutoRightDisplay
TrackPanAutoLeft will control TrackPan or TrackPanL (if using Dual Pans). TrackPanAutoRight will control TrackPanWidth or TrackPanR (if using Dual Pans). This adds considerable convenience in that you can use Stereo Balance Pans or Dual Pans even in the same Reaper project and control them in CSI without having to change zones. The one difference is that the WidgetMode for TrackPanAutoRight must be fixed (i.e. you can't use the Spread mode for PanWidth and Dot mode for PanR - you have to pick one).
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

**Note:** When using Dual Pans, TrackL and TrackR automation does not get written from a control surface. This appears to require a change to the Reaper API's. 

## New Action: WidgetMode
[[WidgetMode]] is designed to send additional, specific, instructions to a given widget. For instance, on a typical MCU-style device, you can set the Rotary encoder feedback to vary between Dot, BoostCut, Fill, and Spread modes.
```
    Rotary|                     TrackPan
    Rotary|			WidgetMode Dot
    DisplayLower|      		TrackPanDisplay

    Toggle+Rotary|              TrackPanWidth
    Toggle+Rotary|		WidgetMode BoostCut
    Toggle+DisplayLower| 	TrackPanWidthDisplay
```

## New Action: SetWidgetMode
SetWidgetMode exists because you may want to set a Faderport display ScribbleStripMode, therefore, you will need SetWidgetMode since there is no other Action that updates the Widget, as there would be with, say, Rotary values and LED ring style.
```
    FPDisplay   SetWidgetMode
    FPDisplay   WidgetMode SomeWidgetMode
```

## Depreciated Actions: MCUTrackPan, ToggleMCUTrackPanWidth, MCUTrackPanDisplay
The MCUTrackPan actions have been removed in favor of the new, more flexible, "ToggleChannel" functionality.

## New Message Generator: Encoder7Bit
[[Encoder7Bit|Message-Generators#Encoder7Bit]] was created to address 7-Bit absolute encoders that continue to transmit 00 values when turned counter-clockwise after the minimum has been reached, and send 7f values when turned clockwise even after the maximum value has been reached. The X-Touch Compact and X-Touch Mini encoders can be configured to behave this way.

# New FaderPortDisplay functionality (FB_FP8ScribbleStripMode Feedback Processors, new widget modes)
CSI supports all the display functionality of the Presonus FaderPort8 and FaderPort16. The FaderPorts have multiple (9) display types to choose from, contains a ValueBar and can, depending on the display type, show the VU Meter. 

### Display Type
Display type is a per display setting and consists of a widget and an action to set the actual display type. In your .mst file this will look like:
```
// ===========================================
// SCRIBBLE STRIP MODE
// ===========================================
Widget ScribbleStripMode1
	FB_FP8ScribbleStripMode 0
WidgetEnd

Widget ScribbleStripMode2
	FB_FP8ScribbleStripMode 1
WidgetEnd
etc.....
```
The image below shows all the display types and the Id

![FaderPortDisplayTypes](https://user-images.githubusercontent.com/52307138/185772386-6217f370-9564-43b4-8ca3-79463b374c9d.jpg)

The default scribble strip mode is id **2** This is a version with 4 lines and in most cases the most versatile one.

For implementing the scribble strip mode you need to add 2 lines of code. The first one is adding the scribblescript widget to the file. It's value should be `SetWidgetMode`. This tells CSI the only value to use will be the WidgetMode. The second line sets the WidgetMode value. In this case that is the Scribble strip mode. The value is the ID of one of the layouts shown above.

In your zone file this will look like this:
```
Zone "Track"
  ScribbleStripMode|        SetWidgetMode
  ScribbleStripMode|        WidgetMode 8
```

### Scribble lines

Each of the 4 scribble lines requires it’s own widget in the .mst file.
For your .mst, here are the names for the FB generators that correspond to each line on the surface.

|  | **FaderPort 8** | **FaderPort 16** |
| --- | --- | --- |
| Line 1 | FB_FP8ScribbleLine1 | FB_FP16ScribbleLine1 |
| Line 2 | FB_FP8ScribbleLine2 | FB_FP16ScribbleLine2 |
| Line 3 | FB_FP8ScribbleLine3 | FB_FP16ScribbleLine3 |
| Line 4 | FB_FP8ScribbleLine4 | FB_FP16ScribbleLine4 |

And here is an example of how the `.mst` would be mapped out for Display 1 on the Faderport8.
```
// ===========================================
// SCRIBBLE LINES CHANNEL 1
// ===========================================
Widget ScribbleLine1_1
	FB_FP8ScribbleLine1 0
WidgetEnd

Widget ScribbleLine2_1
	FB_FP8ScribbleLine2 0
WidgetEnd

Widget ScribbleLine3_1
	FB_FP8ScribbleLine3 0
WidgetEnd

Widget ScribbleLine4_1
	FB_FP8ScribbleLine4 0
WidgetEnd

```

Per scribble line you're able to set ythe text align and you can set a line to inverted (changing back and foreground colour). These values are set with a WidgetMode.
* **TextAlign**: Possible values are **Center**, **Left** and **Right**. The default when not set is **Center**
* **Invert**: Pass the value **Invert** to invert. It will be normal when not passed.

Using this in a zone file will look like this:
```
Zone "Track"
  ScribbleLine1_|      TrackNameDisplay
  ScribbleLine1_|      WidgetMode "Left"

  ScribbleLine2_|      TrackNameDisplay
  ScribbleLine2_|      WidgetMode "Center Invert"
ZoneEnd
```
_In this example line 1 in the scribble text will be left aligned. Line 2 will be right aligned and inverted._

### Valuebar
The FaderPort 8 and FaderPort 16 have a Valuebar available in the scribble display. The ValueBar can be used for a visual representation of pan value, volume, pan width, etc. The valuebar has 5 different modes.

![FaderPortValueBar](https://user-images.githubusercontent.com/52307138/185798368-b404f2a3-945f-4c06-88e5-3088c663faed.jpg)

In the mst file it looks like:
```
// ===========================================
// VALUE BAR
// ===========================================
Widget ValueBar1
	FB_FPValueBar 0
WidgetEnd

Widget ValueBar2
	FB_FPValueBar 1
WidgetEnd
```
Setting the mode is the zone file is handled by a property. For this reason it is not possible changing this with a modifier key. In the zone file it looks like:
```
Zone "Track"
  ValueBar|     TrackPan
  ValueBar|     WidgetMode BiPolar
```

# August 15, 2022 Update
Reaper forum user [Navelpluisje](https://forum.cockos.com/member.php?u=139512) has made some contributions to improve FaderPort8/16 functionality in this build, including developing a TrackNumberDisplay action. Thanks to Navelpluisje for the additions!

### BREAKING CHANGE: FB_FaderportRGB7bit is now FB_FaderportRGB
Anyone using an existing set of files for the Presonus Faderport8 or Faderport16 will need to update their .mst files to rename FB_FaderportRGB7bit to FB_FaderportRGB.

### New Action: TrackNumberDisplay
This new display action will show the Reaper track number on a display. This can be useful for multi-line displays like the Faderport8/16 or SCE24 where you may want to spare a line for the track number as well as OSC surfaces.

### New Feedback Processors: FB_FaderportTwoStateRGB, FB_FPVUMeter
FB_FaderportTwoStateRGB is used to allow two different colors for the Select buttons on the Faderport8/16 (track selected, track not selected), and FB_FPVUMeter allows one of the lines on the Faderport8/16 to function as a VU meter.

# August 14, 2022 Update

### Bug Fix: Custom Deltas
The 'Custom Delta' functionality for [[Encoders|Message-Generators#encoders]] was not working previously working and fixed in this build.

# August 5, 2022 Update

### New Feature: X-Touch Color Support (FB_XTouchDisplayUpper)
Color support for the X-Touch Universal and X-Touch Extender has been added. See [[FB_XTouchDisplayUpper|Feedback-Processors#fb_xtouchdisplayupper]] for details.

### Removed ForceScrollLink Action
The ForceScrollLink action has been removed.

### ToggleScrollLink now defaults to Off
The [[ToggleScrollLink|Navigation-Actions#togglescrolllink]] action now defaults to Off. Previously defaulted to on.

### Renamed FixedRGBColourDisplay to FixedRGBColorDisplay to standardize spelling
Standardizing CSI to consistently utilize US spelling. The FixedRGBColourDisplay action is now [[FixedRGBColorDisplay|Other-Actions#FixedRGBColorDisplay]].

### Renamed FB_GainReductionMeter to FB_ConsoleOneGainReductionMeter for clarity
Name changed to clarify this widget is specific to the Softube Console One.

### Renamed FB_VUMeter to FB_ConsoleOneVUMeter for clarity
Name changed to clarify this widget is specific to the Softube Console One.

# CSI Version 2.0 - July 10, 2022

CSI version 2.0 made a number of under-the-hood changes to zone loading to improve results and simplify some processes, while also expanding capabilities and improving features in various areas. This page is designed towards users with familiarity with CSI v1/1.1 authoring and is meant to assist in migration to version 2.0 by summarizing and consolidating the changes. 

### No more Navigators and fixed Zone names
In order to simplify .zon creation, the concept of Navigators have been removed (at least from the .zon files), and CSI inherits the Zone type based on a fixed set of Zone names. The fixed Zone names are:


```
Home
Buttons
Track
SelectedTrack
MasterTrack
SelectedTrackFXMenu
SelectedTrackSend
SelectedTrackReceive
TrackFXMenu
TrackReceive
TrackSend
```

### Changes to the Home.zon syntax (AssociatedZones)
CSI version 2 introduced the concept of AssociatedZones. These are zones like Sends, Receives, and FX Menus that are not activated as part of the home.zon, but will be called from this zone.

Below is an example of a typical MCU-style home.zon. The "Track" zone will use the displays and widgets when the Home zone is active, but if you want to call up an FX menu, Sends, or Receives to then takeover over some of the widgets, they need to be listed as AssociatedZones as shown below. When configured like this, the AssociatedZones function as "radio-button" style zones, where only one can be active at a given time (example: SelectedTrackSends or SelectedTrackReceives - not both simultaneously).
```
Zone Home
    IncludedZones
        "Buttons"
        "Track"
        "MasterTrack"
    IncludedZonesEnd
    AssociatedZones
       "SelectedTrackFXMenu"
       "SelectedTrackSend"
       "SelectedTrackReceive"
       "TrackFXMenu"
       "TrackSend"
       "TrackReceive"
    AssociatedZonesEnd
ZoneEnd
```

### No more “GoZone”
Fixed zone names mean we no longer need the GoZone action. Instead, users can activate fixed zones directly from the following list of actions:

* [GoHome](https://github.com/GeoffAWaddington/CSIWiki/wiki/GoHome)
* [GoSubZone](https://github.com/GeoffAWaddington/CSIWiki/wiki/FX-Sub-Zones)
* [LeaveSubZone](https://github.com/GeoffAWaddington/CSIWiki/wiki/LeaveSubZone)
* [GoTrackFXMenu](https://github.com/GeoffAWaddington/CSIWiki/wiki/GoTrackFXMenu)
* [GoTrackSend](https://github.com/GeoffAWaddington/CSIWiki/wiki/GoTrackSend)
* [GoTrackReceive](https://github.com/GeoffAWaddington/CSIWiki/wiki/GoTrackReceive)
* [GoSelectedTrackFXMenu](https://github.com/GeoffAWaddington/CSIWiki/wiki/GoSelectedTrackFXMenu)
* [GoSelectedTrackSend](https://github.com/GeoffAWaddington/CSIWiki/wiki/GoSelectedTrackSend)
* [GoSelectedTrackReceive](https://github.com/GeoffAWaddington/CSIWiki/wiki/GoSelectedTrackReceive)
* [GoSelectedTrack](https://github.com/GeoffAWaddington/CSIWiki/wiki/GoSelectedTrack)

### Zone feedback
Now, active zones will send feedback to surfaces that support this like the Behringer X-Touch, X-Touch One, etc. Example: if the Home zone is active, the button dedicated to this zone on these types of surfaces will light up. Same for the Send/Receive/FXMenu type zones. 

### Broadcast and Receive syntax improved
In CSI, you can instruct one surface to "broadcast" zone changes to another surface to keep them in sync, as long as that other surface is set to "receive" and listen to those broadcast changes. This is not new functionality, however, rather than having a bunch of separate actions for this behavior, you can now list all your broadcast message types on a single row in the Home.zon, and same for Receive messages. 

Broadcast/Receive only works for "Home" and "Associated Zones". If you have SelectedTrackFXMenu or FXMenu set to broadcast/receive, that includes the GoFXSlot messages that activates those zones.

The below example shows what Broadcast/Receive would look like with all currently supported options:
```
Zone Home
OnInitialization Broadcast Home SelectedTrackSend SelectedTrackReceive SelectedTrackFXMenu TrackSend TrackReceive TrackFXMenu
OnInitialization Receive Home SelectedTrackSend SelectedTrackReceive SelectedTrackFXMenu TrackSend TrackReceive TrackFXMenu
    IncludedZones
        "Buttons"
        "Track"
        "MasterTrack"
    IncludedZonesEnd
    AssociatedZones
       "SelectedTrackFXMenu"
       "SelectedTrackSend"
       "SelectedTrackReceive"
       "TrackFXMenu"
       "TrackSend"
       "TrackReceive"
    AssociatedZonesEnd
ZoneEnd
```

### FocusedFX changes
The elimination of navigators mentioned above applies to FX.zon files too! By default, CSI version 2 will have FocusedFX mapping enabled. This means if you have a fx.zon file for a particular FX/instrument plugin, and open the GUI in Reaper, that mapping will become activated by default. You can toggle this behavior off and on by assigning a button to the ToggleEnableFocusedFXMapping action as shown below:
```
   SomeButton     ToggleEnableFocusedFXMapping
```

But what if you don't want FocusedFXMapping on by default?  Since the default toggle state is "on", you can add "OnInitialization ToggleFocusedFXMapping" to flip it to off when CSI starts up as shown below:
```
Zone Home
OnInitialization ToggleEnableFocusedFXMapping
OnInitialization Broadcast Home SelectedTrackSend SelectedTrackReceive SelectedTrackFXMenu TrackSend TrackReceive TrackFXMenu
OnInitialization Receive Home SelectedTrackSend SelectedTrackReceive SelectedTrackFXMenu TrackSend TrackReceive TrackFXMenu
    IncludedZones
        "Buttons"
        "Track"
        "MasterTrack"
    IncludedZonesEnd
    AssociatedZones
       "SelectedTrackFXMenu"
       "SelectedTrackSend"
       "SelectedTrackReceive"
       "TrackFXMenu"
       "TrackSend"
       "TrackReceive"
    AssociatedZonesEnd
ZoneEnd
```

### Rewind and FastForward improvements
Big improvements have been made to the Rewind and FastForward actions. They will now latch and seek the play/edit cursor. The first press of either button will result in a slower rewind/forward speed. If you press the button again, you get a second, faster speed. In addition, feedback has been added to these actions for use in OSC surfaces or any MIDI surfaces that can provide rewind/fastforward feedback.

### New "Flip" Modifier
A new modifier called "Flip" has been introduced with the intention of being assigned to the Flip button on MCU-style control surfaces. A common use-case for this would be to temporarily put track pans onto the faders for quick panning adjustments. And because it's a modifier, a quick press of the button this is assigned to will allow for latching.

Here's how you'd define the modifier in a Buttons zone.
```
Zone "Buttons"
    Flip      Flip
ZoneEnd
```

And an example of the intended use-case can be seen here where Flip gets used as a modifier to put TrackPan on the faders....
```
Zone "Track"
    DisplayUpper|               TrackNameDisplay
    Fader|Touch+DisplayLower|   TrackVolumeDisplay
    DisplayLower|               MCUTrackPanDisplay
    VUMeter|                    TrackOutputMeterMaxPeakLR
    Fader|                      TrackVolume 
    Flip+Fader|                 TrackPan 
    Rotary|                     MCUTrackPan
    RotaryPush|                 ToggleMCUTrackPanWidth
    RecordArm|                  TrackRecordArm
    Solo|                       TrackSolo
    Mute|                       TrackMute
    Select|                     TrackUniqueSelect
    Shift+Select|               TrackRangeSelect
    Control+Select|             TrackSelect
ZoneEnd
```

### SubZones are for more than just FX now
SubZones are custom zones (i.e. they don’t have a fixed, pre-defined name) that can be called up from other zones. Common use-cases for SubZones would be create a custom Zoom zone, or a custom Marker zone that can re-purpose some widget assignments and change the functionality of the surface. They are also commonly used FX SubZones, which existed in CSI version 1.1. Think of an example of a Mastering Suite that may have more parameters than you have controls for on your surface. With FX SubZones, you could create one zone for the compressor section, another for the limiter section, then activate those different surfaces via button presses.

### FX SubZone crashes on Mac have been resolved
In CSI v1.1, using a SubZone on Mac would cause CSI to crash Reaper [there was a workaround]. These now work as intended without crashing.

### Banking actions can exist in the buttons zone
In CSI v1.1, Send, Receive and FX Menu banking actions needed to exist in those zones. In CSI v2, there are unique banking actions for each type. Those banking actions are no longer constrained to the Send, Receive, FX Menu zone files…you can use those in the buttons.zon

### Track Pin features have been removed
Track pinning has been removed [as of now] from CSI v2 in order to clean up and simplify the code behind the scenes.

### New CSI Preferences screen
To accommodate some under the hood changes how to surfaces get assigned to Pages, the CSI preferences screen has been redesigned. It now has a left to right flow where first you configure Surfaces on the left, then create one or more [[Pages]] in the middle, then use the Assignments section to assign surfaces to each page. See [[Setting up CSI for the first time|Installation-and-Setup#setting-up-your-csi-devices-for-the-first-time]] for more details. 

![CSI Preferences Screen Print](https://i.imgur.com/3gqL16s.png)

### CSI Preferences: removed the FX Menu and Send/Receive channel count fields
In the CSI Device Preferences, you'll no longer see the Send/Receive channel count fields. They inherit the count from the channel count.

### New CSI.ini format, new file error handling
Version 2.0 introduces changes to the file [[CSI.ini|CSI.INI]] file format. Additionally, now CSI will check for an incorrectly formatted CSI.ini and warn users when Reaper starts, notifying users of the version mismatch (rather than crashing Reaper). In general, CSI will warn users if there's a missing folder or incorrectly formated CSI.ini and should not crash Reaper.

### MCUTrackPan action no longer requires toggle row in .mst file
If the Rotary encoders in your .mst file contained the toggle line that was required for the MCUTrackPan action, that line can now be removed.

So if you previously had this in your .mst file...
```
Widget Rotary1
	Encoder b0 10 7f [ < 41-48 > 01-08 ]
	FB_Encoder b0 10 7f
	Toggle 90 20 7f
WidgetEnd
```

You can delete the toggle row so it looks like this...
```
Widget Rotary1
	Encoder b0 10 7f [ < 41-48 > 01-08 ]
	FB_Encoder b0 10 7f
WidgetEnd
```
### RGB Color Fixes
Reaper handles RGB colors differently between Mac and PC. A bug in CSI has been fixed to correct the RGB behavior on both platforms.

### TouchOSC can now run locally
You can now use the TouchOSC [mk II] application running on your local machine as a control surface in CSI.

### Native Apple Silicon (ARM) support, universal binary
Mac users with M1 [or more recent] can now use CSI natively in the Reaper ARM version. The .dylib is now a universal binary that includes ARM and Intel versions. You will need to allow the CSI .dylib to have access within Security and Privacy settings in Mac OS, then restart Reaper.

### Catalina or greater required for MacOS
The minimum version of MacOS supported is 10.15. This was changed in order to support new processors.

### No more 32-bit CSI builds
CSI no longer includes 32-bit builds on Windows and Mac. This was dropped in order to support new processors. 

### New actions
Here’s a list of new actions introduced in CSI v2:

* SaveProject
* Undo
* Redo
* MCUTimeDisplay (previously existed and named TimeDisplay)
* CycleTrackVCAFolderModes
* TrackVCAFolderModeDisplay
* Broadcast
* Receive
* GoHome
* GoSubZone
* LeaveSubZone
* ToggleEnableFocusedFXMapping
* ToggleEnableFocusedFXParamMapping
* GoSelectedTrackFX
* GoTrackSend
* GoTrackReceive
* GoTrackFXMenu
* GoSelectedTrackSend
* GoSelectedTrackReceive
* GoSelectedTrackFXMenu
* TrackSendBank
* TrackReceiveBank
* TrackFXMenuBank
* SelectedTrackSendBank
* SelectedTrackReceiveBank
* SelectedTrackFXMenuBank
* Flip
* ToggleFXBypass
* FXBypassDisplay

### Deprecated actions
Here’s a list of legacy actions that have been removed. Many of these actions had to do with how zones were activated.

* TimeDisplay (still exists - was renamed to MCUTimeDisplay)
* MapSelectedTrackSendsToWidgets
* UnmapSelectedTrackSendsFromWidgets
* MapTrackSendsSlotToWidgets
* UnmapTrackSendsSlotFromWidgets
* MapSelectedTrackSendsSlotToWidgets
* UnmapSelectedTrackSendsSlotFromWidgets
* MapSelectedTrackReceivesToWidgets
* UnmapSelectedTrackReceivesFromWidgets
* MapTrackReceivesSlotToWidgets
* UnmapTrackReceivesSlotFromWidgets
* MapSelectedTrackReceivesSlotToWidgets
* UnmapSelectedTrackReceivesSlotFromWidgets
* FXParamRelative
* MapSelectedTrackFXToWidgets
* UnmapSelectedTrackFXFromMenu
* MapSelectedTrackFXToMenu
* UnmapSelectedTrackFXFromMenu
* MapTrackFXMenusSlotToWidgets
* UnmapTrackFXMenusSlotFromWidgets
* GoCurrentFXSlot
* SendSlotBank
* ReceiveSlotBank
* FXMenuSlotBank
* TogglePin
* GoZone
* SetBroadcastGoZone
* SetReceiveGoZone
* SetBroadcastGoFXSlot
* SetReceiveGoFXSlot
* SetBroadcastMapSelectedTrackSendsToWidgets
* SetReceiveMapSelectedTrackSendsToWidgets
* SetBroadcastMapSelectedTrackReceivesToWidgets
* SetReceiveMapSelectedTrackReceivesToWidgets
* SetBroadcastMapSelectedTrackFXToWidgets
* SetReceiveMapSelectedTrackFXToWidgets
* SetBroadcastMapSelectedTrackFXToMenu
* SetReceiveMapSelectedTrackFXToMenu
* SetBroadcastMapTrackSendsSlotToWidgets
* SetReceiveMapTrackSendsSlotToWidgets
* SetBroadcastMapTrackReceivesSlotToWidgets
* SetReceiveMapTrackReceivesSlotToWidgets
* SetBroadcastMapTrackFXMenusSlotToWidgets
* SetReceiveMapTrackFXMenusSlotToWidgets
* ToggleVCAMode
