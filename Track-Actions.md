# Track Actions
The following CSI actions are available for controlling Reaper's track controls. They will receive their context from the Zone Type, whether a "Track" zone (used for controlling multiple channels) or "SelectedTrack" zone.

* [[TrackVolume|Track-Actions#TrackVolume-TrackVolumeDisplay]]
* [[SoftTakeover7BitTrackVolume|Track-Actions#SoftTakeover7BitTrackVolume]]
* [[SoftTakeover14BitTrackVolume|Track-Actions#SoftTakeover14BitTrackVolume]]
* [[MCUTrackPan|Track-Actions#MCUTrackPan-MCUTrackPanDisplay]]
* [[TrackPan|Track-Actions#TrackPan-TrackPanDisplay]]
* [[TrackPanWidth|Track-Actions#TrackPanWidth-TrackPanWidthDisplay]]
* [[TrackPanL|Track-Actions#TrackPanL-TrackPanLDisplay]]
* [[TrackPanR|Track-Actions#TrackPanR-TrackPanRDisplay]]
* [[TrackSelect|Track-Actions#TrackSelect]]
* [[TrackUniqueSelect|Track-Actions#TrackUniqueSelect]]
* [[TrackRangeSelect|Track-Actions#TrackRangeSelect]]
* [[TrackSolo|Track-Actions#TrackSolo]]
* [[TrackMute|Track-Actions#TrackMute]]
* [[TrackRecordArm|Track-Actions#TrackRecordArm]]
* [[TrackInvertPolarity|Track-Actions#TrackInvertPolarity]]
* [[TrackNameDisplay|Track-Actions#TrackNameDisplay]]
* [[TrackVolumeDisplay|Track-Actions#TrackVolume-TrackVolumeDisplay]]
* [[MCUTrackPanDisplay|Track-Actions#MCUTrackPan-MCUTrackPanDisplay]]
* [[TrackPanDisplay|Track-Actions#TrackPan-TrackPanDisplay]]
* [[TrackPanWidthDisplay|Track-Actions#TrackPanWidth-TrackPanWidthDisplay]]
* [[TrackPanLeftDisplay|Track-Actions#TrackPanL-TrackPanLeftDisplay]]
* [[TrackPanRightDisplay|Track-Actions#TrackPanR-TrackPanRightDisplay]]
* [[TrackOutputMeter|Track-Actions#TrackOutputMeter]]
* [[TrackOutputMeterAverageLR|Track-Actions#TrackOutputMeterAverageLR]]
* [[TrackOutputMeterMaxPeakLR|Track-Actions#TrackOutputMeterMaxPeakLR]]

## TrackVolume, TrackVolumeDisplay
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
     Rotary|            TrackPan 0
ZoneEnd
```

Now, notice the number after TrackPan...The numbers are for the LED ring displays on the MCU style encoders:

* 0 means a single led -- perfect for Pan
* 1 means fill from right edge - centre single -- fill to left edge perfect for Width or EQ boost/cut
* 2 - left right fill -- good for level
* 3 - spread -- good for Q

## TrackPanWidth, TrackPanWidthDisplay
TrackPanWidth is used for controlling the PanWidth control in Reaper. 
```
Zone "Track"
     Shift+DisplayLower|      TrackPanWidthDisplay
     Shift+Rotary|            TrackPanWidth 1
ZoneEnd
```

## TrackPanL, TrackPanLeftDisplay
TrackPanL controls the Left channel's pan position when using the Dual Pan option in Reaper. Note: there is a known limitation where automation is not written when using this CSI action.
```
Zone "Track"
     Alt+DisplayLower|     TrackPanLeftDisplay
     Alt+Rotary|           TrackPanL
ZoneEnd
```

## TrackPanR, TrackPanRightDisplay
TrackPanR controls the Right channel's pan position when using the Dual Pan option in Reaper. Note: there is a known limitation where automation is not written when using this CSI action.
```
Zone "Track"
     Control+DisplayLower|     TrackPanRightDisplay
     Control+Rotary|           TrackPanR
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

## TrackSolo
TrackSolo will toggle the Solo state of the track. This action provides feedback of the solo state to the surface.
```
Zone "Track"
     Solo|     TrackSolo
ZoneEnd
```

## TrackMute
TrackSolo will toggle the Solo state of the track. This action provides feedback of the mute state to the surface.
```
Zone "Track"
     Mute|     TrackMute
ZoneEnd
```

## TrackRecordArm
Use TrackRecordArm to toggle the "Armed" state of the track. This action provides feedback to the surface.
```
Zone "Track"
     RecordArm|     TrackRecordArm
ZoneEnd
```

## TrackInvertPolarity
TrackInvertPolarity will invert the polarity of the track. This action provides feedback to the surface. In this example, because most surfaces do not have a dedicated button for this, I'm using the Shift modifier combined with the RecordArm button to fire the action.
```
Zone "Track"
     Shift+RecordArm|     TrackInvertPolarity
ZoneEnd
```

## TrackNameDisplay
Use TrackNameDisplay on a display widget to show the name of the track in question. Note: if a track is not named in Reaper, it will display "Track 1", "Track 2", etc. On a surface with only 8 characters, "Track 10" may be truncated to just "Track 1". Workaround: properly name your tracks!
```
Zone "Track"
     DisplayUpper|     TrackNameDisplay
ZoneEnd
```

## TrackOutputMeter
Use this action if your surface has two columns of meter LEDs (one for left and one for right channel).
```
Zone "Track"
     VUMeter|      TrackOutputMeter
ZoneEnd
```

## TrackOutputMeterAverageLR
Use this action if your surface has a single column LED for metering and you want that meter to show the average of both the left and right channels.
```
Zone "Track"
     VUMeter|      TrackOutputMeterAverageLR
ZoneEnd
```

## TrackOutputMeterMaxPeakLR
Use this action if your surface has a single column LED for metering and you want that meter to show the highest peak value of both the left and right channels.
```
Zone "Track"
     VUMeter|      TrackOutputMeterMaxPeakLR
ZoneEnd
```