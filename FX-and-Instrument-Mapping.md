

CSI allows users to create custom FX and instrument mappings which can be used across one or more surfaces. FX Zones are similar to normal [[Zones]] in that we are mapping [[Widgets defined in our mst files|Defining Control Surface Capabilities]] to behaviour we want to occur. The major difference is about what that behaviour is. In a [[normal Zone|Zones]] we're binding the widgets to some sort of action in Reaper, whereas for FX Zones, we're binding them to a parameter value in an FX plugin (eg.  VST, VSTi, VST3, VST3i, AU, AUi, etc.)

This section of the Wiki will first focus on how to create FX zone files (I may refer to them as fx.zon files as shorthand) and how to activate those fx.zon files. Note: There is no distinction in CSI or Reaper between instruments or FX so the same principals apply to both.

At a high level, first you create the fx.zon file for each plugin, then you determine how you’d like to activate that fx.zon using CSI. We’ll cover all elements of that process here.

## Creating FX/Instrument Zone Files

### Understanding FX.zon Files
Each plugin you map will require its own unique .zon file which needs to be placed inside your **CSI\Zones\[SurfaceName]\** folder. The name of the .zon file itself can be whatever you’d like, but as a best practice you may want to include the plugin format, plugin name, and manufacturer name. 

When CSI is initialized, it reads all .zon files in your CSI surface folders. This is important to understand because when you create an FX.zon file with Reaper open, you will need to re-initialize CSI by either running the Reaper action “Refresh all surfaces”, or opening the CSI preferences in Reaper and clicking OK, or by restarting Reaper.  

It is highly recommended, but not required, that you create an FXZones subfolder to keep things tidy. So **CSI\Zones\[SurfaceName]\FXZones\**. If you create a large number of fx zones, you can even further break them down by manufacturer such as **CSI\Zones\[SurfaceName]\FXZones\Universal Audio\**. 


### An Example FX.Zon
Let’s begin by reviewing a simple fx.zon file and breaking it down to its component parts, then working backwards as to how we got there. I’m using the UAD Teletronix LA-2A Silver plugin here, mapped to the widgets on a MCU/X-Touch Universal-style device.

```
Zone "VST: UAD Teletronix LA-2A Silver (Universal Audio, Inc.)" "LA2ASlv"
     DisplayUpper1       FixedTextDisplay "Thresh"
     DisplayLower1       FXParamValueDisplay 0
     Rotary1             FXParam 0 
     RotaryPush1         NoAction

     DisplayUpper2       FixedTextDisplay "HF Emph"
     DisplayLower2       FXParamValueDisplay 3
     Rotary2             FXParam 3
     RotaryPush2         NoAction  

     DisplayUpper3       FixedTextDisplay "Output" 
     DisplayLower3       FXParamValueDisplay 1
     Rotary3             FXParam 1
     RotaryPush3         NoAction  

     DisplayUpper4       FixedTextDisplay "CompLim" 
     DisplayLower4       FXParamValueDisplay 2
     Rotary4             NoAction
     RotaryPush4         FXParam 2 [ 0.0 1.0 ]

     DisplayUpper5       FixedTextDisplay "Meter" 
     DisplayLower5       FXParamValueDisplay 4
     Rotary5             FXParam 4 [ 0.0 0.50 1.0 ]
     RotaryPush5         NoAction

     DisplayUpper6       NoAction
     DisplayLower6       NoAction
     Rotary6             NoAction
     RotaryPush6         NoAction

     DisplayUpper7       NoAction 
     DisplayLower7       NoAction 
     Rotary7             NoAction 
     RotaryPush7         NoAction

     DisplayUpper8       NoAction
     DisplayLower8       FXBypassedDisplay
     Rotary8             NoAction
     RotaryPush8         ToggleFXBypass
ZoneEnd

```

The first row shows…

```
Zone "VST: UAD Teletronix LA-2A Silver (Universal Audio, Inc.)" "LA2ASlv"
```

The word Zone is required, followed by the EXACT plugin name as it appears in Reaper. A typo in the plugin name will prevent your map from working. Following the full plugin name, you’ll see “LA2ASlv”: this is the plugin alias and can be whatever you want. This will be read as the FXMenuNameDisplay action in CSI, and can be seen on the FXMenu zone files, which we will get into later on. As a best practice, it’s recommended you create a short alias, but it’s not required. However, if you do not have an alias in your FX zone, you may only see something like “VST: UAD” on your surface, which wouldn’t tell you which UAD plugin was loaded in that slot.

I’ll also jump ahead to the very last row of the .zon file which is ZoneEnd. This is required at the end of every CSI zone file, whether an FX or otherwise.


Next we see:
```
     DisplayUpper1       FixedTextDisplay "Thresh"
     DisplayLower1       FXParamValueDisplay 0
     Rotary1             FXParam 0 
     RotaryPush1         NoAction

```

If your surface has displays, you probably want to label which controls are tied to which FX parameters. So DisplayUpper1 will show the word “Thresh” when this zone is active. This is useful if you have limited character displays and FX Params may have long names. Otherwise, you could alternately have used the FXParamNameDIsplay action as shown below:

```

DisplayUpper1 FXParamNameDisplay 0

```

…this action will attempt to display the full parameter name.

### FX SubZones

## FX Mapping Actions

## Activating FX Zones (FocusedFX, TrackFXMenu, SelectedTrackFXMenu, SelectedTrack)