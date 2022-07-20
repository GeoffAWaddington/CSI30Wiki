Page under construction.

* [[TrackBank|Navigation Actions#TrackBank]]
* [[SelectedTrackBank|Navigation Actions#SelectedTrackBank]]
* [[TrackSendBank|Navigation Actions#TrackSendBank]]
* [[TrackReceiveBank|Navigation Actions#TrackReceiveBank]]
* [[TrackFXMenuBank|Navigation Actions#TrackFXMenuBank]]
* [[SelectedTrackSendBank|Navigation Actions#SelectedTrackSendBank]]
* [[SelectedTrackReceiveBank|Navigation Actions#SelectedTrackReceiveBank]]
* [[SelectedTrackFXMenuBank|Navigation Actions#SelectedTrackFXMenuBank]]
* [[ToggleSynchPageBanking]]
* [[ToggleScrollLink]]
* [[ForceScrollLink]]
* [[NextPage|Pages#paging-actions]]
* [[GoPage|Pages#paging-actions]]
* [[GoHome]]
* [[GoSubZone|FX-Sub-Zones]]
* [[LeaveSubZone]]
* [[PageNameDisplay|Pages#pagenamedisplay]]
* [[GoTrackSend]]
* [[GoTrackReceive]]
* [[GoTrackFXMenu]]
* [[GoSelectedTrackSend]]
* [[GoSelectedTrackReceive]]
* [[GoSelectedTrackFXMenu]]
* [[GoSelectedTrackFX]]

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