# Track Actions
The following CSI actions are available for controlling Reaper's track controls. They will receive their context from the Zone Type, whether a "Track" zone (used for controlling multiple channels) or "SelectedTrack" zone.

* [[TrackVolume|Track-Actions#TrackVolume]]
* [[SoftTakeover7BitTrackVolume]]
* [[SoftTakeover14BitTrackVolume]]
* [[MCUTrackPan]]
* [[TrackPan]]
* [[TrackPanWidth]]
* [[TrackPanL]]
* [[TrackPanR]]
* [[TrackSelect]]
* [[TrackUniqueSelect|TrackSelect]]
* [[TrackRangeSelect|TrackSelect]]
* [[TrackSolo]]
* [[TrackMute]]
* [[TrackInvertPolarity]]
* [[TrackRecordArm]]
* [[TrackNameDisplay]]
* [[TrackVolumeDisplay]]
* [[MCUTrackPanDisplay|MCUTrackPan]]
* [[TrackPanDisplay]]
* [[TrackPanWidthDisplay]]
* [[TrackPanLeftDisplay]]
* [[TrackPanRightDisplay]]
* [[TrackOutputMeter]]
* [[TrackOutputMeterAverageLR|TrackOutputMeter#TrackOutputMeterAverageLR]]
* [[TrackOutputMeterMaxPeakLR|TrackOutputMeter#TrackOutputMeterMaxPeakLR]]

## TrackVolume
TrackVolume is the most commonly used CSI action for controlling TrackVolume. Use this for surfaces with motorized faders or encoders. TrackVolumeDisplay will display the fader volume on your surface. The example shown below shows the syntax for TrackVolumeDisplay could be used with touch sensitive faders to only show the track volume when touching the faders (you could show the Pan level in that display the rest of the time).  
```
Zone "Track"
    Fader|Touch+DisplayLower|     TrackVolumeDisplay
    Fader|                        TrackVolume
ZoneEnd
```

If you only wanted to see the TrackVolume on the lower display at all times, you could just remove the Fader|Touch portion and have this.
```
Zone "Track"
    DisplayLower|     TrackVolumeDisplay
    Fader|            TrackVolume
ZoneEnd
```
## SoftTakeover7BitTrackVolume
If you are assigning TrackVolume to a 7-Bit MIDI fader or an absolute rotary [a knob with defined start and end positions] you can consider using CSI's SoftTakeover7BitTrackVolume in lieu of TrackVolume in order to avoid volume jumps when you move a fader. The "soft takeover" variant will only change the volume once the fader/knob's value passes through the current value in Reaper.
```
Zone "Track"
     Fader|      SoftTakeover7BitTrackVolume
ZoneEnd
```

## SoftTakeover14BitTrackVolume
Same as SoftTakeover7BitTrackVolume, only designed for 14-bit MIDI faders/knobs.
```
Zone "Track
     Fader|      SoftTakeover14BitTrackVolume
ZoneEnd
```

## MCUTrackPan, MCUTrackPanDisplay
MCUTrackPan is meant to allow you to toggle between TrackPan and TrackPanWidth from a single CSI action. The two lines of code [I've included the syntaxs for MCUTrackPanDisplay for convenience) control Pan and PanWidth, as well as Pan L and Pan R if you prefer to use Dual Pan. No zone switching, multiple widgets, or special rotary metering modes required.
```
Zone "Track"
     DisplayLower|     MCUTrackPanDisplay Rotary|
     Rotary|           MCUTrackPan  
ZoneEnd
```

## TrackPan, TrackPanDisplay
Use TrackPan for controlling the TrackPan in Reaper. TrackPanDisplay will display the value of the TrackPan action on your surface.
```
Zone "Track"
     DisplayLower|      TrackPanDisplay
     Rotary|            TrackPan
ZoneEnd
```

## TrackPanWidth, TrackPanWidthDisplay
TrackPanWidth is used for controlling the PanWidth control in Reaper. 
```
Zone "Track"
     Shift+DisplayLower|      TrackPanWidthDisplay
     Shift+Rotary|            TrackPanWidth
ZoneEnd
```

## TrackPanL
TrackPanL controls the Left channel's pan position when using the Dual Pan option in Reaper. Note: there is a known limitation where automation is not written when using this CSI action.
```
Zone "Track"
     Alt+Rotary|     TrackPanL
ZoneEnd
```

## TrackPanR
TrackPanR controls the Right channel's pan position when using the Dual Pan option in Reaper. Note: there is a known limitation where automation is not written when using this CSI action.
```
Zone "Track"
     Control+Rotary|     TrackPanR
ZoneEnd
```

## TrackSelect
Use this action in a Track zone context to select one of the channels in Reaper.
```
Zone "Track"
     Select|     TrackSelect
ZoneEnd
```

## TrackUniqueSelect
This action works similar to TrackSelect, but will deselect any other previously selected tracks.
```
Zone "Track"
     Shift+Select|     TrackUniqueSelect
ZoneEnd
```


## TrackRangeSelect
Use this action to select the start and end of a range of tracks and have them all selected.
```
Zone "Track"
     Control+Select|     TrackRangeSelect
ZoneEnd
```