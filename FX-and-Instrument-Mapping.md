CSI allows users to create custom FX and instrument mappings which can be used across one or more surfaces. FX Zones are similar to surface [[Zones]] in that we are mapping [[Widgets defined in our mst files|Defining Control Surface Capabilities]] to behavior we want to occur. The major difference is about what that behavior is. In a surface [[Zone|Zones]] we're binding the widgets to some sort of action in Reaper itself, whereas for FX Zones, we are binding widgets to a parameter value in an FX plugin (eg.  VST, VSTi, VST3, VST3i, AU, AUi, etc.). 

This section of the Wiki will first focus on how to create FX zone files (I may refer to them as fx.zon files as shorthand) and how to activate those fx.zon files. Note: There is no distinction in CSI or Reaper between instruments or FX so the same principals apply to both.

At a high level, first you create the fx.zon file for each plugin, then you determine how you’d like to activate that fx.zon using CSI. We’ll cover all elements of that process here.

## Understanding FX.zon Files
Each plugin you map will require its own unique .zon file which needs to be placed inside your **CSI/Zones/[SurfaceName]/** folder. The .zon file is a plain text file that you will create and simply rename the file extension to .zon. The name of the .zon file itself can be whatever you’d like, but as a best practice you may want to include the plugin format, plugin name, and manufacturer name. Chances are, you'll need to create this fx.zon file on your own, unless someone with the same plugin and surface already created a mapping that you can use.

When CSI is initialized, it reads all .zon files in your CSI surface folders. This is important to understand because when you create an FX.zon file with Reaper open, you will need to re-initialize CSI by either running the Reaper action “Refresh all surfaces”, or opening the CSI preferences in Reaper and clicking OK, or by restarting Reaper.  

It is highly recommended, but not required, that you create an FXZones subfolder to keep things tidy. So **CSI/Zones/[SurfaceName]/FXZones/**. If you create a large number of fx zones, you can even further break them down by manufacturer such as **CSI/Zones/[SurfaceName]/FXZones/Universal Audio/**. 

**Tip:** Many CSI users have created custom Excel documents or even just simple text files that they can use to help speed up the process of creating fx.zon files. You would basically list out all the widgets on your surface you'd use for FX, set up a document that included all of those as a template, then just copy paste the FXParam #'s or other actions to the create the mapping, and export as a .txt file.

## Finding FXParam Numbers
The first step to actually creating your first fx.zon is locating the FXParam # that CSI will utilize to map FX parameters to control surface widgets. There are 3 methods you can use to accomplish this task.

### Method 1: CSI Toggle Show Params when FX inserted
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

Note: the last 3 parameters in any such list will be assigned to the Reaper parameters for plugin Bypass, Wet, and Delta Solo. This will be in addition to any bypass and/or wet-dry mix parameters that may already exist in the plugin.

### Method 2: CSI Toggle Write Params to /CSI/Zones/ZoneRawFXFiles when FX inserted
You can also run the Reaper action **"CSI Toggle Write Params to /CSI/Zones/ZoneRawFXFiles when FX inserted"** then insert the FX. That will create a .txt file that looks identical the ReaConsole window shown above. The .txt file can be located in the **CSI/Zones/ZoneRawFXFiles** folder.

### Method 3: Turn off the plugin UI and count!
The third method is that you can toggle the plugin UI in Reaper by clicking the "UI" button in the plugin menu. This hides the plugin graphics and replaces them with a series of horizontal sliders. The first slider is FXParam 0, and then you would count up from there. 

You will need to append the FXParam # for the following actions to work:
```
FXParam
FXParamNameDisplay
FXParamValueDisplay
```

FXParam is used to control the parameter and map it to a widget. FXParamNameDisplay can be used on a surface with displays to show the name of the parameter. Note: if the parameter name is too long, you may want to use FixedTextDisplay followed by an alias in quotes. You'll see that used throughout this example. FXParamValueDisplay shows the value of the parameter.

## Creating FX and Instrument Zone Files

### An Example FX.Zon
Let’s begin by reviewing a simple fx.zon file and breaking it down to its component parts, then working backwards as to how we got there. I’m using the UAD Teletronix LA-2A Silver plugin here, mapped to the widgets on a MCU/X-Touch Universal-style device. This example may look a little scary at first, but you'll see that it's actually pretty easy to understand when you break it down line by line.

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

     Rotary8             FXParam 8
     RotaryPush8         ToggleFXBypass
     DisplayUpper8       FixedTextDisplay "Wet"
     DisplayLower8       FXBypassedDisplay
ZoneEnd

```

### Open up a plain Text Editor
As mentioned earlier, fx.zon files are just plain text files so to create the fx.zon, the first thing we'll need to do is open a text editor application. **Tip:** if your fx.zon isn't being read by CSI, make sure you're not using "Rich Text" or any similar encoding features in your text editor. 

Also, as a best practice, fx.zon files are much more legible and easier to troubleshoot when you use spaces in the .txt file to align the FX parameter and other CSI actions as shown in the example above. Notice how all the CSI actions on the right are horizontally aligned in the .zon file? A little upfront work in the zone authoring process will save you headaches later on. I personally prefer spacebar avoid the Tab key because this will translate inconsistently by different applications.

### The first and last lines of an fx.zon 
The first line of an FX zone file must show the plugin name exactly as it appears in Reaper. You can optionally add a plugin "alias" (or short name) that will appear in FX lists. 

In this FX zone, the first line is:
```
Zone "VST: UAD Teletronix LA-2A Silver (Universal Audio, Inc.)" "LA2ASlv"
```

The word Zone is required, followed by the EXACT plugin name as it appears in Reaper - this will include the plugin format (VST, VST3, etc.). A typo anywhere in the plugin name will prevent your map from working. Therefore, it is recommended you use method #1 or #2 from [[Finding FXParam Numbers|FX-and-Instrument-Mapping#finding-fxparam-numbers]] and copy and paste the first line directly into your new fx.zon. If you have both VST2 and VST3 versions of the same plugin installed, you will need a .zon file for each. **Tip:** be careful, sometimes the mappings are not identical between formats so it's not always as easy as renaming the files and changing the file name in the first line of the .zon file.

Following the full plugin name, you’ll see “LA2ASlv”: this is the plugin alias and can be whatever you want. This will be read as the FXMenuNameDisplay action in CSI, and can be seen on the FXMenu zone files, which we will get into later on. As a best practice, it’s recommended you create a short alias, but it’s not required. However, if you do not have an alias in your FX zone, you may only see something like “VST: UAD” on your surface, which wouldn’t tell you which UAD plugin was loaded in that slot.

I’ll also jump ahead to the very last line of the .zon file which is simply the word ZoneEnd. This is required at the end of every CSI zone file, whether an FX or otherwise.
```
ZoneEnd
```

### Mapping our first action
The first FX parameter that has been mapped to a widget in CSI, is FX Param 0 (which happens to be the Threshold control in this plugin) and that's mapped to the Rotary1 widget. So we add the widget name (Rotary1), followed by the CSI action FXParam and the number 0, which represents the plugin parameter to assign to the widget. Reminder: we identified the FXParam # by using the methods outlined in [[Finding FXParam Numbers|FX-and-Instrument-Mapping#finding-fxparam-numbers]]. 
```
     Rotary1             FXParam 0 
```

See [[FX-and-Instrument-Mapping]] for the full list of available FX mapping actions.

### Adding some displays for parameter name and value
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

This will display the value of the FX Parameter being controlled.

### Our first "toggle" action and using NoAction to reserve parameters
Jumping further ahead you see...
```
     Rotary4             NoAction
     RotaryPush4         FXParam 2 [ 0.0 1.0 ]
     DisplayUpper4       FixedTextDisplay "CompLim" 
     DisplayLower4       FXParamValueDisplay 2
```

Here, turning the Rotary encoder does nothing due to the NoAction line, but the RotaryPush4 will toggle the Comp/Limiter parameter when pressed. That's what the [ 0.0 1.0 ] means when it follows an FX Param. In CSI, all FX parameters are normalized to a range of 0.0 to 1.0 so that syntax is essentially saying, "each press should toggle between the minimum and maximum values". You'll also notice similar syntax used to step between 3 parameters on the rotary encoder with the "Meter" parameter in this .zon example. See the [Stepped Params and Toggles](https://github.com/GeoffAWaddington/CSIWiki/wiki/Stepped-Parameters-and-Toggles) page for more details. 

The final thing I want to call out in this FX.zon is that I've used NoAction for any controls on my surface that don't have an action in the FX.zon. Why? This is a best-practice recommendation to make sure that you're not inadvertently changing parameters on your track or elsewhere that you don't intend to when the fx.zon is active. This will also ensure the displays clear out.
```
     Rotary6             NoAction
     RotaryPush6         NoAction     
     DisplayUpper6       NoAction
     DisplayLower6       NoAction
```

### Save the .zon file
The last step of course is to save the .zon file. Remember, these are just plain .txt files where you will rename the file extension .zon. In this example, I'd recommend a name like **VST2_UAD_LA2A_Silver.zon**. Do not use spaces in the file name. See the [[Understandig FX and Instrument Mapping|FX-and-Instrument-Mapping#understanding-fxzon-files]] section of this page for details and tips about how to name and where to save these files.

Those are the basics to creating an fx.zon file. See the [[FX Parameter Mapping Actions|FX-Parameter-Mapping-Actions]] page for details on all the pertinent mapping actions.

## FX SubZones
FX Sub Zones are useful when you are trying to map an FX that has more parameters than your surface has controls for, or maybe the plugin has multiple FX types or modes and you'd like to put them into different zones you can switch between. This could even be useful for mapping instruments if you wanted to have filter controls on one zone, oscillator controls in another, envelopes and LFO's in another, etc. The sky is the limit with FX sub zones.

For FX Sub Zones to work you basically need a few key elements:

1. Your initial fx.zon file. This should include the parameter mapping you want to see when the plugin is mapped just like any other fx.zon. It should also have a name that matches the plugin, just like any typical fx.zon file. Example: [plugin].zon

2. One or more separate files for each sub zone. Remember: each zone must be in a separate .zon file. It is recommended you number these Sub Zone FX files using the same plugin name but appending a number to the end of the file in ascending order for each Sub Zone. Example: [plugin]-1.zon and [plugin]-2.zon

3. Instructions in the initial fx.zon file about which SubZones are to be included. See the "SubZones" and "SubZonesEnd" section in the example below.

4. GoSubZone actions in all of the .zon files so CSI knows how to move from zone to zone.

### FX SubZone Example
In this example, I've mapped Limiter 6 GE from Tokyo Dawn Labs across 3 different zones. The primary [plugin].zon has the Compressor controls, the first Sub Zone has the Peak Limiter controls, and the second Sub Zone has the High Frequency Limiter, Clip and Output controls.

I have dedicated banking buttons on my surface (BankA, BankB, and BankC) that I will use to switch between each zone. You can see that each zone includes the GoSubZone  action to jump to the other Banks. You could of course assign different buttons to switch between zones.

First file: **VST__TDR_Limiter_6_GE__Tokyo_Dawn_Labs_.zon**
```
Zone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)" "Limiter6 GE"
     SubZones
          "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-1"
	  "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-2"
     SubZonesEnd
/
BankA         GoSubZone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)" "Compressor"
BankB         GoSubZone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-1" "Peak Lim"
BankC         GoSubZone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-2" "HF Lim+Clip"
/ 
RotaryA1      FXParam 6 "Comp Thresh"
RotaryA2      FXParam 8 "Comp Ratio" 
RotaryA3      FXParam 9 "Comp Attack"
RotaryA4      FXParam 10 "Comp Release"
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
RotaryA1      FXParam 18 "Peak Lim Gain"
RotaryA2      FXParam 20 "Peak Lim Thresh"
RotaryA3      FXParam 25 "Peak Lim Focus"
RotaryA4      FXParam 26 "Peak Lim Release"
/
ZoneEnd
```

Second Sub Zone file: **VST__TDR_Limiter_6_GE__Tokyo_Dawn_Labs_-2.zon**
```
Zone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-2" "HF Lim+Clip"
/
BankA         GoSubZone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)"   "Compressor"
BankB         GoSubZone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-1" "Peak Limiter"
BankC         GoSubZone "VST: TDR Limiter 6 GE (Tokyo Dawn Labs)-2" "HF Lim+Clip"
/  
RotaryA1      FXParam 34  "HF Lim Threshold"
RotaryA2      FXParam 38 "HF Lim Frequency"
RotaryA3      FXParam 36 "HF Lim Range"
RotaryA4      FXParam 41 "HF Lim Dry Amt"
/
ZoneEnd
```