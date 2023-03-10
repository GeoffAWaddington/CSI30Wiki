## What are Receives?
Track Receives are like inverse sends. Lets say you're sending multiple mix elements to a "Room Reverb" bus. If you wanted to adjust the amount of reverb on multiple channels using Sends, you'd have to go track by track and adjust the send levels one at a time. A more efficient way would be to map the receives going into the Room Reverb bus, and adjusting the various levels from that one track.

It's ultimately a much more efficient way of controlling multiple send levels feeding the same [receive] bus. Track Receives are useful for reverb adjustments across multiple tracks, cue/headphone mixes, and even adjusting monitor mixes in a live setup.

## Two types of Receive zones
There are two types of Receive Zones in CSI:

* SelectedTrackReceive 
* TrackReceive

**SelectedTrackReceive** is best used when you have a surface with multiple channels and you want to map out the Receives on the selected track, across those various channels. Example: you have an 8 channel MCU type device. If you have more than 8 Receives on the selected track, you would use the SelectedTrackReceiveBank action to bank to the additional Receives. 

**TrackReceive** is best used when you have a multiple channel surface, but you only want to see Receives for the channel that corresponds that specific track. Example: you have an 8-channel MCU type device. Using the TrackReceive will show you Receive Slot #1 for channels 1-8. If you want to see the Receive loaded in Receive Slot #2, you will use TrackReceiveBank action to navigate to the next slot, at which point, you'll be looking at Receive Slot #2 for channels 1-8. You setup the number of tracks in the CSI device preferences.


## Receive Mapping and Unmapping Actions
Depending on the type of Receive zone you are creating, you will need to create a CSI action to map the Receives. 

**SelectedTrackReceive** uses the CSI action **GoSelectedTrackReceive** for mapping. 

**TrackReceive** uses the CSI action **GoTrackReceive** for mapping.

Unmapping is a simple as triggering the action a second time or using the GoHome action (which is more of a brute force approach). 

## Activating a Receive Map
You can activate the Receive map one of three ways. First, you can assign the mapping action above to a button like this...
```
Zone "Buttons"
      Receive     GoSelectedTrackReceive
ZoneEnd
```

Or, if you can dedicate a portion of your surface to Receives and always want them to appear as part of your Home zone, you can simply add the Receive zone to the IncludedZones like this...
```
Zone Home
     IncludedZones
          "Buttons"
          "Channel"
          "SelectedTrackReceive"
     IncludedZoneReceive
ZoneEnd
```

## SelectedTrackReceive Zone Example
**Note:** SelectedTrackReceive zones are a special type of zone so your Receive zone must be named "SelectedTrackReceive" (exactly).

Here's an example of a typical MCU Receive zone.
```
Zone "SelectedTrackReceive"
        DisplayUpper|               TrackReceiveNameDisplay
        DisplayLower|               TrackReceivePanDisplay
        Control+DisplayLower|       TrackReceivePrePostDisplay
        Control+RotaryPush|         TrackReceivePrePost
        Fader|Touch+DisplayLower|   TrackReceiveVolumeDisplay
        Mute|                       TrackReceiveMute
        Rotary|                     TrackReceivePan        
        Fader|                      TrackReceiveVolume
        Left                        SelectedTrackReceiveBank -8
        Right                       SelectedTrackReceiveBank 8
ZoneEnd
```

## TrackReceive Zone Example
**Note:** TrackReceive zones are a special type of zone so your Receive zone must be named "TrackReceive" (exactly).

Here's an example of a typical TrackReceive zone.
```
Zone "TrackReceive"
        DisplayUpper|               TrackNameDisplay
        DisplayLower|               TrackReceiveNameDisplay
        FaderTouch+DisplayLower|    TrackReceiveVolumeDisplay
        Shift+DisplayLower|         TrackReceivePrePostDisplay
        Shift+RotaryPush|           TrackReceivePrePost
        Mute|                       TrackReceiveMute
        Rotary|                     TrackReceivePan
        Fader|                      TrackReceiveVolume
        Up                          TrackReceiveBank -1
        Down                        TrackReceiveBank 1
ZoneEnd
```

# Receive Actions
The available Receive zone actions are shown below.

* [[TrackReceiveNameDisplay|Receive-Zones#TrackReceiveNameDisplay]]
* [[TrackReceiveVolume|Receive-Zones#trackReceivevolume-trackReceivevolumedisplay]]
* [[TrackReceiveVolumeDisplay|Receive-Zones##trackReceivevolume-trackReceivevolumedisplay]]
* [[TrackReceivePan|Receive-Zones#trackReceivepan-trackReceivepandisplay]]
* [[TrackReceivePanDisplay|Receive-Zones#trackReceivepan-trackReceivepandisplay]]
* [[TrackReceivePrePost|Receive-Zones#trackReceiveprepost-trackReceiveprepostdisplay]]
* [[TrackReceivePrePostDisplay|Receive-Zones#trackReceiveprepost-trackReceiveprepostdisplay]]
* [[TrackReceiveMute|Receive-Zones#trackReceivemute]]
* [[TrackReceiveStereoMonoToggle|Receive-Zones#trackReceivestereomonotoggle]]
* [[TrackReceiveInvertPolarity|Receive-Zones#trackReceiveinvertpolarity]]

## TrackReceiveNameDisplay
Use the TrackReceiveNameDisplay action in a "TrackReceive" or "SelectedTrackReceive" zone to display the name of the Receive channel.
```
Zone "TrackReceive"
	DisplayLower| 		    TrackReceiveNameDisplay
ZoneEnd
```

## TrackReceiveVolume, TrackReceiveVolumeDisplay
Use the TrackReceiveVolume action in a "TrackReceive" or "SelectedTrackReceive" zone to control the Receive level. TrackReceiveVolumeDisplay can be used in a display widget to show the volume level.
```
Zone "TrackReceive"
    	Fader|Touch+DisplayLower|   TrackReceiveVolumeDisplay
    	Fader|                      TrackReceiveVolume
ZoneEnd
```

## TrackReceivePan, TrackReceivePanDisplay
Use the TrackReceivePan action in a "TrackReceive" or "SelectedTrackReceive" zone to control the Receive panning. TrackReceivePanDisplay can be used in a display widget to show the pan position.
```
Zone "TrackReceive"
    	DisplayLower|               TrackReceivePanDisplay
    	Rotary|                     TrackReceivePan
ZoneEnd
```

## TrackReceivePrePost, TrackReceivePrePostDisplay
Use the TrackReceivePrePost action in a "TrackReceive" or "SelectedTrackReceive" zone to cycle through whether the Receive is 1) Post-Fader (Post-Pan), 2) Pre-Fader (Post-FX), or 3) Pre-FX. TrackReceivePrePostDisplay can be used in a display widget to show the current value.
```
Zone "TrackReceive"
    	Shift+DisplayLower|         TrackReceivePrePostDisplay
    	Shift+RotaryPush|           TrackReceivePrePost
ZoneEnd
```

## TrackReceiveMute
Use the TrackReceiveMute action in a "TrackReceive" or "SelectedTrackReceive" zone to toggle muting the Receive.
```
Zone "TrackReceive"
    	Mute|                       TrackReceiveMute
ZoneEnd
```

## TrackReceiveStereoMonoToggle
Use the TrackReceiveStereoMonoToggle action in a "TrackReceive" or "SelectedTrackReceive" zone to toggle between a stereo (default) and mono Receive.
```
Zone "TrackReceive"
    	Shift+Mute|                 TrackReceiveStereoMonoToggle
ZoneEnd
```

## TrackReceiveInvertPolarity
Use the TrackReceiveInvertPolarity action in a "TrackReceive" or "SelectedTrackReceive" zone to invert the polarity of the Receive.
```
Zone "TrackReceive"
    	Alt+Mute|                   TrackReceiveInvertPolarity
ZoneEnd
```