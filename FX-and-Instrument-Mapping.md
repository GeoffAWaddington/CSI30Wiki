CSI allows users to create custom FX and instrument mappings which can be used across one or more surfaces. FX Zones are similar to surface [[Zones]] in that we are mapping [[Widgets defined in our mst files|Defining Control Surface Capabilities]] to behavior we want to occur. The major difference is about what that behavior is. In a surface [[Zone|Zones]] we're binding the widgets to some sort of action in Reaper itself, whereas for FX Zones, we are binding widgets to a parameter value in an FX plugin (eg.  VST, VSTi, VST3, VST3i, AU, AUi, etc.). 

This section of the Wiki will first focus on how to create FX zone files (I may refer to them as fx.zon files as shorthand) and how to activate those fx.zon files. Note: There is no distinction in CSI or Reaper between instruments or FX so the same principals apply to both.

At a high level, first you create the fx.zon file for each plugin, then you determine how you’d like to activate that fx.zon using CSI. We’ll cover all elements of that process here.

## Understanding FX.zon Files
Each plugin you map will require its own unique .zon file which needs to be placed inside your **CSI/Zones/[SurfaceName]/** folder. The .zon file is a plain text file that you will create and simply rename the file extension to .zon. The name of the .zon file itself can be whatever you’d like, but as a best practice you may want to include the plugin format, plugin name, and manufacturer name. Chances are, you'll need to create this fx.zon file on your own, unless someone with the same plugin and surface already created a mapping that you can use.

When CSI is initialized, it reads all .zon files in your CSI surface folders. This is important to understand because when you create an FX.zon file with Reaper open you will need to first, remove all instances of the FX from your project, then re-initialize CSI by either running the Reaper action “Refresh all surfaces”, or opening the CSI preferences in Reaper and clicking OK, or by restarting Reaper.  

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

In your Reaper Resource Path, you must have a CSI/Zones/ZoneRawFXFiles folder already. If one doesn't exist, create one. If it's still not generating the .txt files when you insert FX after toggling that action to On, check permissions and/or run Reaper in "Admin mode" then try again.

### Method 3: Turn off the plugin UI and count!
The third method is that you can toggle the plugin UI in Reaper by clicking the "UI" button in the plugin menu. This hides the plugin graphics and replaces them with a series of horizontal sliders. The first slider is FXParam 0, and then you would count up from there. 

You will need to append the FXParam # for the following actions to work:
```
FXParam
FXParamNameDisplay
FXParamValueDisplay
```

FXParam is used to control the parameter and map it to a widget. FXParamNameDisplay can be used on a surface with displays to show the name of the parameter. Note: if the parameter name is too long, you may want to use FixedTextDisplay followed by an alias in quotes. You'll see that used throughout this example. FXParamValueDisplay shows the value of the parameter.

# Creating FX and Instrument Zone Files - 2 Types: Classic FX Zones, and EZ FX Zones
There are now two options when it comes to FX Zones: 1) Classic FX Zones, and 2) EZ FX Zones. When mapping new FX, you have the option of using either type. CSI will be able to parse out which type is in use. You cannot however mix and match types in a single fx.zon. 

**Classic FX Zones** follow a similar syntax to surface zone files in that there is one assignment per row following a "WidgetName, Action, Additional Instructions" type of syntax from left to right. They can be read from left to right, top down. The benefits of classic FX zones are that they are very flexible, and easy to read. Cons are that they can get very lengthy and have lots of duplicated instructions.

**EZ FX Zones (aka EZFXZones)** are an alternative syntax for FX Zones that requires far fewer lines, saving a lot of the tedious repetition of the Classic FX Zone format. EZFXZones follow a spreadhseet-like format where you read both across and down. When combined with a properly setup .mst file, EZFXZones can utilize user-created defaults for encoder resolution (aka "step size") and encoder acceleration, rather than having to add each of these instructions on a per-row basis like in Classic FX Zones. EZFXZones are a first step in the roadmap to auto-mapping of fx in CSI. 

## Classic FX Zones

### An Example Classic FX Zone
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

### The first and last lines of an fx.zon (Plugin Type, Plugin Name, Plugin Alias)
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

## EZ FX Zones

Diving right into a real-world example, the first and last lines of any fx.zon remain unchanged. After that, the first block of text starting with "FXParams" is saying FXParam 9, which, reading down, we are giving the alias "HeadRoom", gets assigned to Rotary1. You will see the FXParamName on DisplayUpper1, and you will see the FXParamValue on DisplayLower1. Reading across on line 2 again, FXParam 1, "Input", will get assigned to Rotary2 (we're reading down again), with the FXParamName name showing up on DisplayUpper2 and FXParamValueName on DisplayLower2. And on and on.
```
Zone "VST: UAD Fairchild 660 (Universal Audio, Inc.)" "Fair660"
     FXParams                 9      1     2      3     6    7      0     8
     FXParamNames             HdRoom Input Thresh Ratio Knee Output Meter WetDry
     FXValueWidgets           Rotary|
     FXParamNameDisplays      DisplayUpper|
     FXParamValueDisplays     DisplayLower|
ZoneEnd
```
**Note:** using the legacy FX.zon syntax, this simple mapping would've required something like 40 lines of code. The new EZFXZone syntax accomplishes all the same functionality in only 5 lines! If you wanted, you could stop right there; that could be an entire fx.zon!

Now, if we want to add a modifier, we add the next block of text shown below. Very similar layout, except there is one more row, with the addition of the **FXWidgetModifiers Shift** line. This says, show FXParam 5, "DC Bal", on Shift+Rotary1 and the corresponding Shift-Displays. Also note that "DC Bal" is in quotes - these are only required because there is a space in what we want to appear on the display. Next, if you continue reading across, you see a series of -1 entries in the FXParams row and double quotes in the corresponding FXParamNames rows. This tells CSI, "No Action here and clear out the displays".
```
Zone "VST: UAD Fairchild 660 (Universal Audio, Inc.)" "Fair660"
     FXParams                 9      1     2      3     6    7      0     8
     FXParamNames             HdRoom Input Thresh Ratio Knee Output Meter WetDry
     FXValueWidgets           Rotary|
     FXParamNameDisplays      DisplayUpper|
     FXParamValueDisplays     DisplayLower|

     FXParams		      5	 -1 -1 -1 -1 -1 -1 4
     FXParamNames	      "DC Bal" "" "" "" "" "" "" SidChn
     FXValueWidgets  	      Rotary|
     FXParamNameDisplays      DisplayUpper|
     FXParamValueDisplays     DisplayLower|
     FXWidgetModifiers	      Shift
ZoneEnd
```

Now, let's say I want RotaryPush8 to toggle the FX Bypass state, but it's the only toggle-style action I need and I don't necessarily need to see that on a display. You can simply add another block with that specific instruction on the one widget:
```
Zone "VST: UAD Fairchild 660 (Universal Audio, Inc.)" "Fair660"
     FXParams                 9      1     2      3     6    7      0     8
     FXParamNames             HdRoom Input Thresh Ratio Knee Output Meter WetDry
     FXValueWidgets           Rotary|
     FXParamNameDisplays      DisplayUpper|
     FXParamValueDisplays     DisplayLower|

     FXParams		      5	 -1 -1 -1 -1 -1 -1 4
     FXParamNames	      "DC Bal" "" "" "" "" "" "" SidChn
     FXValueWidgets  	      Rotary|
     FXParamNameDisplays      DisplayUpper|
     FXParamValueDisplays     DisplayLower|
     FXWidgetModifiers	      Shift

     FXParams                 11
     FXValueWidgets           RotaryPush8
ZoneEnd
```

Now, how do we deal with stepped params, encoder acceleration, step sizes, colors, etc.? We simply add those to a new block of text at the bottom and define them on a per-parameter basis (except DefaultAcceleration, which is not at a per-parameter level). 
```
Zone "VST: UAD Fairchild 660 (Universal Audio, Inc.)" "Fair660"
     FXParams                 9      1     2      3     6    7      0     8
     FXParamNames             HdRoom Input Thresh Ratio Knee Output Meter WetDry
     FXValueWidgets           Rotary|
     FXParamNameDisplays      DisplayUpper|
     FXParamValueDisplays     DisplayLower|

     FXParams		      5	 -1 -1 -1 -1 -1 -1 4
     FXParamNames	      "DC Bal" "" "" "" "" "" "" SidChn
     FXValueWidgets  	      Rotary|
     FXParamNameDisplays      DisplayUpper|
     FXParamValueDisplays     DisplayLower|
     FXWidgetModifiers	      Shift

     FXParams                 11
     FXValueWidgets           RotaryPush8

     Left                     FXParamsBank -1
     Right                    FXParamsBank 1

     DefaultAcceleration      0.001 0.002 0.003 0.004 0.005 0.006 0.0075 0.01 0.02 0.035 0.05   
     FXParamAcceleration 7    0.001 0.002 0.003 0.004 0.005 0.006 0.0075 0.01 0.02 0.025 0.03 
     FXParamStepSize     0    0.5
     FXParamStepValues   1    0.0 0.05 0.11 0.16 0.21 0.26 0.32 0.37 0.42 0.47 0.53 0.58 0.63 0.68 0.74 0.79 0.84 0.89 0.95 1.0
     FXParamStepValues   3    0.0 0.20 0.40 0.60 0.80 1.0 
     FXParamStepValues   9    0.0 0.17 0.33 0.50 0.67 0.83 1.0
     FXParamTickCounts   3    3
     FXWidgetModes       1    BoostCut
     FXWidgetModes       7    BoostCut  
     FXParamColors       11   #ff0000 #00ff007f
ZoneEnd
```
So let's look at the new additions: what happens if you have more than 8 FXParams you want to assign to your 8-channel unit? You can continue adding additional FXParams in lines 2 and 3, then CSI will allow you to bank to the next FXParam via the **FXParamsBank** actions. Banking will also allow for mapping fx on 1-fader surfaces.

**DefaultAcceleration** is optional but allows you to set a custom acceleration curve to all encoders (there are per-parameter curves as well). 

The next set of actions are all on per-parameter basis. Notice how the syntax is the action type, the FXParam #, then the instructions with no need for brackets as used in traditional fx.zon files. So you can further still create parameter-level custom acceleration curves using the **FXParamAcceleration** action. **FXParamStepSize** is used to set the step size for an encoder tick on a FX Param. **FXParamStepValues** is used for stepped params. **FXParamTickCounts** sets the number of encoder ticks CSI must receive before advancing the FXParamValue. **FXWidgetModes** allow you to set display [[WidgetModes|WidgetMode]] on a per-parameter baseis. Lastly, **FXParamColors** can be used by supported surfaces to send RGBA colors (in hex format) to a supported widget. **Note:** CSI will be moving away from RGB to Hex everywhere.

At the time of this writing, the following actions have been implemented
```
FXParams
FXParamNames
FXValueWidgets
FXParamNameDisplays
FXParamValueDisplays

FXParamAcceleration
FXParamRange
FXParamStepSize
FXParamStepValues
FXParamTickCounts
FXParamColors (now uses Hex colors)
DefaultAcceleration
```

The following actions are coming soon.
```
FXParamsBank
FXWidgetModes
```

### RotaryWidgetClass and JogWheelWidgetClass
To facilitate better use of encoders in EZFXZones, a few .mst changes have been implemented. The first of those we will discuss is RotaryWidgetClass. RotaryWidgetClass is designed to help streamline how encoders are defined in .mst files and tell CSI which widgets are encoders. There are multiple elements to how this is used in an .mst file. 

First, by putting the word RotaryWidgetClass after the widget name in the .mst file, you are telling CSI, "this widget belongs in the rotary widget class" (as shown below). In a moment, we'll show you what that allows for.
```
Widget RotaryA1 RotaryWidgetClass
    Encoder b0 00 7f
    FB_Fader7Bit b0 00 00
WidgetEnd
```

The same can be done with your JogWheel using the new JogWheelWidgetClass.
```
Widget JogWheel JogWheelWidgetClass
	Encoder b0 3c 7f
WidgetEnd
```

### Defining "StepSize" for All Encoders in the RotaryWidgetClass
Now that the RotaryWidgetClass and/or JogWheelWidgetClass is defined for our encdoers, we can set the encoder StepSize globally by adding this to the top of the .mst file. This represents how fine the resolution will be for each encoder "tick". A value of 0.001 will be very fine and move parameters one-tenth of one-percent, which is very fine. If you find that resolution a little too fine, resulting in slow encoders, you may have better luck with a value of 0.003 or some other higher value. It will really depend on your hardware surfaces and preferences.

Here is an example from the X-Touch.mst showing both class types, each using a StepSize of 0.003.
```
StepSize
    RotaryWidgetClass   0.003
    JogWheelWidgetClass 0.003
StepSizeEnd
```

The encoders on MIDI Fighter Twister are very sensitive, so I might use a StepSize of 0.001 for that surface.
```
StepSize
    RotaryWidgetClass 0.001
StepSizeEnd
```

### AccelerationValues
Next, we can then remove the Encoder Acceleration step values from each individual widget and just create a global set of acceleration values at the top of the .mst file. The benefit of this approach is that rather than being required to define the Encoder Acceleration in each EZFXZone, CSI can now use a default for all encoders in the RotaryWidgetClass. 

Here we are defining the Decrease values ("Dec") from slowest encoder turns to fastest, and the same for the Increase ("Inc") values. Below that, you will find one encoder acceleration value ("Val") for each encoder acceleration step. The actual values used will depend on what your encoder transmits. The values below are from a MIDI Fighter Twister and my own personal acceleration curve.
```
AccelerationValues
    RotaryWidgetClass Dec 3f     3e    3d    3c    3b    3a    39     38    36    33    2f     
    RotaryWidgetClass Inc 41     42    43    44    45    46    47     48    4a    4d    51
    RotaryWidgetClass Val 0.001  0.002 0.003 0.004 0.005 0.006 0.0075 0.01  0.02  0.03  0.04
AccelerationValuesEnd
```
So in the past, each of my MFTEncoder widgets looked like the this...
```
Widget RotaryA1
	MFTEncoder b0 00 7f [ < 3f 3e 3d 3c 3b 3a 39 38 36 33 2f > 41 42 43 44 45 46 47 48 4a 4d 51 ]
	FB_Fader7Bit b0 00 00
WidgetEnd
```

Now we've got this text at the top of the .mst
```
StepSize
    RotaryWidgetClass 0.001
StepSizeEnd

AccelerationValues
    RotaryWidgetClass Dec 3f     3e    3d    3c    3b    3a    39     38    36    33    2f      
    RotaryWidgetClass Inc 41     42    43    44    45    46    47     48    4a    4d    51
    RotaryWidgetClass Val 0.001  0.002 0.003 0.004 0.005 0.006 0.0075 0.01  0.02  0.03  0.04
AccelerationValuesEnd
```

And the encoder widgets look like this (Note: MFTEncoder has been replaced with the standard Encoder widget since all the instructions are now up top).
```
Widget RotaryA1 RotaryWidgetClass
    Encoder b0 00 7f
    FB_Fader7Bit b0 00 00
WidgetEnd
```

Here is an example from the X-Touch.mst where the step size is 0.003 and the AccelerationValues line up with MCU-style devices.
```
StepSize
    RotaryWidgetClass   0.003
    JogWheelWidgetClass 0.003
StepSizeEnd

AccelerationValues
    RotaryWidgetClass   Dec 41     42    43    44    45    46    47  
    RotaryWidgetClass   Inc 01     02    03    04    05    06    07  
    RotaryWidgetClass   Val 0.0006 0.001 0.002 0.003 0.008 0.04  0.08 

    JogWheelWidgetClass Dec 41     42    43    44    45    46    47  
    JogWheelWidgetClass Inc 01     02    03    04    05    06    07  
    JogWheelWidgetClass Val 0.0006 0.001 0.002 0.003 0.008 0.04  0.08 
AccelerationValuesEnd
```

### EWidget (or "Eligible Widgets")
Another feature instituted now as part of the roadmap to auto-mapping FX is the addition of the "Ewidget" option in .mst files. This will eventually be used to tell CSI which widgets you'd like automatically included for automatic FX.zon mapping and which widgets you'd like excluded from that. Anything defined as an EWidget will be eligible for mapping. Here are some examples from an X-Touch.mst where we are using Displays, Rotarypush, Rotary, and Faders for FX mapping.

```
EWidget DisplayUpper1
	FB_XTouchDisplayUpper 0
EWidgetEnd
```

```
EWidget Fader1
	Fader14Bit e0 7f 7f
	FB_Fader14Bit e0 7f 7f
	Touch 90 68 7f 90 68 00
EWidgetEnd
```

```
EWidget RotaryPush1
	Press 90 20 7f 90 20 00
EWidgetEnd
```

```
EWidget Rotary1 RotaryWidgetClass
	Encoder b0 10 7f
	FB_Encoder b0 10 7f
EWidgetEnd
```

## ZoneStepSizes and .stp Files
The CSI Support Files now include a "ZoneStepSizes" sub-folder within the Zones folder. These files will be used by CSI in EZFXZones to determine which FX Parameters are stepped, how many steps each parameter has, and what the exact step values are. Again, this is another feature meant to simplify EZFXZone creation. Once a ZoneStepFile exists for a plugin, it shouldn't need to change (unless the developer adds new automation parameters) and can be shared. The CSI Support Files currently include ZoneFXFiles for almost 500 FX to get users started.

If you'd like to create some .stp files for your own use, you can add the below "AutoScan" line to your CSI.ini. The AutoScan process only attempts to create the .stp files for fx you already have a .zon for. It will not create .stp files for all FX. Note: this is an experimental feature and works better on Mac right now. If you run into issues, I'd encourage you to turn the AutoScan off by commenting out that line. When the AutoScan is complete and ZoneStepSize files created, you should also comment out (with a forward slash) or delete that line in your CSI.
```
Version 2.0

AutoScan

MidiSurface "XTouchOne" 7 9 
MidiSurface "MFTwister" 6 8 
OSCSurface "iPad Pro" 8003 9003 10.0.0.146
OSCSurface "TouchOSCLocal" 8002 9002 10.0.0.100
MidiSurface "CMC-QC" 23 24 

Page "HomePage"
"XTouchOne" 1 0 "X-Touch_One.mst" "X-Touch_One_FB" 
"MFTwister" 8 0 "MIDIFighterTwisterEncoder.mst" "FXTwisterMenu" 
"iPad Pro" 8 0 "FXTwister.ost" "FXTwisterMenu" 
"TouchOSCLocal" 8 0 "FXTwister.ost" "FXTwisterMenu" 
"CMC-QC" 0 0 "Stienberg_CMC-QC-2.mst" "Steinberg_CMC-QC-2" 
```

When a .stp file exists for a plugin, things like the below example are no longer required in EZFXZones. CSI will already know the step values and will automatically create a curve for your surface to move through each step if that parameter is assigned to an encoder. 
```
     FXParamStepValues   1    0.0 0.05 0.11 0.16 0.21 0.26 0.32 0.37 0.42 0.47 0.53 0.58 0.63 0.68 0.74 0.79 0.84 0.89 0.95 1.0
```

## EZFXZone Changes
If you've got Step Files for your FX, and made the changes outlined above, EZFXZones get much easier. The block of text at the bottom dictating the default acceleration and FX Step sizes are no longer required.

Before...
```
Zone "VST: UAD Fairchild 660 (Universal Audio, Inc.)" "Fair660"
     FXParams                 1            2         3               7             4           6           9        8
     FXParamNames             "Input Gain" Threshold "Time Constant" "Output Gain" "SC Filter" "DC Thresh" Headroom Mix
     FXValueWidgets           RotaryA|
     FXParamNameDisplays      DisplayUpperA|
     FXParamValueDisplays     DisplayLowerA|

     FXParams                 5     0       -1     -1     -1      -1     -1     12 
     FXParamNames             Bal   Meter   ""     ""     ""      ""     ""     Wet
     FXValueWidgets           RotaryB|
     FXParamNameDisplays      DisplayUpperB|
     FXParamValueDisplays     DisplayLowerB|

     FXParams                 -1   -1   -1   -1   -1   -1   -1     -1
     FXParamNames             ""   ""   ""   ""   ""   ""   ""     "" 
     FXValueWidgets           RotaryPushA|
     FXParamNameDisplays      DisplayRotaryPushA|

     FXParams                 -1   -1   -1   -1   -1   -1   13     11
     FXParamNames             ""   ""   ""   ""   ""   ""   Delta  Bypass
     FXValueWidgets           RotaryPushB|
     FXParamNameDisplays      DisplayRotaryPushB|

     DefaultAcceleration      0.001 0.002 0.003 0.004 0.005 0.006 0.0075 0.01 0.02 0.035 0.04     
     FXParamStepValues   0    0.0 0.50 1.0
     FXParamStepValues   1    0.0 0.05 0.11 0.16 0.21 0.26 0.32 0.37 0.42 0.47 0.53 0.58 0.63 0.68 0.74 0.79 0.84 0.89 0.95 1.0
     FXParamStepValues   3    0.0 0.20 0.40 0.60 0.80 1.0 
     FXParamStepValues   9    0.0 0.17 0.33 0.50 0.67 0.83 1.0
ZoneEnd
```

After...
```
Zone "VST: UAD Fairchild 660 (Universal Audio, Inc.)" "Fair660"
     FXParams                 1            2         3               7             4           6           9        8
     FXParamNames             "Input Gain" Threshold "Time Constant" "Output Gain" "SC Filter" "DC Thresh" Headroom Mix
     FXValueWidgets           RotaryA|
     FXParamNameDisplays      DisplayUpperA|
     FXParamValueDisplays     DisplayLowerA|

     FXParams                 5     0       -1     -1     -1      -1     -1     12 
     FXParamNames             Bal   Meter   ""     ""     ""      ""     ""     Wet
     FXValueWidgets           RotaryB|
     FXParamNameDisplays      DisplayUpperB|
     FXParamValueDisplays     DisplayLowerB|

     FXParams                 -1   -1   -1   -1   -1   -1   -1     -1
     FXParamNames             ""   ""   ""   ""   ""   ""   ""     "" 
     FXValueWidgets           RotaryPushA|
     FXParamNameDisplays      DisplayRotaryPushA|

     FXParams                 -1   -1   -1   -1   -1   -1   13     11
     FXParamNames             ""   ""   ""   ""   ""   ""   Delta  Bypass
     FXValueWidgets           RotaryPushB|
     FXParamNameDisplays      DisplayRotaryPushB|
ZoneEnd
```
