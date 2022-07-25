# Send Zones

## Two types of Send zones
There are two types of Send Zones in CSI:

* SelectedTrackSend 
* TrackSend

**SelectedTrackSend** is best used when you have a surface with multiple channels and you want to map out the sends on the selected track, across those various channels. Example: you have an 8 channel MCU type device. If you have more than 8 sends on the selected track, you would use the SelectedTrackSendBank action to bank to the additional sends. 

**TrackSend** is best used when you have a multiple channel surface, but you only want to see sends for the channel that corresponds to that specific channel. Example: you have an 8 channel MCU type device. Using the TrackSend will show you Send Slot #1 for channels 1-8. If you want to see the send loaded in Send Slot #2, you will use TrackSendBank action to navigate to the next slot, at which point, you'll be looking at Send Slot #2 for channels 1-8. You setup the number of tracks in the CSI device preferences.


## Send Mapping and Unmapping Actions
Depending on the type of send zone you are creating, you will need to create a CSI action to map the sends. 

**SelectedTrackSend** uses the CSI action **GoSelectedTrackSend** for mapping. 

**TrackSend** uses the CSI action **GoTrackSend** for mapping.

Unmapping is a simple as triggering the action a second time or using the GoHome action (which is more of a brute force approach). 

## Activating a Send Map
You can activate the send map one of three ways. First, you can assign the mapping action above to a button like this...
```
Zone "Buttons"
      Send     GoSelectedTrackSend
ZoneEnd
```

Or, if you can dedicate a portion of your surface to sends and always want them to appear as part of your Home zone, you can simply add the send zone to the IncludedZones like this...
```
Zone Home
     IncludedZones
          "Buttons"
          "Channel"
          "SelectedTrackSend"
     IncludedZonesEnd
ZoneEnd
```

## SelectedTrackSend Zone Example
**Note:** SelectedTrackSend zones are a special type of zone so your send zone must be named "SelectedTrackSend" (exactly).

Here's an example of a typical MCU send zone.
```
Zone "SelectedTrackSend"
        DisplayUpper|               TrackSendNameDisplay
        DisplayLower|               TrackSendPanDisplay
        Control+DisplayLower|       TrackSendPrePostDisplay
        Control+RotaryPush|         TrackSendPrePost
        Fader|Touch+DisplayLower|   TrackSendVolumeDisplay
        Mute|                       TrackSendMute
        Rotary|                     TrackSendPan        
        Fader|                      TrackSendVolume
        Left                        SelectedTrackSendBank -8
        Right                       SelectedTrackSendBank 8
ZoneEnd
```

## TrackSend Zone Example
**Note:** TrackSend zones are a special type of zone so your send zone must be named "TrackSend" (exactly).

Here's an example of a typical TrackSend zone.
```
Zone "TrackSend"
        DisplayUpper|               TrackNameDisplay
        DisplayLower|               TrackSendNameDisplay
        FaderTouch+DisplayLower|    TrackSendVolumeDisplay
        Shift+DisplayLower|         TrackSendPrePostDisplay
        Shift+RotaryPush|           TrackSendPrePost
        Mute|                       TrackSendMute
        Rotary|                     TrackSendPan
        Fader|                      TrackSendVolume
        Up                          TrackSendBank -1
        Down                        TrackSendBank 1
ZoneEnd
```

# Send Actions
The available send zone actions are shown below.

* [[TrackSendVolume|Send-Zones#send-actions]]
* [[TrackSendVolumeDisplay|Send-Zones#send-actions]]
* [[TrackSendPan|Send-Zones#send-actions]]
* [[TrackSendPanDisplay|Send-Zones#send-actions]]
* [[TrackSendMute|Send-Zones#send-actions]]
* [[TrackSendPrePost|Send-Zones#send-actions]]
* [[TrackSendPrePostDisplay|Send-Zones#send-actions]]
* [[TrackSendStereoMonoToggle|Send-Zones#send-actions]]
* [[TrackSendInvertPolarity|Send-Zones#send-actions]]
* [[TrackSendNameDisplay|Send-Zones#send-actions]]