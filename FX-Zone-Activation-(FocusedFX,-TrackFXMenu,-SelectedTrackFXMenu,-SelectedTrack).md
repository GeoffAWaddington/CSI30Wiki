## Overview: Four Different Activation Methods:
Once you’ve created some fx.zon files, the next step is determining how you want that fx.zon to be activated. CSI offers 4 methods for activating fx zones:

* **FocusedFX:** Opening the GUI in Reaper will cause the plugin to map on your surface. This mode is enabled by default. **Mapping action: ToggleEnableFocusedFXMapping**

* **TrackFXMenu:** You select the plugin you want to activate from a menu on your control surface. TrackFXMenu splays the FX slots out vertically, so imagine an 8-fader surface with displays showing FX Slot 1 on Track 1, FX Slot 1 on Track 2, FX Slot 1 on Track 3, etc. You change FX slots by banking vertically. **Mapping action: GoTrackFXMenu** to activate the zone, **GoFXSlot** to activate the FX mapping.

* **SelectedTrackFXMenu:** Like TrackFXMenu, you select the plugin you want to activate from a menu on your control surface. SelectedTrackFXMenu splays the FX slots of the selected track horizontally. Here, imagine an 8-fader surface with displays. You've got track 3 selected in Reaper. SelectedTrackFXMenu will show FX Slot 1 on Track 3, FX Slot 2 on Track 3, FX Slot 3 on Track 3, etc. spread out across the 8 channels on your surface. If you have more FX on the selected track than you have control surface channels for, you can change FX slots by banking horizontally. **Mapping action: GoSelectedTrackFXMenu**, **GoFXSlot** to activate the FX mapping.

* **SelectedTrack:** When this mode is enabled, simply selecting a track in Reaper will activate any FX maps on that track. This is particularly handy for control surfaces with pre-defined layouts like the Softube Console One. With this mode, because multiple FX can be activated at once (example: an EQ and a compressor), users would have to be careful when mapping their fx.zon files as to avoid conflicts. **Mapping action: GoSelectedTrack**

**Tip:** You can use different FX activation methods on the same surface and use the various mapping actions to dictate which method is activate at any given time. Example: you can have a TrackFXMenu and a SelectedTrackFXMenu that you use a button to switch between, while also having another button toggling FocusedFXMapping as needed.

## FocusedFXMapping
CSI version 2 has FocusedFX mapping enabled by default. This means if you have a fx.zon file for a particular FX/instrument plugin, and open the GUI in Reaper, that mapping will become activated on your surface by default. You can toggle this behavior off and on by assigning a button to the ToggleEnableFocusedFXMapping action as shown below:
```
   SomeButton     ToggleEnableFocusedFXMapping
```

But what if you don't want FocusedFXMapping on by default?  Since the default toggle state is "on", you can add "OnInitialization ToggleFocusedFXMapping" to flip it to off when CSI starts up as shown below:
```
Zone Home
OnInitialization ToggleEnableFocusedFXMapping
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

## TrackFXMenu
TrackFXMenu is is best used when you have a multi-channel control surface and want to see the one FX Slot at a time for each different channel of the surface. Example: you have an 8 channel MCU type device. Using the TrackFXMenu will show you FXMenu Slot #1 for channels 1-8. If you want to see the FX loaded in FXMenu Slot #2, you will use TrackFXMenuBank action to navigate to the next slot, at which point, you'll be looking at FXMenu Slot #2 for channels 1-8. 

For TrackFXMenu to work, you first need a dedicated TrackFXMenu.zon in your Surface folder. The zone name must be TrackFXMenu and cannot be customized. Here is a typical TrackFXMenu you might use on a MCU-style device.
```
Zone "TrackFXMenu"
    DisplayLower|         FXMenuNameDisplay
    Rotary|               NoAction
    RotaryPush|           GoFXSlot
    Mute|                 ToggleFXBypass
    Up                    TrackFXMenuBank -1
    Down                  TrackFXMenuBank 1

    RecordArm|            NoAction
    Solo|                 NoAction
    Select|               NoAction
ZoneEnd
```
The key things to call out here is that we are using the **FXMenuNameDisplay** action to show the name of the plugin (or the alias if one exists) on the control surface and the **GoFXSlot** action is assigned to the corresponding RotaryPush to actually activate the FX.

Next you need to decide if you want the TrackFXMenu to be called up as needed, and therefore as part of an Associated Zone, or if you want to include it directly in your Home.zon. If you're using a MCU-style device, the most common usage will be to call it up as-needed as part of an AssociatedZone. If you're using more of a Mackie C4-style device, you may want to include the FXMenu in your Home zone. I will show how to setup both options below.

### TrackFXMenu as an AssociatedZone
If you want to use a button to call up the TrackFXMenu as-needed, you need to first define that TrackFXMenu zone as an AssociatedZone in your Home.zon file. Here's an example of what that might look like.
```
Zone Home
    IncludedZones
        "Buttons"
        "Track"
        "MasterTrack"
    IncludedZonesEnd
    AssociatedZones
       "TrackFXMenu"
    AssociatedZonesEnd
ZoneEnd
```

Next, you would need to include the GoTrackFXMenu action in your Buttons.zon as shown below. GoTrackFXMenu acts as a toggle, so pressing it once will activate the TrackFXMenu zone and a second press will exit that zone. Additionally, if you're using a surface+widget that supports feedback, a light will appear when this zone is active.
```
Zone "Buttons"
    Busses                      GoTrackFXMenu
ZoneEnd
```

### TrackFXMenu as part of your Home.zon
If you prefer to have TrackFXmenu on whenever you're in your Home Zone, no additional button will be required to activate the TrackFXMenu zone. You just need to be sure TrackFXMenu exists as an IncludedZone in your Home.zon as shown below.
```
Zone Home
    IncludedZones
        "Buttons"
        "Track"
        "TrackFXMenu"
        "MasterTrack"
    IncludedZonesEnd
ZoneEnd
```

## SelectedTrackFXMenu
SelectedTrackFXMenu is is best used when you have a multi-channel control surface and want to see the FX of the selected channel splayed out horizontally on your surface. Example: you have an 8-channel MCU type device. Using a SelectedTrackFXMenu zone will allow you to control up to 8 FXMenus from the selected channel on each of the surface's channels. 

For SelectedTrackFXMenu to work, you first need a dedicated SelectedTrackFXMenu.zon in your Surface folder. The zone name must be SelectedTrackFXMenu and cannot be customized. Here is a typical SelectedTrackFXMenu you might use on a MCU-style device.
```
Zone "SelectedTrackFXMenu"
     DisplayUpper|    FXMenuNameDisplay
     DisplayLower|    FXBypassedDisplay
     Rotary|          NoAction
     RotaryPush|      GoFXSlot
     RotaryPush|      Reaper "_S&M_FLOATFX|"          // Floats the FX window, use "_S&M_SHOWFXCHAIN|" insteead if you prefer the FX chain window
     Mute|            ToggleFXBypass
     Left             SelectedTrackFXMenuBank -1
     Right            SelectedTrackFXMenuBank 1

     RecordArm|       NoAction
     Solo|            NoAction
     Select|          NoAction
ZoneEnd
```
The key things to call out here is that we are using the **FXMenuNameDisplay** action to show the name of the plugin (or the alias if one exists) on the control surface and the **GoFXSlot** action is assigned to the corresponding RotaryPush to actually activate the FX. Additionally, I've added an SWS extension action to actually open the plugin GUI when the FX map is activated. If you don't want this behvaior, you can remove that line.

Next you need to decide if you want the SelectedTrackFXMenu to be called up as needed, and therefore as part of an Associated Zone, or if you want to include it directly in your Home.zon. If you're using a MCU-style device, the most common usage will be to call it up as-needed as part of an AssociatedZone. If you're using more of a Mackie C4-style device, you may want to include the FXMenu in your Home zone. I will show how to setup both options below.

### SelectedTrackFXMenu as an AssociatedZone
If you want to use a button to call up the SelectedTrackFXMenu as-needed, you need to first define that SelectedTrackFXMenu zone as an AssociatedZone in your Home.zon file. Here's an example of what that might look like.
```
Zone Home
    IncludedZones
        "Buttons"
        "Track"
        "MasterTrack"
    IncludedZonesEnd
    AssociatedZones
       "SelectedTrackFXMenu"
    AssociatedZonesEnd
ZoneEnd
```

Next, you would need to include the GoSelectedTrackFXMenu action in your Buttons.zon as shown below. GoSelectedTrackFXMenu acts as a toggle, so pressing it once will activate the SelectedTrackFXMenu zone and a second press will exit that zone. Additionally, if you're using a surface+widget that supports feedback, a light will appear when this zone is active.
```
Zone "Buttons"
    AudioTracks                 GoSelectedTrackFXMenu
ZoneEnd
```

### SelectedTrackFXMenu as part of your Home.zon
If you prefer to have SelectedTrackFXmenu on whenever you're in your Home Zone, no additional button will be required to activate the SelectedTrackFXMenu zone. You just need to be sure SelectedTrackFXMenu exists as an IncludedZone in your Home.zon as shown below.
```
Zone Home
    IncludedZones
        "Buttons"
        "Track"
        "SelectedTrackFXMenu"
        "MasterTrack"
    IncludedZonesEnd
ZoneEnd
```