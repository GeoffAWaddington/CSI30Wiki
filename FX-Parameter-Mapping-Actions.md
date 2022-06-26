CSI allows you to map plugin parameters to your control surface. See [[FX Zones]] for details on how to create a .zon file for your effect or instrument (they're called FX zones, but work equally well for instruments). You'll also want to see [[Generating a Raw FX Zon File|FX Zones#Generating a Raw FX zon file]] for a quick and easy to way to get the FX Param # that you'll need for the mapping. 

## FXParam
Is the CSI action to control a plugin parameter. Let's say we've generated the [[Raw FX Zon File|FX Zones#Generating a Raw FX zon file]] for ReaComp, which looks like this...

```
Zone "VST: ReaComp"
	FXParam 0 "Thresh"
	FXParam 1 "Ratio"
	FXParam 2 "Attack"
	FXParam 4 "Release"
...
ZoneEnd
```

If you want to map these first four parameters to the first four rotary knobs on your surface, you'd use the following syntax (in this case my widget names are RotaryA1 through A4, yours may vary):
```
RotaryA1 FXParam 0
RotaryA2 FXParam 1
RotaryA3 FXParam 2
RotaryA3 FXParam 4
```

Alternatively, if you wanted to include the names (not as displays, that's coming in the next section, but just for easy reference) this is also acceptable...
```
RotaryA1 FXParam 0 "Thresh"
RotaryA2 FXParam 1 "Ratio"
RotaryA3 FXParam 2 "Attack"
RotaryA3 FXParam 4 "Release"
```

...as is this (using trailing comments)...
```
RotaryA1 FXParam 0 //Thresh
RotaryA2 FXParam 1 // Ratio
RotaryA3 FXParam 2 // Attack
RotaryA3 FXParam 4 // Release
```

## FXParamNameDisplay
If you wanted to add in the name of the the Parameter to the corresponding display, that would look like this...
```
DisplayUpperA1 FXParamNameDisplay 0 "Thresh"
RotaryA1 FXParam 0
DisplayUpperA2 FXParamNameDisplay 1 "Ratio"
RotaryA2 FXParam 1
DisplayUpperA3 FXParamNameDisplay 2 "Attack"
RotaryA3 FXParam 2
DisplayUpperA3 FXParamNameDisplay 3 "Release"
RotaryA3 FXParam 4
```

## FXParamValueDisplay
And if you wanted to use one of your displays to show the actual parameter value (e.g. "32ms" or "220hz") on your displays, there is a CSI action for that as well. Combining all three examples together you'd end up with the following:

``` 
DisplayUpperA1 FXParamNameDisplay 0 "Thresh"
DisplayLowerA1 FXParamValueDisplay 0 
RotaryA1 FXParam 0
/
DisplayUpperA2 FXParamNameDisplay 1 "Ratio"
DisplayLowerA2 FXParamValueDisplay 1 
RotaryA2 FXParam 1
/
DisplayUpperA3 FXParamNameDisplay 2 "Attack"
DisplayLowerA3 FXParamValueDisplay 2 
RotaryA3 FXParam 2
/
DisplayUpperA4 FXParamNameDisplay 3 "Release"
DisplayLowerA4 FXParamValueDisplay 3 
RotaryA4 FXParam 4
```

You may notice in the above example I broke up each set of widgets with an empty row with a single / character. This essentially "comments out" the empty line, but will make your .zon file easier to read and troubleshoot later on, so I recommend this as a best practice when grouping together multiple widgets for a single FX parameter. 

## FXNameDisplay
If you want your display to show the FX Name, you could simply add a line to one of your display widgets such as:
```
DisplayUpperA1 FXNameDisplay
```

Which would display on your surface (continuing the example from above with ReaComp):
```
VST: ReaComp (Cockos)
```

## ToggleFXBypass
Use this action in a SelectedTrackFXMenu or TrackFXMenu zone to assign toggling the FX Slot to a button.
```
Zone "TrackFXMenu"
        DisplayUpper|       	FXMenuNameDisplay
        Rotary|             	NoAction
        RotaryPush|         	GoFXSlot
	Mute| 			ToggleFXBypass
        Left	            	TrackFXMenuBank -1
        Right	           	TrackFXMenuBank 1
ZoneEnd
```

## FXBypassedDisplay
If you want to add the FX state ("Enabled" or "Bypass") to the SelectedTrackFXMenu or TrackFXMenu you would utilize the FXBypassedDisplay action as shown below:
```
Zone "TrackFXMenu"
        DisplayUpper|       	FXMenuNameDisplay
        DisplayLower|           FXBypassedDisplay
        Rotary|             	NoAction
        RotaryPush|         	GoFXSlot
	Mute| 			ToggleFXBypass
        Left	            	TrackFXMenuBank -1
        Right	           	TrackFXMenuBank 1
ZoneEnd
```

## ToggleEnableFocusedFXMapping
CSI version 2 will have FocusedFX mapping enabled by default. This means if you have a fx.zon file for a particular FX/instrument plugin, and open the GUI in Reaper, that mapping will become activated on your surface by default. You can toggle this behavior off and on by assigning a button to the ToggleEnableFocusedFXMapping action as shown below:
```
   SomeButton     ToggleEnableFocusedFXMapping
```

But what if you don't want FocusedFXMapping on by default?  Since the default toggle state is "on", you can add "OnInitialization ToggleFocusedFXMapping" to flip it to off when CSI starts up as shown below:
```
Zone Home
OnInitialization ToggleEnableFocusedFXMapping
OnInitialization Broadcast Home SelectedTrackSend SelectedTrackReceive SelectedTrackFXMenu TrackSend TrackReceive TrackFXMenu
OnInitialization Receive Home SelectedTrackSend SelectedTrackReceive SelectedTrackFXMenu TrackSend TrackReceive TrackFXMenu
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

## ToggleEnableFocusedFXParamMapping
Another feature of CSI is the ability to have the last touched (or "focused") FX parameter automatically mapped to a widget. This behavior can be toggled off and on using the ToggleEnableFocusedFXParamMapping action.

Example: you may have this assigned to a button such as:
```
Zone "Buttons"
    User                        ToggleEnableFocusedFXParamMapping
ZoneEnd
```

Then have a fader or widget or even a whole SubZone assigned to the Focused FX Param.
```
Zone "FocusedFXParam"
     Fader1                             FocusedFXParam
     DisplayUpper1                      FocusedFXParamNameDisplay
     DisplayLower1                      FocusedFXParamValueDisplay
     F2                                 LeaveSubZone
ZoneEnd
```

## FXParamRelative
FXParamRelative uses the value from the controller as a delta and adds it to the current parameter value. If the value is negative, that amounts to subtracting it from the current parameter value.

```
SomeWidget    FXParamRelative 4
```