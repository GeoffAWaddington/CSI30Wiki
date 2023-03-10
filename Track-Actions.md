# Track Actions
The following CSI actions are available for controlling Reaper's track controls. They will receive their context from the Zone Type, whether a "Track" zone (used for controlling multiple channels) or "SelectedTrack" zone.

### Tracks
* [[TrackVolume|Track-Actions#trackvolume-trackvolumedisplay]]
* [[SoftTakeover7BitTrackVolume|Track-Actions#softtakeover7bittrackvolume]]
* [[SoftTakeover14BitTrackVolume|Track-Actions#softtakeover14bittrackvolume]]
* [[TrackPanAutoLeft|Track-Actions#trackpanautoleft-trackpanautoright-trackpanautoleftdisplay-trackpanautorightdisplay]]
* [[TrackPanAutoRight|Track-Actions#trackpanautoleft-trackpanautoright-trackpanautoleftdisplay-trackpanautorightdisplay]]
* [[TrackPan|Track-Actions#trackpan-trackpandisplay]]
* [[TrackPanWidth|Track-Actions#trackpanwidth-trackpanwidthdisplay]]
* [[TrackPanL|Track-Actions#trackpanl-trackpanldisplay]]
* [[TrackPanR|Track-Actions#trackpanr-trackpanrdisplay]]
* [[TrackSelect|Track-Actions#trackselect]]
* [[TrackUniqueSelect|Track-Actions#trackuniqueselect]]
* [[TrackRangeSelect|Track-Actions#trackrangeselect]]
* [[TrackSolo|Track-Actions#tracksolo]]
* [[TrackMute|Track-Actions#trackmute]]
* [[TrackRecordArm|Track-Actions#trackrecordarm]]
* [[TrackInvertPolarity|Track-Actions#trackinvertpolarity]]
* [[CycleTrackInputMonitor|Track-Actions#cycletrackinputmonitor]]
* [[TrackInputMonitorDisplay|Track-Actions#trackinputmonitordisplay]]
* [[TrackNameDisplay|Track-Actions#tracknamedisplay]]
* [[TrackNumberDisplay|Track-Actions#tracknumberdisplay]]
* [[TrackVolumeDisplay|Track-Actions#trackvolume-trackvolumedisplay]]
* [[TrackPanAutoLeftDisplay|Track-Actions#trackpanautoleft-trackpanautoright-trackpanautoleftdisplay-trackpanautorightdisplay]]
* [[TrackPanAutoRightDisplay|Track-Actions#trackpanautoleft-trackpanautoright-trackpanautoleftdisplay-trackpanautorightdisplay]]
* [[TrackPanDisplay|Track-Actions#TrackPan-trackpandisplay]]
* [[TrackPanWidthDisplay|Track-Actions#trackpanwidth-trackpanwidthdisplay]]
* [[TrackPanLeftDisplay|Track-Actions#trackpanl-trackpanleftdisplay]]
* [[TrackPanRightDisplay|Track-Actions#trackpanr-trackpanrightdisplay]]
* [[TrackOutputMeter|Track-Actions#trackoutputmeter]]
* [[TrackOutputMeterAverageLR|Track-Actions#trackoutputmeteraveragelr]]
* [[TrackOutputMeterMaxPeakLR|Track-Actions#trackoutputmetermaxpeaklr]]

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

## TrackPanAutoLeft, TrackPanAutoRight, TrackPanAutoLeftDisplay, TrackPanAutoRightDisplay
TrackPanAutoLeft will control TrackPan or TrackPanL (if using Dual Pans). TrackPanAutoRight will control TrackPanWidth or TrackPanR (if using Dual Pans). This adds considerable convenience in that you can use Stereo Balance Pans or Dual Pans even in the same Reaper project and control them in CSI without having to change zones. The one difference is that the WidgetMode for TrackPanAutoRight must be fixed (i.e. you can't use the Spread mode for PanWidth and Dot mode for PanR - you have to pick one). **Note:** in the below example, the [[ToggleChannel|Other Actions#togglechannel]] action is being used to flip between TrackPanAutoLeft and TrackPanAutoRight on the same widgets without changing zones.
```
Zone "Track"
    RotaryPush|                 ToggleChannel

    Rotary|                     TrackPanAutoLeft
    Rotary|			WidgetMode Dot
    Toggle+Rotary|              TrackPanAutoRight
    Toggle+Rotary|		WidgetMode Dot

    DisplayLower|      		TrackPanAutoLeftDisplay
    Toggle+DisplayLower|   	TrackPanAutoRightDisplay
ZoneEnd
```

**Note:** When using Dual Pans, TrackL and TrackR automation does not get written from a control surface. This appears to require a change to the Reaper API's. 

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

Here's TrackNameDisplay used in a typical "Track" zone.
```
Zone "Track"
     DisplayUpper|     TrackNameDisplay
ZoneEnd
```

In the below example, we've got a display widget called "MainDisplay" and want to assign the selected track's name to always appear on that widget. To do this, we can create a SelectedTrack.zon and simply add the following... 
```
Zone "SelectedTrack"
     MainDisplay     TrackNameDisplay
ZoneEnd
```

## TrackNumberDisplay
Use TrackNumberDisplay on a display widget to display the Reaper track # on your surface. In the below example, we've got a surface that has 4 display rows (FaderPort8/16, SCE-24 or an OSC device). The track number is being displayed on the second row of the display (DisplayUpperMiddle). 
```
Zone "Track"
    DisplayUpper|          TrackNameDisplay
    DisplayUpperMiddle|    TrackNumberDisplay
    DisplayLowerMiddle|    TrackPanDisplay
    DisplayLower|          TrackVolumeDisplay
ZoneEnd
```

That's to [Navelpluisje](https://forum.cockos.com/member.php?u=139512) for contributing this action!

## TrackOutputMeter
Use this action if your surface has two columns of meter LEDs (one for left and one for right channel). By default, CSI will display the left channel (0) on a mono meter.
```
Zone "Track"
     VUMeter|      TrackOutputMeter
ZoneEnd
```

If you have a surface where the meter has right and left channels, you would break those up into two widgets in your .mst and in your Track zone, append the channel number (0=Left, 1=Right) to the end of the TrackOutputMeter action as shown below.
```
Zone "Track"
     VUMeterLeft|      TrackOutputMeter 0
     VUMeterRight|     TrackOutputMeter 1
ZoneEnd
```
If you have more than just left and right channels (e.g. surround) you can use TrackOutputMeter 2, TrackOutputMeter 3, etc.

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