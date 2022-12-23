## Overview: Four Different Activation Methods:
Once you’ve created some fx.zon files, the next step is determining how you want that fx.zon to be activated. CSI offers 4 methods for activating fx zones:

* **FocusedFX:** Opening the GUI in Reaper will cause the plugin to map on your surface. This mode is enabled by default. **Mapping action: ToggleEnableFocusedFXMapping**

* **TrackFXMenu:** You select the plugin you want to activate from a menu on your control surface. TrackFXMenu splays the FX slots out vertically, so imagine an 8-fader surface with displays showing FX Slot 1 on Track 1, FX Slot 1 on Track 2, FX Slot 1 on Track 3, etc. You change FX slots by banking vertically. **Mapping action: GoTrackFXMenu** to activate the zone, **GoFXSlot** to activate the FX mapping.

* **SelectedTrackFXMenu:** Like TrackFXMenu, you select the plugin you want to activate from a menu on your control surface. SelectedTrackFXMenu splays the FX slots of the selected track horizontally. Here, imagine an 8-fader surface with displays. You've got track 3 selected in Reaper. SelectedTrackFXMenu will show FX Slot 1 on Track 3, FX Slot 2 on Track 3, FX Slot 3 on Track 3, etc. spread out across the 8 channels on your surface. If you have more FX on the selected track than you have control surface channels for, you can change FX slots by banking horizontally. **Mapping action: GoSelectedTrackFXMenu** to activate the zone, **GoFXSlot** to activate the FX mapping.

* **SelectedTrackFX:** When this mode is enabled, any fx.zon's on the selected track will become active. This means multiple FX can be mapped at the same time. This is particularly handy for control surfaces with pre-defined layouts like the Softube Console One - or someone seeking to replicate a similar workflow. With this mode, because multiple FX can be activated at once (example: an EQ and a compressor), users would have to be careful when mapping their fx.zon files as to avoid conflicts. **Mapping action: GoSelectedTrackFX**

**Tip:** You can use different FX activation methods on the same surface and use the various mapping actions to dictate which method is activate at any given time. Example: you can have a TrackFXMenu and a SelectedTrackFXMenu that you use a button to switch between, while also having another button toggling FocusedFXMapping as needed.

## FocusedFXMapping
CSI version 2 has FocusedFX mapping enabled by default. This means if you have a fx.zon file for a particular FX/instrument plugin, and open the GUI in Reaper, that mapping will become activated on your surface by default. You can toggle this behavior off and on by assigning a button to the ToggleEnableFocusedFXMapping action as shown below:
```
   SomeButton     ToggleEnableFocusedFXMapping
```

But what if you don't want FocusedFXMapping on by default?  Since the default toggle state is "on", you can add "OnInitialization ToggleFocusedFXMapping" to your Home.zon to toggle the state to off when CSI starts up as shown in the example below:
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
The key things to call out here is that we are using the **FXMenuNameDisplay** action to show the name of the plugin (or the alias if one exists) on the control surface and the **GoFXSlot** action is assigned to the corresponding RotaryPush to actually activate the FX. The **TrackFXMenuBank** actions are used to bank between the different FX slots.

## How to activate the TrackFXMenu.zon
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
The key things to call out here is that we are using the **FXMenuNameDisplay** action to show the name of the plugin (or the alias if one exists) on the control surface and the **GoFXSlot** action is assigned to the corresponding RotaryPush to actually activate the FX. The **SelectedTrackFXMenuBank** actions are used to bank between the different FX slots. Additionally, I've added an SWS extension action to actually open the plugin GUI when the FX is activated. If you don't want this behvaior, you can remove that line. Note: that action only works on a SelectedTrackFXMenu zone and not on TrackFXMenu.

## How to activate the SelectedTrackFXMenu.zon
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

## SelectedTrackFX
SelectedTrackFX is designed to automatically call up any FX.zon's simply by selecting a track in Reaper. This is especially useful for surface's like Softube's Console One or any other surface where you always want one or more track FX mapped to specifics widgets on your hardware. The key to making this approach work is pre-planning because you want to avoid the types of conflicts that could occur if you have two or more FX on the selected channel mapped to the same widget on your control surface. But if you're using a Channel Strip plugin on every channel and want specific EQ or compressor settings to automatically map to your control surface, this may be the approach for you.

You have one of two options for activating SelectedTrackFX: 1) assigning GoSelectedTrackFX to a button, or 2) assigning GoSelectedTrackFX to automatically map on Track Selection. I'll show both methods below.

Mapping SelectedTrackFX to a button is as simple as:
```
Zone "Buttons"
     F1     GoSelectedTrackFX
ZoneEnd
```

If you'd prefer to have that automatically occur whenever you select a track in Reaper, add this action to your Home.zon like this:
```
Zone "Home"
OnInitialization        ToggleEnableFocusedFXMapping 
OnTrackSelection 	GoSelectedTrack
OnTrackSelection 	GoSelectedTrackFX
        AssociatedZones
           "SelectedTrack"
        AssociatedZonesEnd
ZoneEnd
```
Note: in the above example how I'm using OnInitilization ToggleEnableFocusedFXMapping to turn off FcusedFXMapping by default. 

Just to illustrate an example using Softube's Console One for instance, one could have a SelectedTrack zone assigning some of the widgets to your surface to the selected track's controls in Reaper...
```
Zone "SelectedTrack"
	InputMeterLeft 		TrackOutputMeter "0"
	InputMeterRight		TrackOutputMeter "1"
	OutputMeterLeft 	TrackOutputMeter "0"
	OutputMeterRight	TrackOutputMeter "1"
	Volume 			TrackVolume
	Control+Volume 		TrackPanWidth	"1"
	Shift+Volume 		TrackPan 	"0"
	Option+Volume 		FocusedFXParam
	Mute 			TrackMute
	Solo 			TrackSolo
ZoneEnd
```

...while also having a "compressor" map active on different widgets...
```
Zone "VST: UAD Teletronix LA-2A Silver (Universal Audio, Inc.)" "LA2ASlv"
	Threshold 		FXParam "0"
	Output	 		FXParam "1"
	Meter 			FXParam "4"
	Attack 			FXParam "3"
	Ratio 			FXParam "2"
	InvertFB+Compressor 	FXParam "6" "Bypass" [ 0.0 1.0 ]
	WetDry			FXParam "7"
ZoneEnd
```

...at the same time as an EQ map on even different widgets...
```
Zone "VST: UAD Pultec EQP-1A (Universal Audio, Inc.)" "EQP1A"
	HiGain 			FXParam "7" "HF Atten"
	HiFrequency 		FXParam "6" "HF Atten Freq"
	HiMidGain 		FXParam "4" "HF Boost"
	HiMidFrequency 		FXParam "3" "High Freq"
	HiMidQ 			FXParam "5" "HF Q"
	LoMidGain 		FXParam "2" "LF Atten"
	LoMidFrequency 		FXParam "0" "Low Freq"
	LoGain 			FXParam "1" "LF Boost"
	LoFrequency 		FXParam "0" "Low Freq"
	InvertFB+Equalizer	FXParam "10" "Bypass" [ 0.0 1.0 ]
	Parallel 		FXParam "11" "Wet"
ZoneEnd
```

...and everything works in tandem as long as there are no conflicting widget assignments and there's one track selected in Reaper.

## Unmapping FX zones
Now that you've got your fx.zon activated, what is the right way unmap/deactivate that fx.zon?

* **FocusedFX:** Simply unfocus the FX in question in Reaper. Note: if you unfocus an FX then refocus the same FX, CSI may not pick it up the second time. You may need to focus another FX first.

* **TrackFXMenu:** Unmapping actions are either **GoTrackFXMenu** or **GoHome** depending on whether you are trying to return to the FX menu or trying to return to the Home zone.

* **SelectedTrackFXMenu:** Unmapping actions are either **GoSelectedTrackFXMenu** or **GoHome** depending on whether you are trying to return to the FX menu or trying to return to the Home zone.

* **SelectedTrackFX:** Select another track or deselect the track. 