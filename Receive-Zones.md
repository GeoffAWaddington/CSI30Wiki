## What are Receives?
Track Receives are like inverse sends. Lets say you're sending multiple mix elements to a "Room Reverb" bus. If you wanted to adjust the amount of reverb on multiple channels using Sends, you'd have to go track by track and adjust the send levels one at a time. A more efficient way would be to map the receives going into the Room Reverb bus, and adjusting the various levels from that one track.

It's ultimately a much more efficient way of controlling multiple send levels feeding the same [receive] bus. Track Receives are useful for reverb adjustments across multiple tracks, cue/headphone mixes, and even adjusting monitor mixes in a live setup.

## Three Different Receive Zone Types
As of CSI version 1.1, there are 3 different type of Receive zones to choose from depending on your particular needs, workflow, and surface. They are:

* SelectedTrackReceive
* TrackReceiveSlot
* SelectedTrackReceiveSlot

**SelectedTrackReceive** is best used when you have a surface with multiple channels and you want to map out the Receives on the selected track, across those various channels. Example: you have an 8 channel MCU type device. Using a SelectedTrackReceive zone will allow you to control up to 8 Receives from the selected channel on each of the surface's channels. You setup the number of receives in the CSI device preferences for sends (there's not a separate item for receives).

**TrackReceiveSlot** is best used when you have a multiple channel surface, but you only want to see Receives for the channel that corresponds to the track navigator. Example: you have an 8 channel MCU type device. Using the TrackReceiveSlot will show you Receive Slot #1 for channels 1-8. If you want to see the Receive loaded in Receive Slot #2, you will use ReceiveSlotBank action to navigate to the next slot, at which point, you'll be looking at Receive Slot #2 for channels 1-8. You setup the number of tracks in the CSI device preferences.

**SelectedTrackReceiveSlot** works best with single fader surfaces like the Behringer X-Touch One or Presonus FaderPort 2. This allows you to control Receives from the Selected Channel by using the ReceiveSlotBank action to navigate from Receive slot to Receive slot. In this use-case, you would setup 1 send in the CSI device preferences (there's not a separate item for receives).

## Receive Mapping and Unmapping Actions
Depending on the type of Receive zone you are creating, you will need to create a CSI action to map the Receives. 

**SelectedTrackReceive** uses the CSI action **MapSelectedTrackReceivesToWidgets** for mapping. 

**TrackReceiveSlot** uses the CSI action **MapTrackReceivesSlotToWidgets** for mapping.

**SelectedTrackReceiveSlot** uses the CSI action **MapSelectedTrackReceivesSlotToWidgets** for mapping.

Each of those has a corresponding Unmap action.

## Activating a Receive Map
You can activate the Receive map one of three ways. First, you can assign the mapping action above to a button like this...
```
Zone "Buttons"
      Receive     MapSelectedTrackReceivesToWidgets
ZoneEnd
```

Or, if you want the act of selecting a track to automatically map your Receives (works with SelectedTrackReceive and SelectedTrackReceiveSlot), you could set that up in your Home zone like this...
```
Zone Home
     OnTrackSelection MapSelectedTrackReceivesToWidgets
     IncludedZones
          "Buttons"
          "SelectedTrack"
     IncludedZoneReceive
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

## Unmapping Receive Zones
Assuming the Receive zone does not live in your Home zone, you'll also need a GoZone "Home" somewhere in your surface zone file to un-map Receives and get back Home.
```
Zone "Buttons"
     Receive        MapSelectedTrackReceivesToWidgets
     Cancel      GoZone "Home"
ZoneEnd
```

Or, if you'd want to manually unmap the Receives, you could assign the Unmap action to a button or modifer+button combo...
```
     Shift+Receive   UnmapSelectedTrackReceivesFromWidgets
```

## SelectedTrackReceive Zone Example
**Note:** SelectedTrackReceive zones are a special type of zone so your Receive zone must be named "SelectedTrackReceive" (exactly) and must use the SelectedTrackReceiveNavigator as shown below.

Here's an example of a typical MCU Receive zone.
```
Zone "SelectedTrackReceive"
     SelectedTrackReceiveNavigator
     DisplayUpper|                      TrackReceiveNameDisplay
     DisplayLower|                      TrackReceiveVolumeDisplay
     Mute|                              TrackReceiveMute
     Fader|                             TrackReceiveVolume
     Rotary|                            TrackReceivePan
     RotaryPush|                        NoAction
ZoneEnd
```

## TrackReceiveSlot Zone Example
**Note:** TrackReceiveSlot zones are a special type of zone so your Receive zone must be named "TrackReceiveSlot" (exactly) and must use the TrackReceiveSlotNavigator as shown below. 

Here's an example of a typical TrackReceiveSlot zone.
```
Zone "TrackReceiveSlot"
     TrackReceiveSlotNavigator
     DisplayUpper|      TrackReceiveNameDisplay
     DisplayLower|      TrackReceiveVolumeDisplay
     Fader|             TraclReceiveVolume
     BankLeft           ReceiveSlotBank "-1"
     BankRight          ReceiveSlotBank "1"
ZoneEnd
```

## SelectedTrackReceiveSlot Zone Example
**Note:** SelectedTrackReceiveSlot zones are a special type of zone so your Receive zone must be named "SelectedTrackReceiveSlot" (exactly) and must use the SelectedTrackReceiveSlotNavigator as shown below. 

```
Zone "SelectedTrackReceiveSlot"
     SelectedTrackReceiveSlotNavigator
     DisplayUpper1                      TrackNameDisplay
     DisplayLower1                      TrackReceiveNameDisplay
     Fader1Touch+DisplayLower1          TrackReceiveVolumeDisplay
     Fader1                             TrackReceiveVolume
     Mute1                              TrackReceiveMute
     Rotary1                            TrackReceivePan
     BankLeft                           ReceiveSlotBank -1
     BankRight                          ReceiveSlotBank 1
ZoneEnd
```

## Receive Actions
The available receive actions are shown below.
```
TrackReceiveVolume
TrackReceivePan
TrackReceiveMute
TrackReceivePrePost
TrackReceiveInvertPolarity
TrackReceiveNameDisplay
TrackReceiveVolumeDisplay
TrackReceivePanDisplay
TrackReceivePrePostDisplay
```