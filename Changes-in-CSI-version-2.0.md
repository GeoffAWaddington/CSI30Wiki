CSI version 2.0 made a number of under-the-hood changes to zone loading to improve results and simplify some processes. This page is designed towards users with familiarity with CSI v1/1.1 authoring and is meant to assist in migration to version 2.0 by summarizing and consolidating the changes. 

## No more navigators and fixed zone names
In order to simplify zone creation, navigators have been removed, and CSI inherits the zone type based on a fixed set of Zone names. The fixed zone names are:


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

Below is an example of a typical MCU-style zone with some displays. The "Track" zone will use the displays and widgets when the Home zone is active, but if you want to call up an FX menu, Sends, or Receives to then takeover over some of the widgets, they need to be listed as AssociatedZones as shown below. When configured like this, the AssociatedZones function as "radio-button" style zones, where only one can be active at a given time (example: SelectedTrackSends or SelectedTrackReceives - not both simultaneously).
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


## Revised Rewind and FastForward actions
Improvements have been made to the Rewind and FastForward actions. They will now latch and seek the play/edit cursor. Multiple presses will change the speed similar to rewind/fast forward buttons found on cable boxes, DVR’s, and streaming services, providing a familiar experience. 

## SubZones are for more than just FX now
SubZones are custom zones (i.e. they don’t have a fixed, pre-defined name) that can be called up from other zones. Common use-cases for SubZones would be create a custom Zoom zone, or a custom Marker zone that can re-purpose some widget assignments and change the functionality of the surface. They are also commonly used FX SubZones, which existed in CSI version 1.1. Think of an example of a Mastering Suite that may have more parameters than you have controls for on your surface. With FX SubZones, you could create one zone for the compressor section, another for the limiter section, then activate those different surfaces via button presses.

## FX SubZone crashes on Mac have been resolved
In CSI v1.1, using a SubZone on Mac would cause CSI to crash Reaper [there was a workaround]. These now work as intended without crashing.

## Banking actions can exist in the buttons zone
In CSI v1.1, Send, Receive and FX Menu banking actions needed to exist in those zones. In CSI v2, there are unique banking actions for each type. Those banking actions are no longer constrained to the Send, Receive, FX Menu zone files…you can use those in the buttons.zon

## Track Pin features have been removed
Track pinning has been removed [as of now] from CSI v2 in order to clean up and simplify the code behind the scenes.

## CSI Preferences: removed the FX Menu and Send/Receive channel count fields
In the CSI Device Preferences, you'll no longer see the Send/Receive channel count fields. They inherit the count from the channel count.

## New CSI.ini format, new file error handling
Version 2.0 introduces changes to the file CSI.ini file format. Additionally, now CSI will check for an incorrectly formatted CSI.ini and warn users when Reaper starts, notifying users of the version mismatch (rather than crashing Reaper). In general, CSI will warn users if there's a missing folder or incorrectly formated CSI.ini and should not crash Reaper.

## Native Apple Silicon (ARM) support, universal binary
Mac users with M1 [or more recent] can now use CSI natively in the Reaper ARM version. The .dylib is now a universal binary that includes ARM and Intel versions. You will need to allow the CSI .dylib to have access within Security and Privacy settings in Mac OS, then restart Reaper.

## Catalina or greater required for Mac OS
The minimum version of Mac OS supported is 10.14. This was dropped in order to support new processors.

## No more 32-bit CSI builds
CSI no longer includes 32-bit builds. This was dropped in order to support new processors. 

## New Wiki location
If you found this page via the CSI thread in the Reaper forums, you may not realize that the CSI Wiki link has changed. Please update your bookmarks. The new Wiki page is:

https://github.com/GeoffAWaddington/CSIWiki/wiki

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

## Depreciated actions
Here’s a list of legacy actions that have been depreciated. Many of these actions had to do with how zones were activated.

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
