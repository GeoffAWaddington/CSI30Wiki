****# Automation Actions

* [[TrackAutoMode|Automation-Actions#trackautomode]]
* [[GlobalAutoMode|Automation-Actions#globalautomode-globalautomodedisplay]]
* [[CycleTrackAutoMode|Automation-Actions#cycletrackautomode]]
* [[TrackAutoModeDisplay|Automation-Actions#trackautomodedisplay]]
* [[GlobalAutoModeDisplay|Automation-Actions#globalautomode-globalautomodedisplay]]

## TrackAutoMode
Use the CSI Action TrackAutoMode to assign a button to each of Reaper's Track Automation Modes. This action is designed to work on the selected track(s) without the need for a navigator, allowing it to work in a typical "Buttons" zone.

Here is an example of buttons mapped to each of the 5 automation modes in Reaper.

```` 
Zone "Buttons"
     Trim            TrackAutoMode 0      // Trim
     Read            TrackAutoMode 1      // Read
     Touch           TrackAutoMode 2      // Touch
     Write           TrackAutoMode 3      // Write
     Latch           TrackAutoMode 4      // Latch
     Alt+Latch       TrackAutoMode 5 	  // LatchPreview
ZoneEnd
```` 

## GlobalAutoMode, GlobalAutoModeDisplay
GlobalAutoMode is used to set Reaper's Global Automation override mode. You may want to use a modifier and put it in the same Zone as your other automation buttons as shown in the example below.

```
Zone "Buttons"
     Trim            TrackAutoMode 0      // Trim
     Read            TrackAutoMode 1      // Read
     Touch           TrackAutoMode 2      // Touch
     Write           TrackAutoMode 3      // Write
     Latch           TrackAutoMode 4      // Latch
     Alt+Latch       TrackAutoMode 5 	  // Latch Preview

     Shift+Trim      GlobalAutoMode 0     // Global automation override off
     Shift+Read      GlobalAutoMode 1     // Global Read
     Shift+Touch     GlobalAutoMode 2     // Global Touch
     Shift+Write     GlobalAutoMode 3     // Global Write
     Shift+Latch     GlobalAutoMode 4     // Global Latch
     Shift+Alt+Latch GlobalAutoMode 5     // Global Latch Preview
ZoneEnd
```

If you wanted to dedicate a display to showing the global auomation mode within Reaper (example: on an OSC device), there is now a CSI action that will display that.
```
Zone "Buttons"
     AutoModeDisplay     GlobalAutoModeDisplay
ZoneEnd
```
## CycleTrackAutoMode
Use the CSI action CycleTrackAutoMode when you're looking to cycle through the various automation modes in Reaper. **Note:** "Write" mode is left out by design in order to prevent accidental writing or over-writing of automation while cycling through modes.

Here Shift+RecordArm will cycle through the various automation modes.
```` 
	Shift+RecordArm|	CycleTrackAutoMode
```` 

## TrackAutoModeDisplay
TrackAutoModeDisplay can be combined with a TrackNavigator or SelectedTrackNavigator to show you the current automation mode. In this example, pressing Shift+RecordArm on a given channel will cycle through the automation mode on the selected track and the corresponding display will tell us which mode it's in.

```
Zone "Track"
    Shift+RecordArm|        CycleTrackAutoMode
    Shift+DisplayLower|     TrackAutoModeDisplay
ZoneEnd
```