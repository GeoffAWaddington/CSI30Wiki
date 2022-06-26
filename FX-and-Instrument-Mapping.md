CSI allows users to create custom FX and instrument mappings which can be used across one or more surfaces. FX Zones are similar to normal [[Zones]] in that we are mapping [[Widgets defined in our mst files|Defining Control Surface Capabilities]] to behaviour we want to occur. The major difference is about what that behaviour is. In a [[normal Zone|Zones]] we're binding the widgets to some sort of action in Reaper, whereas for FX Zones, we're binding them to a parameter value in an FX plugin (eg.  VST, VSTi, VST3, VST3i, AU, AUi, etc.)

This section of the Wiki will first focus on how to create FX zone files (I may refer to them as fx.zon files as shorthand) and how to activate those fx.zon files. Note: There is no distinction in CSI or Reaper between instruments or FX so the same principals apply to both.

At a high level, first you create the fx.zon file for each plugin, then you determine how you’d like to activate that fx.zon using CSI. We’ll cover all elements of that process here.

## Creating FX/Instrument Zone Files

### Understanding FX.zon Files
Each plugin you map will require its own unique .zon file which needs to be placed inside your **CSI/Zones/[SurfaceName]/** folder. The name of the .zon file itself can be whatever you’d like, but as a best practice you may want to include the plugin format, plugin name, and manufacturer name. 

When CSI is initialized, it reads all .zon files in your CSI surface folders. This is important to understand because when you create an FX.zon file with Reaper open, you will need to re-initialize CSI by either running the Reaper action “Refresh all surfaces”, or opening the CSI preferences in Reaper and clicking OK, or by restarting Reaper.  

It is highly recommended, but not required, that you create an FXZones subfolder to keep things tidy. So **CSI/Zones/[SurfaceName]/FXZones/**. If you create a large number of fx zones, you can even further break them down by manufacturer such as **CSI/Zones/[SurfaceName]/FXZones/Universal Audio/**. 


### An Example FX.Zon
Let’s begin by reviewing a simple fx.zon file and breaking it down to its component parts, then working backwards as to how we got there. I’m using the UAD Teletronix LA-2A Silver plugin here, mapped to the widgets on a MCU/X-Touch Universal-style device.

```
Zone "VST: UAD Teletronix LA-2A Silver (Universal Audio, Inc.)" "LA2ASlv"
     Rotary1             FXParam 0 
     RotaryPush1         NoAction
     DisplayUpper1       FixedTextDisplay "Thresh"
     DisplayLower1       FXParamValueDisplay 0

     Rotary2             FXParam 3
     RotaryPush2         NoAction  
     DisplayUpper2       FixedTextDisplay "HF Emph"
     DisplayLower2       FXParamValueDisplay 3

     Rotary3             FXParam 1
     RotaryPush3         NoAction 
     DisplayUpper3       FixedTextDisplay "Output" 
     DisplayLower3       FXParamValueDisplay 1 

     Rotary4             NoAction
     RotaryPush4         FXParam 2 [ 0.0 1.0 ]
     DisplayUpper4       FixedTextDisplay "CompLim" 
     DisplayLower4       FXParamValueDisplay 2

     Rotary5             FXParam 4 [ 0.0 0.50 1.0 ]
     RotaryPush5         NoAction     
     DisplayUpper5       FixedTextDisplay "Meter" 
     DisplayLower5       FXParamValueDisplay 4

     Rotary6             NoAction
     RotaryPush6         NoAction     
     DisplayUpper6       NoAction
     DisplayLower6       NoAction

     Rotary7             NoAction 
     RotaryPush7         NoAction
     DisplayUpper7       NoAction 
     DisplayLower7       NoAction 

     Rotary8             NoAction
     RotaryPush8         ToggleFXBypass
     DisplayUpper8       NoAction
     DisplayLower8       FXBypassedDisplay
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
     Rotary1             FXParam 0 
```

FXParam is a CSI action, and the number 0 is telling CSI which plugin parameter to assign to the widget. Each plugin parameter that can be controlled by CSI will have a unique FX Param index #. 

### Finding FXParam Numbers
So how do you find the correct FXParam numbers? There are a few options. 

First, you can run the Reaper action **"CSI Toggle Show Params when FX inserted"**, then insert the FX. This will open a ReaConsole window that looks like this...
```
Zone "VST: UAD Teletronix LA-2A Silver (Universal Audio, Inc.)"
	FXParam 0 "Peak Reduct"
	FXParam 1 "Gain"
	FXParam 2 "Comp/Limit"
	FXParam 3 "Emphasis"
	FXParam 4 "Meter"
	FXParam 5 "Mix"
	FXParam 6 "Power"
	FXParam 7 "Bypass"
	FXParam 8 "Wet"
	FXParam 9 "Delta"
ZoneEnd
```

You can also run the Reaper action **"CSI Toggle Write Params to /CSI/Zones/ZoneRawFXFiles when FX inserted"** then insert the FX. That will create a .txt file that looks identical the ReaConsole window shown above. The .txt file can be located in the **CSI/Zones/ZoneRawFXFiles** folder.

The third method is that you can toggle the plugin UI in Reaper by clicking the "UI" button in the plugin menu. This hides the plugin graphics and replaces them with a series of horizontal sliders. The first slider is FXParam 0, and then you would count up from there. 

You will need to append the FXParam # for the following actions to work:
```
FXParam
FXParamNameDisplay
FXParamValueDisplay
```

FXParam is used to control the parameter and map it to a widget. FXParamNameDisplay can be used on a surface with displays to show the name of the parameter. Note: if the parameter name is too long, you may want to use FixedTextDisplay followed by an alias in quotes. You'll see that used throughout this example. FXParamValueDisplay shows the value of the parameter.

### Returning to Our Example
We then see...
```
     RotaryPush1         NoAction     
     DisplayUpper1       FixedTextDisplay "Thresh"
     DisplayLower1       FXParamValueDisplay 0
```

RotaryPush1 "NoAction" says that when this FX.zon is active, RotaryPush1 should not do anything.

DisplayUpper1 will show the word “Thresh” when this zone is active. This is useful if you have limited character displays and FX Params may have long names. Alternately, you could have used the FXParamNameDIsplay action as shown below:

```
     DisplayUpper1       FXParamNameDisplay 0
```

…this action will display the full parameter name, versus the alias that was created using the FixedTextDisplay action. The 0 that follows the FXParamNameDisplay action is FXParam #. We'll get into that in a second. 

The next line shows
```
     DisplayLower1       FXParamValueDisplay 0
```

This will display the value of the FX Parmeter being controlled.

Jumping further ahead you see...
```
     Rotary4             NoAction
     RotaryPush4         FXParam 2 [ 0.0 1.0 ]
     DisplayUpper4       FixedTextDisplay "CompLim" 
     DisplayLower4       FXParamValueDisplay 2
```

Here, the turning the Rotary encoder does nothing due to the NoAction line, but the RotaryPush4 will toggle the Comp/Limiter parameter when pressed. That's what the [ 0.0 1.0 ] means when it follows an FX Param. It's essentially saying, "each press should toggle between the minimum and maximum values". See the [Stepped Params and Toggles](https://github.com/GeoffAWaddington/CSIWiki/wiki/Stepped-Parameters-and-Toggles) page for more details.

The final thing I want to call out in this FX.zon is that I've used NoAction for any controls on my surface that don't have an action in the FX.zon. Why? This is a best-practice recommendation to make sure that you're not inadvertently changing parameters on your track or elsewhere that you don't intend to when the fx.zon is active. This will also ensure the displays clear out.
```
     Rotary6             NoAction
     RotaryPush6         NoAction     
     DisplayUpper6       NoAction
     DisplayLower6       NoAction
```

Those are the basics to creating an fx.zon file. 

### FX SubZones
CSI version 1.1 introduced FX Sub Zones. These are useful when you are trying to map an FX that has more parameters than your surface has controls for, or maybe the plugin has multiple FX types or modes and you'd like to put them into different zones you can switch between. This could even be useful for mapping instruments if you wanted to have filter controls on one zone, oscillator controls in another, envelopes and LFO's in another, etc. The sky is the limit with FX sub zones.

For FX Sub Zones to work you basically need a few key elements:

1. Your initial fx.zon file. This should include the parameter mapping you want to see when the plugin is mapped just like any other fx.zon. It should also have a name that matches the plugin, just like any typical fx.zon file. Example: [plugin].zon

2. One or more separate files for each sub zone. Remember: in CSI version 1.1, each zone must be in a separate .zon file (it's one per). The sub zones should not include a navigator. It is recommended you number these Sub Zone FX files using the same plugin name, but appending a number to the end of the file in ascending order for each Sub Zone. Example: [plugin]-1.zon and [plugin]-2.zon

3. Instructions in the initial fx.zon file about which SubZones are to be included. See the "SubZones" and "SubZonesEnd" section in the example below.

4. GoSubZone actions in all of the .zon files so CSI knows how to move from zone to zone.

## Example
In this example, I've mapped Limiter 6 GE from Tokyo Dawn Labs across 3 different zones. The primary [plugin].zon has the Compressor controls, the first Sub Zone has the Peak Limiter controls, and the second Sub Zone has the High Frequency Limiter, Clip and Output controls.

I have dedicated banking buttons on my surface (BankA, BankB, and BankC) that I will use to switch between each zone. You can see that each zone includes the GoSubZone  action to jump to the other Banks. You could of course assign different buttons to switch between zones.

First file: **VST__TDR_Limiter_6_GE__Tokyo_Dawn_Labs_.zon**
```
Zone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)" "Limiter6 GE"
SelectedTrackNavigator
     SubZones
          "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-1"
	  "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-2"
     SubZonesEnd
/
BankA         GoSubZone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)" "Compressor"
BankB         GoSubZone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-1" "Peak Lim"
BankC         GoSubZone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-2" "HF Lim+Clip"
/ 
RotaryA1 FXParam 6 "Comp Thresh"
RotaryA2 FXParam 8 "Comp Ratio" 
RotaryA3 FXParam 9 "Comp Attack"
RotaryA4 FXParam 10 "Comp Release"
/
ZoneEnd
```

First Sub Zone file: **VST__TDR_Limiter_6_GE__Tokyo_Dawn_Labs_-1.zon**
```
Zone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-1" "Peak Lim"
/
BankA         GoSubZone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)" "Compressor"
BankB         GoSubZone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-1" "Peak Lim"
BankC         GoSubZone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-2" "HF Lim+Clip"
/  
RotaryA1 FXParam 18 "Peak Lim Gain"
RotaryA2 FXParam 20 "Peak Lim Thresh"
RotaryA3 FXParam 25 "Peak Lim Focus"
RotaryA4 FXParam 26 "Peak Lim Release"
/
ZoneEnd
```
**Note:** Only the initial [plugin].zon should include a navigator. Do not include a navigator in any Sub Zones.

Second Sub Zone file: **VST__TDR_Limiter_6_GE__Tokyo_Dawn_Labs_-2.zon**
```
Zone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-2" "HF Lim+Clip"
/
BankA         GoSubZone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)"   "Compressor"
BankB         GoSubZone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-1" "Peak Limiter"
BankC         GoSubZone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-2" "HF Lim+Clip"
/  
RotaryA1 FXParam 34  "HF Lim Threshold"
RotaryA2 FXParam 38 "HF Lim Frequency"
RotaryA3 FXParam 36 "HF Lim Range"
RotaryA4 FXParam 41 "HF Lim Dry Amt"
/
ZoneEnd
```

## FX Mapping Actions
CSI allows you to map plugin parameters to your control surface. See [[FX Zones]] for details on how to create a .zon file for your effect or instrument (they're called FX zones, but work equally well for instruments). You'll also want to see [[Generating a Raw FX Zon File|FX Zones#Generating a Raw FX zon file]] for a quick and easy to way to get the FX Param # that you'll need for the mapping. 

## # FXParam
Is the CSI action to control a plugin parameter. Let's say we've generated the [[Raw FX Zon File|FX Zones#Generating a Raw FX zon file]] for ReaComp, which looks like this...

```
Zone "VST: ReaComp"
	SelectedTrackNavigator
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

## Activating FX Zones (FocusedFX, TrackFXMenu, SelectedTrackFXMenu, SelectedTrack)