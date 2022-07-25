CSI offers the ability to spill VCA's onto your multi-fader surfaces. We won't get into how to create VCA's in Reaper other than to say there are plenty of videos and other resources on that topic like this one:

https://www.youtube.com/watch?v=JZzR7-KSQMU&t

## What is VCA spill? 
One common mixing workflow involves creating multiple VCA faders to control the levels of entire groups of tracks. For instance, you may have VCA faders for each of your orchestral sections, or maybe one for drums, another for bass tracks, another for guitars, lead vocals, background vocals, etc. VCA's can be a great way to manage projects with large track counts and simplify mixing by minimizing the visible number of channels on your control surface.

But let's say you're mixing using VCA's and you've got your Drum VCA but you still want to tweak the relative levels of the kick and snare drums, how do you quickly make those tweaks? You can use the VCA Spill feature of CSI to "spill out" the tracks feeding that VCA to the other channels on the surface, similar to opening and closing a folder.

## How to Use VCA's and VCA Spill
The process is basically: 1) press a button you've assigned on your surface to enable VCA mode, where your surface will only show VCA tracks, then 2) press another button to "spill" (expand) the tracks feeding a particular VCA fader out to the rest of the surface.

## ToggleVCAMode
Use the CSI action ToggleVCAMode to only show VCA faders on your surface. All other types of tracks will be hidden. This is of course a toggle, so pressing the widget a second time will return to the full track view.

In this example, my F1 widget has been assigned to ToggleVCAMode in the .zon file for my surface:
```` 
Zone "Buttons|"
	Shift 		Shift
	Alt		Alt
	Option		Option
	Control		Control
	F1 		ToggleVCAMode
ZoneEnd
```` 

## TrackToggleVCASpill
Once VCA Mode is enabled, you can assign a CSI action to spill the faders for any selected VCA fader. You probably want to use a modifier in addition to your channel select button to do this. 

In the below example, we're using Alt+Select to toggle spilling the tracks feeding the VCA fader.
```` 
Zone "Track"
     Alt+Select|      TrackToggleVCASpill
ZoneEnd
```` 

So if I have "Drums VCA" fader with all my drum tracks, entering VCA Mode (by pressing the F1 widget like in the above example) will show me only the VCA tracks, then Alt+Selecting the Drum VCA will show me all the drum tracks controlled by that particular VCA fader. To hide those tracks, I'd simply Alt+Select the Drum VCA again. To exit VCA mode, I'd simple toggle VCA mode again.

You can also spill multiple levels of VCAs.

## CycleTrackVCAFolderModes, TrackVCAFolderModeDisplay
On an MCU or X-Touch TrackVCAFolderModeDisplay will display the overall 'mode' CSI is currently in, on the LED display labelled 'Assignment' immediately to then left of the SMPTE/Beats indicators. On an X-Touch, it's immediately to the left of the master solo indicator.

CycleTrackVCAFolderModes will cycle through the various modes: 1) Normal (i.e. regular track display); 2) VCA (VCA leaders); or 3) Folder (top level folders).


    nameValue                 CycleTrackVCAFolderModes
    AssignmentDisplay         TrackVCAFolderModeDisplay