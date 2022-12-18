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

...in this case, I'm using a combination of the Control modifier plus the channel Select button to reset the track volume to unity gain (which is what the 0.716 represents). **Note:** The unity-gain track volume level can vary a bit from Reaper install to Reaper install as it's dependent on the "Volume Fader Range" setting found in Reaper's Preferences -> Appearance -> Track Control Panels. So start with 0.716 and adjust that number up or down as needed to get the fader to reset to 0.0db.

## Exclusive Solo
This suggestion comes from Reaper forum user M4TU. If you want Exclusive Solo functionality (i.e. only one track solo'd at a time), you could do something like this in your TrackZone...

```
Zone "Track"
    Shift+Solo|     ClearAllSolo
    Shift+Solo|     TrackSolo
ZoneEnd
```

## Dedicated TrackNameDisplay for Last Touched Track
If you've got a surface that transmits touch messages and you want a display that will show the name of the track you're touching, you can attach a Fader Touch modifier to the display. But because fader touch messages were designed to override existing functionality, you'd have to add a FixedTextDisplay action wtih an empty string (just 2 empty quotes with a space between), followed by the fader touch syntax with a TrackNameDisplay as shown below.

```
Zone "Track"
    MainDisplay|                   FixedTextDisplay " "
    Fader|Touch+MainDisplay|       TrackNameDisplay
ZoneEnd
```

## Want to See the Active Zone or Mapping In Reaper?
Big thanks to Reaper forum user Manwë for this incredible tip! Would you like to see the active zone name, or even have the active mapping appear on screen within Reaper itself? You can do this by leveraging the “SWS/S&M: Resources - Show image, slot” actions and combining those with custom images.

![CSI-IMAGE-DEMO](https://user-images.githubusercontent.com/52307138/208313310-881d091e-dfc1-44f3-a4d9-25dc9afe7dc8.gif)

Pre-Conditions: You must have SWS extensions installed. 

### Step 1: Increase the Number of Show Image Slots
By default, only 4 “Show Image Slots” exist. So if you want to extend the number of these, you need to open the S&M.ini, locate the line "S&M_SHOW_IMG=___", then change the number, resave the S&M.ini, and restart Reaper. 

### Step 2: Make .png template files for your zone names and/or mappings
Make a PNG template for your zones [you can use pixlr.com] and add them to your Images resources in your SWS Extensions menu. 

To make it easier, Manwë has already provided image files for the most common zone names which can be found here:
[CSI ZONE PNGs.zip](https://github.com/GeoffAWaddington/CSIWiki/files/10254403/CSI.ZONE.PNGs.zip)

### Step 3: Add your images in SWS Extensions
Now that we have images we want to use, we must assign those to the S&M Image Slots in Reaper.

1.	Open Reaper’s Action List
2.	Run the action “SWS/S&M: Open/close Resources window (images)”
3.	Right click on the empty space under the word slot and select “Add Slot”
4.	Locate the image file you’d like to assign to the S&M Show Image Slot 1 action
5.	Repeat this for other slots/images as required

### Step 4: Add the S&M Show Image Slot Actions to Your Zone Activation Buttons
Now you just need to add the images in the corresponding slots to the zone activation buttons (or OnZoneActivation virtual widget) within CSI.

Example: if I have Image Slot 1 assigned to show the Home.zon, and use the F1 button on my surface to go Home, I can do this to make “Home” appear in Reaper whenever I GoHome.
```
Zone "Buttons"

     F1                        GoHome
     F1                        Reaper _S&M_SHOW_IMG1
```

Repeat this as needed for other zones and image slots.

### Step 5: Setup the Window in Reaper as desired
You may want to dock the image window, you may prefer a floating window, you may want it on another screen. May be a good idea to include it in screen sets. 

### What if I Forget My Mapping? Can I Use it for That Too?
Yes, you can also use this to display images for your mapping, including creating a toggle using Cycle Actions to flip between the zone name image and the mapping image as shown below.

![CSI-TOGGLE-DEMO](https://user-images.githubusercontent.com/52307138/208313520-813d0015-51d5-4fba-94d6-6795d827cff9.gif)

To set this type of behavior up, first, make 2 new custom actions for the 2 images you want to use for the selected zone like the following:

```
///First custom action for the Surface mapping image. I named mine "CSI HOME MAPPING--> UNDOCK"

SWS/S&M: Resources - Show image, slot __
Dock/undock currently focused window
```

Second custom action...
```
///Second custom action to return to the docked CSI zone image. I named mine "CSI HOME ZONE-->DOCK"

SWS/S&M: Resources - Show image, slot __
Dock/undock currently focused window
```

Now, create an SWS Cycle Action with toggle enabled like the following:
```
(custom action command ID)          Custom: CSI HOME MAPPING-->UNDOCK
!                                   -----Step-----
(custom action command ID)          Custom: CSI HOME ZONE-->DOCK
```

Dock your Zone image where you'd like it, map your Cycle Action to a surface button in your zone and press, resize/locate your Mapping image where you'd like it and done!

The only downside is you would need to have the Zone image docked before activating another zone. Otherwise the Zone image and projection image are swapped. Quick fix though!
