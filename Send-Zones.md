## Three Different Send Zone Types
As of CSI version 1.1, there are 3 different type of Send zones to choose from depending on your particular needs, workflow, and surface. They are:

* SelectedTrackSend (Note: prior to CSI version 1.1, this was the only type of Send zone and simple called "Send")
* TrackSendSlot
* SelectedTrackSendSlot

**SelectedTrackSend** is best used when you have a surface with multiple channels and you want to map out the sends on the selected track, across those various channels. Example: you have an 8 channel MCU type device. Using a SelectedTrackSend zone will allow you to control up to 8 sends from the selected channel on each of the surface's channels. You setup the number of sends in the CSI device preferences.

**TrackSendSlot** is best used when you have a multiple channel surface, but you only want to see sends for the channel that corresponds to the track navigator. Example: you have an 8 channel MCU type device. Using the TrackSendSlot will show you Send Slot #1 for channels 1-8. If you want to see the send loaded in Send Slot #2, you will use SendSlotBank action to navigate to the next slot, at which point, you'll be looking at Send Slot #2 for channels 1-8. You setup the number of tracks in the CSI device preferences.

**SelectedTrackSendSlot** works best with single fader surfaces like the Behringer X-Touch One or Presonus FaderPort 2. This allows you to control sends from the Selected Channel by using the SendSlotBank action to navigate from send slot to send slot. In this use-case, you would setup 1 sein the CSI device preferences.

## Send Mapping and Unmapping Actions
Depending on the type of send zone you are creating, you will need to create a CSI action to map the sends. 

**SelectedTrackSend** uses the CSI action **MapSelectedTrackSendsToWidgets** for mapping. 

**TrackSendSlot** uses the CSI action **MapTrackSendsSlotToWidgets** for mapping.

**SelectedTrackSendSlot** uses the CSI action **MapSelectedTrackSendsSlotToWidgets** for mapping.

Each of those has a corresponding Unmap action.

## Activating a Send Map
You can activate the send map one of three ways. First, you can assign the mapping action above to a button like this...
```
Zone "Buttons"
      Send     MapSelectedTrackSendsToWidgets
ZoneEnd
```

Or, if you want the act of selecting a track to automatically map your sends (works with SelectedTrackSend and SelectedTrackSendSlot), you could set that up in your Home zone like this...
```
Zone Home
     OnTrackSelection MapSelectedTrackSendsToWidgets
     IncludedZones
          "Buttons"
          "SelectedTrack"
     IncludedZonesEnd
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

## Unmapping Send Zones
Assuming the Send zone does not live in your Home zone, you'll also need a GoZone "Home" somewhere in your surface zone file to un-map sends and get back Home.
```
Zone "Buttons"
     Send        MapSelectedTrackSendsToWidgets
     Cancel      GoZone "Home"
ZoneEnd
```

Or, if you'd want to manually unmap the sends, you could assign the Unmap action to a button or modifer+button combo...
```
     Shift+Send   UnmapSelectedTrackSendsFromWidgets
```

## SelectedTrackSend Zone Example
**Note:** SelectedTrackSend zones are a special type of zone so your send zone must be named "SelectedTrackSend" (exactly) and must use the SelectedTrackSendNavigator as shown below.

Here's an example of a typical MCU send zone.
```
Zone "SelectedTrackSend"
     SelectedTrackSendNavigator
     DisplayUpper|                      TrackSendNameDisplay
     DisplayLower|                      TrackSendVolumeDisplay
     Mute|                              TrackSendMute
     Fader|                             TrackSendVolume
     Rotary|                            TrackSendPan
     RotaryPush|                        NoAction
ZoneEnd
```

## TrackSendSlot Zone Example
**Note:** TrackSendSlot zones are a special type of zone so your send zone must be named "TrackSendSlot" (exactly) and must use the TrackSendSlotNavigator as shown below. 

Here's an example of a typical TrackSendSlot zone.
```
Zone "TrackSendSlot"
     TrackSendSlotNavigator
     DisplayUpper|      TrackSendNameDisplay
     DisplayLower|      TrackSendVolumeDisplay
     Fader|             TraclSendVolume
     BankLeft           SendSlotBank "-1"
     BankRight          SendSlotBank "1"
ZoneEnd
```

## SelectedTrackSendSlot Zone Example
**Note:** SelectedTrackSendSlot zones are a special type of zone so your send zone must be named "SelectedTrackSendSlot" (exactly) and must use the SelectedTrackSendSlotNavigator as shown below. 

```
Zone "SelectedTrackSendSlot"
     SelectedTrackSendSlotNavigator
     DisplayUpper1                      TrackNameDisplay
     DisplayLower1                      TrackSendNameDisplay
     Fader1Touch+DisplayLower1          TrackSendVolumeDisplay
     Fader1                             TrackSendVolume
     Mute1                              TrackSendMute
     Rotary1                            TrackSendPan
     BankLeft                           SendSlotBank -1
     BankRight                          SendSlotBank 1
ZoneEnd
```

## Send Actions
The available send zone actions are shown below.
```
TrackSendVolume
TrackSendPan
TrackSendMute
TrackSendPrePost
TrackSendInvertPolarity
TrackSendNameDisplay
TrackSendVolumeDisplay
TrackSendPanDisplay
TrackSendPrePostDisplay
MapSelectedTrackSendsToWidgets
UnmapSelectedTrackSendsFromWidgets
```