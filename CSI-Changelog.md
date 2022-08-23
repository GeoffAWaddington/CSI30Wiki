# August 23, 2022 - Recent EXP Build Updates
Using this as a placeholder for some recent EXP build updates that will work their way to the main CSI branch once testing and development is complete.

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
Use ToggleFXOffline to change the FX status to "offline" in Reaper. Offline FX is similar to Bypass, but it removes the plugin from memory and additional processing. In the below example, it's assigned to Shift+Mute.
```
Zone "SelectedTrackFXMenu"
        DisplayUpper|         FXMenuNameDisplay
        DisplayLower|         FXBypassedDisplay
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
* FXBypassedDisplay

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
