## You Can Chain Actions For CSI "Macros"
Want to run a few actions in a set sequence? Just create multiple lines in your .zon files with the actions in the order you want. CSI will trigger each action in the order from the .zon file similar to running a custom action (or macro). In this example, holding select will 1) select the track, 2) toggle the mixer setting to show/hide children in folders (which would only apply if a parent folder track), and 3) toggle VCA spill (which would only apply if a VCA master track).

```
     Hold+Select|                       TrackUniqueSelect
     Hold+Select|                       Reaper 41665             //Mixer: Toggle show tracks in folders in mixer
     Hold+Select|                       TrackToggleVCASpill
```

## "Refresh All Surfaces" Reaper Action to Test Changes to Your .zon Files
If you're creating or modifying .zon files with Reaper open, and want to check out the changes, CSI will need to rescan the files. The most efficient way to do this (i.e. without needing to close and restart Reaper) is to run the Reaper action:

```
Control surface: Refresh all surfaces
```

...this will allow CSI's parser to rescan your .zon files. Just remember to actually save the changes to your .zon files before you run it (speaking from experience)!

## Cycling Through Hardware Inputs From Your Control Surface
Nos402 from the Reaper forums posted this trick which allows you to set the hardware input on your channel using SWS Cycle Actions. To do this, first setup a new SWS Cycle Action that looks like the one shown below. You basically add a row, right click it, click Insert statement -> REACONSOLE, then type in i1 if you want input 1, and i4 if you want input 4, etc. If you want a stereo pair, use i1s for stereo pair 1/2, and i3s for stereo pair 3/4. Be sure to "Add Step" in between each REACONSOLE command. 

![](https://i.imgur.com/4YF68mH.png)

Save that cycle action, and locate it in Reaper's Action List. Select it and Copy the Command ID. Now, we just have to add it to your [surface].zon file inside a zone with a selected track navigator similar to what's shown below. In my case, I'm using a one channel surface and have this linked to the Control modifier when used with the RecordArm button. Your Cycle Action number will likely differ than mine so be sure to copy yours from the Reaper Action List:

```
Zone "SelectedChannel"
     SelectedTrackNavigator
     Control+RecordArm1                 Reaper _S&M_CYCLACTION_12
ZoneEnd
```

That's it!

## Centering Pan and FX Input/Output Gain Parameters
You can use stepped parameters, or rather, a single-step parameter to center your pans or reset things like input/output gain on plugins. In the below example, I've got Input and Output Gain (plugin parameters 27 and 28 respectively) mapped to widgets RotaryB5 and B6 respectively. Now, in case I ever move them by accident or want to quickly reset them back to 0db, I'm utilizing the RotaryPush press with a single-step value of "[ 0.5 ]" to center the Input and Output gain parameters of this plugin.

```
DisplayUpperB5 FXParamNameDisplay 27 "Input"
DisplayLowerB5 FXParamValueDisplay 27 
RotaryB5 FXParam 27
RotaryPushB5 FXParam 27 [ 0.5 ]
Property+RotaryPushB5 NoFeedback   
/  
DisplayUpperB6 FXParamNameDisplay 28 "Output"
DisplayLowerB6 FXParamValueDisplay 28 
RotaryB6 FXParam 28
RotaryPushB6 FXParam 28 [ 0.5 ]
```

The same would work for pan as shown below...
```
Rotary|               TrackPan
Shift+RotaryPush|     TrackPan [ 0.5 ]
```

...this tip could of course be expanded to any plugin parameter.

## Resetting Track Volume
Similar to the pan reset trick above, you can reset the Track Volume of a channel to unity gain by doing something like this:

```
Control+Select      TrackVolume 	 [ 0.716 ]
```

...in this case, I'm using a combination of the Control modifier plus the channel Select button to reset the track volume to unity gain (which is what the 0.716 represents).

## Exclusive Solo
This suggestion comes from Reaper forum user M4TU. If you want Exclusive Solo functionality (i.e. only one track solo'd at a time), you could do something like this in your TrackZone...

```
Zone "Track"
    Shift+Solo|     ClearAllSolo
    Shift+Solo|     TrackSolo
ZoneEnd
```