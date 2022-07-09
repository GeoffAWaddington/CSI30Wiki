CSI version 2.0 made a number of under-the-hood changes to zone loading to improve results and simplify some processes, while also expanding capabilities and improving features in various areas. This page is designed towards users with familiarity with CSI v1/1.1 authoring and is meant to assist in migration to version 2.0 by summarizing and consolidating the changes. 

## No more Navigators and fixed Zone names
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

## Changes to the Home.zon syntax (AssociatedZones)
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

## No more “GoZone”
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

## Zone feedback
Now, active zones will send feedback to surfaces that support this like the Behringer X-Touch, X-Touch One, etc. Example: if the Home zone is active, the button dedicated to this zone on these types of surfaces will light up. Same for the Send/Receive/FXMenu type zones. 

## Broadcast and Receive syntax improved
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

## FocusedFX changes
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

## Rewind and FastForward improvements
Big improvements have been made to the Rewind and FastForward actions. They will now latch and seek the play/edit cursor. The first press of either button will result in a slower rewind/forward speed. If you press the button again, you get a second, faster speed. In addition, feedback has been added to these actions for use in OSC surfaces or any MIDI surfaces that can provide rewind/fastforward feedback.

## New "Flip" Modifier
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

## SubZones are for more than just FX now
SubZones are custom zones (i.e. they don’t have a fixed, pre-defined name) that can be called up from other zones. Common use-cases for SubZones would be create a custom Zoom zone, or a custom Marker zone that can re-purpose some widget assignments and change the functionality of the surface. They are also commonly used FX SubZones, which existed in CSI version 1.1. Think of an example of a Mastering Suite that may have more parameters than you have controls for on your surface. With FX SubZones, you could create one zone for the compressor section, another for the limiter section, then activate those different surfaces via button presses.

## FX SubZone crashes on Mac have been resolved
In CSI v1.1, using a SubZone on Mac would cause CSI to crash Reaper [there was a workaround]. These now work as intended without crashing.

## Banking actions can exist in the buttons zone
In CSI v1.1, Send, Receive and FX Menu banking actions needed to exist in those zones. In CSI v2, there are unique banking actions for each type. Those banking actions are no longer constrained to the Send, Receive, FX Menu zone files…you can use those in the buttons.zon

## Track Pin features have been removed
Track pinning has been removed [as of now] from CSI v2 in order to clean up and simplify the code behind the scenes.

## New CSI Preferences screen
To accommodate some under the hood changes how to surfaces get assigned to Pages, the CSI preferences screen has been redesigned. It now has a left to right flow where first you configure Surfaces on the left, then create one or more [[Pages]] in the middle, then use the Assignments section to assign surfaces to each page. See [[Setting up CSI for the first time|Installation-and-Setup#setting-up-your-csi-devices-for-the-first-time]] for more details. 

![CSI Preferences Screen Print](https://i.imgur.com/3gqL16s.png)

## CSI Preferences: removed the FX Menu and Send/Receive channel count fields
In the CSI Device Preferences, you'll no longer see the Send/Receive channel count fields. They inherit the count from the channel count.

## New CSI.ini format, new file error handling
Version 2.0 introduces changes to the file [[CSI.ini|CSI.INI]] file format. Additionally, now CSI will check for an incorrectly formatted CSI.ini and warn users when Reaper starts, notifying users of the version mismatch (rather than crashing Reaper). In general, CSI will warn users if there's a missing folder or incorrectly formated CSI.ini and should not crash Reaper.

## TouchOSC can now run locally
You can now use the TouchOSC [mk II] application running on your local machine as a control surface in CSI.

## Native Apple Silicon (ARM) support, universal binary
Mac users with M1 [or more recent] can now use CSI natively in the Reaper ARM version. The .dylib is now a universal binary that includes ARM and Intel versions. You will need to allow the CSI .dylib to have access within Security and Privacy settings in Mac OS, then restart Reaper.

## Catalina or greater required for MacOS
The minimum version of MacOS supported is 10.15. This was changed in order to support new processors.

## No more 32-bit CSI builds
CSI no longer includes 32-bit builds on Windows and Mac. This was dropped in order to support new processors. 

## New actions
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

## Deprecated actions
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
