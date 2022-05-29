## Two Different FXMenu Zone Types
As of CSI version 1.1, there are 2 different types of FXMenu zones to choose from depending on your particular needs, workflow, and surface. They are:

* SelectedTrackFXMenu (Note: prior to CSI version 1.1, this was the only type of FXMenu zone and simple called "FXMenu")
* TrackFXMenuSlot

**SelectedTrackFXMenu** is best used when you have a surface with multiple channels and you want to map out the FXMenus on the selected track, across those various channels. Example: you have an 8 channel MCU type device. Using a SelectedTrackFXMenu zone will allow you to control up to 8 FXMenus from the selected channel on each of the surface's channels. You setup the number of FXMenus in the CSI device preferences for sends (there's not a separate item for FXMenus).

**TrackFXMenuSlot** is best used when you have a multiple channel surface, but you only want to see FX Slot for the channel that corresponds to the track navigator. Example: you have an 8 channel MCU type device. Using the TrackFXMenuSlot will show you FXMenu Slot #1 for channels 1-8. If you want to see the FX loaded in FXMenu Slot #2, you will use FXMenuSlotBank action to navigate to the next slot, at which point, you'll be looking at FXMenu Slot #2 for channels 1-8. You setup the number of tracks in the CSI device preferences.

**Pre-condition for both zone types:** the FX still need to be mapped with their own fx.zon files and those .zon files would need to use the SelectedTrackNavigator. See [FX Zones](https://github.com/GeoffAWaddington/reaper_csurf_integrator/wiki/FX-Zones) for details on how to create fx.zon files.

## FXMenu Mapping and Unmapping Actions
Depending on the type of FXMenu zone you are creating, you will need to create a CSI action to map the FXMenus. 

**SelectedTrackFXMenu** uses the CSI action **MapSelectedTrackFXToMenu** for mapping. 

**TrackFXMenuSlot** uses the CSI action **MapTrackFXMenusSlotToWidgets** for mapping.

Each of those has a corresponding Unmap action.

## Activating a FXMenu Map
You can activate the FXMenu map one of three ways. First, you can assign the mapping action above to a button like this...
```
Zone "Buttons"
      FXMenu     MapSelectedTrackFXToMenu
ZoneEnd
```

Or, if you want the act of selecting a track to automatically map your FXMenus (works with SelectedTrackFXMenu and SelectedTrackFXMenuSlot), you could set that up in your Home zone like this...
```
Zone Home
     OnTrackSelection MapSelectedTrackFXToMenu
     IncludedZones
          "Buttons"
          "SelectedTrack"
     IncludedZonesEnd
ZoneEnd
```

Or, if you can dedicate a portion of your surface to FXMenus and always want them to appear as part of your Home zone, you can simply add the FXMenu zone to the IncludedZones like this...
```
Zone Home
     IncludedZones
          "Buttons"
          "Channel"
          "SelectedTrackFXMenu"
     IncludedZonesEnd
ZoneEnd
```

## Unmapping FXMenu Zones
Assuming the FXMenu zone does not live in your Home zone, you'll also need a GoZone "Home" somewhere in your surface zone file to un-map FXMenus and get back Home.
```
Zone "Buttons"
     Plugin        MapSelectedTrackFXMenusToWidgets
     Cancel        GoZone "Home"
ZoneEnd
```

Or, if you'd want to manually unmap the FXMenus, you could assign the Unmap action to a button or modifer+button combo...
```
     Shift+FXMenu   UnmapSelectedTrackFXFromMenu
```

## SelectedTrackFXMenu Zone Example
**Note:** SelectedTrackFXMenu zones are a special type of zone so your FXMenu zone must be named "SelectedTrackFXMenu" (exactly) and must use the SelectedTrackFXMenuNavigator as shown below.

```
Zone "SelectedTrackFXMenu"
    SelectedTrackFXMenuNavigator
    DisplayUpper|               FXMenuNameDisplay
    RotaryPush|                 GoFXSlot
ZoneEnd
```

## TrackFXMenuSlot Zone Example
**Note:** TrackFXMenuSlot zones are a special type of zone so your FXMenu zone must be named "TrackFXMenuSlot" (exactly) and must use the TrackFXMenuSlotNavigator as shown below. 

Here's an example of a typical TrackFXMenuSlot zone.
```
Zone "TrackFXMenuSlot"
     TrackFXMenuSlotNavigator
     DisplayUpper|      TrackNameDisplay
     DisplayLower|      FXNameDisplay
     RotaryPush|        GoCurrentFXSlot
     BankLeft           FXMenuSlotBank "-1"
     BankRight          FXMenuSlotBank "1"
ZoneEnd
```

## FXMenu Actions
The available FXMenu actions are shown below.
```
FXMenuNameDisplay
MapSelectedTrackFXToMenu
UnmapSelectedTrackFXFromMenu
MapTrackFXMenusSlotToWidgets
UnmapTrackFXMenusSlotFromWidgets
GoFXSlot (used with SelectedTrackFXMenu)
GoCurrentFXSlot (used with TrackFXMenuSlot)
FXMenuSlotBank (used with TrackFXMenuSlot)
```