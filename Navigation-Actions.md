* [[TrackBank|navigation actions#trackbank]]
* [[SelectedTrackBank|navigation actions#selectedtrackbank]]
* [[TrackSendBank|navigation actions#tracksendbank]]
* [[TrackReceiveBank|navigation actions#trackreceivebank]]
* [[TrackFXMenuBank|navigation actions#trackfxmenubank]]
* [[SelectedTrackSendBank|navigation actions#selectedtracksendbank]]
* [[SelectedTrackReceiveBank|navigation actions#selectedtrackreceivebank]]
* [[SelectedTrackFXMenuBank|navigation actions#selectedtrackfxmenubank]]
* [[ToggleSynchPageBanking|navigation actions#togglesynchpagebanking]]
* [[ToggleScrollLink|navigation actions#togglescrolllink]]
* [[GoHome|navigation actions#gohome]]
* [[GoSubZone|navigation actions#gosubzone-leavesubzone]]
* [[LeaveSubZone|navigation actions#gosubzone-leavesubzone]]
* [[GoTrackSend|navigation actions#gotracksend]]
* [[GoTrackReceive|navigation actions#gotrackreceive]]
* [[GoTrackFXMenu|navigation actions#gotrackfxmenu]]
* [[GoSelectedTrackSend|navigation actions#goselectedtracksend]]
* [[GoSelectedTrackReceive|navigation actions#goselectedtrackreceive]]
* [[GoSelectedTrackFXMenu|navigation actions#goselectedtrackfxmenu]]
* [[GoSelectedTrackFX|navigation actions#goselectedtrackfx]]
* [[GoPage|navigation actions#gopage-nextpage-pagenamedisplay]]
* [[NextPage|navigation actions#gopage-nextpage-pagenamedisplay]]
* [[PageNameDisplay|navigation actions#gopage-nextpage-pagenamedisplay]]


## TrackBank
Add this action to your Buttons zone for banking the surface in a Track context. No change is made to the track selection in Reaper. Positive or negative numbers after the action name will dictate how many tracks will banked at a time.
```
Zone "Buttons"
    BankLeft                    TrackBank -8
    BankRight                   TrackBank 8
    ChannelLeft                 TrackBank -1
    ChannelRight                TrackBank 1
ZoneEnd
```

**Note:** The TrackBank action will automatically bank VCA and Folder tracks when CSI's [[CycleTrackVCAFolderModes|VCA's-and-VCA-Spill#cycletrackvcafoldermodes-trackvcafoldermodedisplay]] action is set to one of those respective modes.  

## SelectedTrackBank
This action works similarly to TrackBank but selects the track in Reaper. Use this in a selected track style workflow.
```
Zone "Buttons"
    BankLeft                    SelectedTrackBank -8
    BankRight                   SelectedTrackBank 8
    ChannelLeft                 SelectedTrackBank -1
    ChannelRight                SelectedTrackBank 1
ZoneEnd
```

**Tip:** If you would prefer a workflow where you can bank your multi-fader surface but also have the track selection in Reaper updated each time you do, then you can combine both actions, one right after the other as shown below.
```
Zone "Buttons"
    BankLeft                    TrackBank -8
    BankLeft                    SelectedTrackBank -8
    BankRight                   TrackBank 8
    BankRight                   SelectedTrackBank 8
    ChannelLeft                 TrackBank -1
    ChannelLeft                 SelectedTrackBank -1
    ChannelRight                TrackBank 1
    ChannelRight                SelectedTrackBank 1
ZoneEnd
```

## TrackSendBank
Use this action for banking the sends in a TrackSend zone.
```
Zone "TrackSend"
	DisplayLower| 		    TrackSendNameDisplay
    	Fader|Touch+DisplayLower|   TrackSendVolumeDisplay
    	Mute|                       TrackSendMute
    	Rotary|                     TrackSendPan
    	Fader|                      TrackSendVolume
    	Up                          TrackSendBank -1
    	Down                        TrackSendBank 1
ZoneEnd
```

## TrackReceiveBank
Use this action for banking the Receives in a TrackReceive zone.
```
Zone "TrackReceive"
	DisplayLower| 		    TrackReceiveNameDisplay
    	Fader|Touch+DisplayLower|   TrackReceiveVolumeDisplay
    	Mute|                       TrackReceiveMute
    	Rotary|                     TrackReceivePan
    	Fader|                      TrackReceiveVolume
    	Up                          TrackReceiveBank -1
    	Down                        TrackReceiveBank 1
ZoneEnd
```

## TrackFXMenuBank
Use this action to bank FX Menu slots in a TrackFXMenu zone.
```
Zone "TrackFXMenu"
        DisplayLower|       	FXMenuNameDisplay
        Rotary|             	NoAction
        RotaryPush|         	GoFXSlot
	Mute| 			ToggleFXBypass
        Up	            	TrackFXMenuBank -1
        Down	           	TrackFXMenuBank 1
ZoneEnd
```

## SelectedTrackSendBank
Use this action for banking the sends in a SelectedTrackSend zone.
```
Zone "SelectedTrackSend"
    	DisplayUpper|               TrackSendNameDisplay
	DisplayLower| 		    TrackSendPrePostDisplay
    	Fader|Touch+DisplayLower|   TrackSendVolumeDisplay
	RotaryPush|		    TrackSendPrePost
    	Mute|                       TrackSendMute
    	Rotary|                     TrackSendPan
    	Fader|                      TrackSendVolume
    	Left                        SelectedTrackSendBank -1
    	Right                       SelectedTrackSendBank 1
ZoneEnd
```

## SelectedTrackReceiveBank
Use this action for banking the Receives in a SelectedTrackReceive zone.
```
Zone "SelectedTrackReceive"
    DisplayUpper|               TrackReceiveNameDisplay
    DisplayLower| 		TrackReceivePrePostDisplay
    Fader|Touch+DisplayLower|   TrackReceiveVolumeDisplay
    RotaryPush|			TrackReceivePrePost
    Mute|			TrackReceiveMute
    Rotary|                     TrackReceivePan
    Fader|                      TrackReceiveVolume
    Left			SelectedTrackReceiveBank -1
    Right   			SelectedTrackReceiveBank 1
ZoneEnd
```

## SelectedTrackFXMenuBank
Use this action to bank FX Menu slots in a SelectedTrackFXMenu zone.
```
Zone "SelectedTrackFXMenu"
        DisplayUpper|       	FXMenuNameDisplay
	DisplayLower|       	FXBypassedDisplay
        Rotary|             	NoAction
        RotaryPush|         	GoFXSlot
	Mute| 			ToggleFXBypass
        Left            	SelectedTrackFXMenuBank -1
        Right           	SelectedTrackFXMenuBank 1
ZoneEnd
```

## ToggleSynchPageBanking
This action will toggle whether or not surface banking stays in sync when you change [[Pages|Pages]] in CSI. Example: if your surface is banked to show channels 25 to 32 on your HomePage, then you switch to a second page, do you want to maintain the banking position? This is enabled by default. This action will toggle the behavior.
```
Zone "Buttons"
     F8     ToggleSynchPageBanking
ZoneEnd
```

## ToggleScrollLink
This action toggles the behavior where Reaper's MCP will follow CSI's banking and scroll in sync with the surface. **Note:** as of the August 5, 2022 CSI release, this defaults to Off (previously defaulted to On). 
```
Zone "Buttons"
     F7     ToggleScrollLink
ZoneEnd
```

## GoHome
Assign this action to a button to return to the Home Zone. If your surface supports feedback, you will see a light engaged when the Home zone is active.
```
Zone "Buttons"
    GlobalView                  GoHome
ZoneEnd
```

## GoSubZone, LeaveSubZone
Use the GoSubZone action when you want to create custom SubZones to be called up on-demand. The SubZones need to be listed as SubZones within the surface definition. In the below example, we've got two SubZones, one for a "Marker" zone and another for a "Zoom" SubZone. The syntax to flag the included SubZones and the buttons to activate them would look like this...
```
Zone "Buttons"
    SubZones
        "Marker"
        "Zoom"
    SubZonesEnd
     Marker                             GoSubZone "Marker"
     Zoom                               GoSubZone "Zoom"
```

The SubZone itself would look like any other standalone .zon file. Also note that once we are in one of those custom SubZones, we need a way to exit the SubZone. You may want to use the same button you used to activate the SubZone in order for it to behave like a toggle. See the example below from the Marker SubZone.
```
Zone "Marker"
    Up                   Reaper 40613   // Delete marker near cursor
    Down                 Reaper 40157   // Insert marker at current or edit position
    Right                Reaper 40173   // Go to next marker or project end
    Left                 Reaper 40172   // Go to previous marker or project start

    JogWheelRotaryCW     Reaper 40173   // Go to next marker or project end    
    JogWheelRotaryCCW    Reaper 40172   // Go to previous marker or project start
    
    Marker               LeaveSubZone
ZoneEnd
```

Click here to learn about [[FX SubZones|FX and Instrument Mapping#fx-subzones]].

## GoTrackSend
Use the GoTrackSend action to activate the [[TrackSend|Send Zones]] zone type. This type of zone has a fixed name. This action provides feedback to surfaces that support it and will act as a toggle for activating and exiting the zone.
```
Zone "Buttons"
    AudioInstrument             GoTrackSend
ZoneEnd
```

## GoTrackReceive
Use the GoTrackReceive action to activate a [[TrackReceive|Receive Zones]] zone type. This type of zone has a fixed name. This action provides feedback to surfaces that support it and will act as a toggle for activating and exiting the zone.
```
Zone "Buttons"
    Aux                         GoTrackReceive
ZoneEnd
```

## GoTrackFXMenu
Use the GoTrackFXMenu action to activate a [[TrackFXMenu|FX-Zone-Activation-(FocusedFX,-TrackFXMenu,-SelectedTrackFXMenu,-SelectedTrack)#trackfxmenu]] zone. This type of zone has a fixed name. This action provides feedback to surfaces that support it and will act as a toggle for activating and exiting the zone.
```
Zone "Buttons"
    Busses                      GoTrackFXMenu
ZoneEnd
```

## GoSelectedTrackSend
Use the GoSelectedTrackSend action to activate a [[SelectedTrackSend|Send Zones]] zone. This type of zone has a fixed name. This action provides feedback to surfaces that support it and will act as a toggle for activating and exiting the zone.
```
Zone "Buttons"
    MidiTracks                  GoSelectedTrackSend
ZoneEnd
```

## GoSelectedTrackReceive
Use the GoSelectedTrackReceive action to activate a [[SelectedTrackReceive|Receive Zones]] zone. This type of zone has a fixed name. This action provides feedback to surfaces that support it and will act as a toggle for activating and exiting the zone.
```
Zone "Buttons"
    Inputs                      GoSelectedTrackReceive
ZoneEnd
```

## GoSelectedTrackFXMenu
Use the GoSelectedTrackFXMenu action to activate a [[SelectedTrackFXMenu|FX-Zone-Activation-(FocusedFX,-TrackFXMenu,-SelectedTrackFXMenu,-SelectedTrack)#selectedtrackfxmenu]] zone. This type of zone has a fixed name. This action provides feedback to surfaces that support it and will act as a toggle for activating and exiting the zone.
```
Zone "Buttons"
    AudioTracks                 GoSelectedTrackFXMenu
ZoneEnd
```

## GoPage, NextPage, PageNameDisplay
If using multiple [[Pages]], the buttons on surface can be assigned to switch between Pages. Use the CSI **GoPage** action to jump between Pages in CSI. Use **NextPage** to cycle between Pages. In the below example, I have a Page called Home and another called Mix. Those Pages will point to different zone folders that are configured to function differently based on the page. See [[Pages]] for more details about when and how to use Pages in CSI, for now we'll just focus on the actions to change pages.

On my Home page, I may want to utilize the "Channel" button on my surface to enter the Mix Page. Note: this "Buttons.zon" exists on the "HomePage" Page, and is different than the "Buttons.zon" on the "Mix" Page.

````
Zone "Buttons"
        Channel         GoPage "Mix"  // Activates the Mix page
ZoneEnd
````

But in order to get back to the Home page, I probably want to make sure I have the opposite happening when the Mix page is active. Note: this "Buttons.zon" exists on the "Mix" Page, and is different than the "Buttons.zon" on the "HomePage".
````
Zone "Buttons"
        Channel         GoPage "Home"  // Activates the Home page
ZoneEnd
````

You could also use the NextPage action to just cycle through the Pages in your CSI setup. In a two-page setup this would essentially be a toggle but if you had 3 or more pages it will cycle through them.
```
Zone "Buttons"
        Channel         NextPage  // Cycles through the list of pages
ZoneEnd
```

Finally, while not a Navigation Action, the PageNameDisplay action can be assigned to a Display widget in order to show the name of the currently active Page.

```
MainDisplay     PageNameDisplay
```