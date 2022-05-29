FX Zones are similar to normal [[Zones]] in that we are mapping [[Widgets defined in our mst files|Defining Control Surface Capabilities]] to behaviour we want to occur. The major difference is about what that behaviour is. In a [[normal Zone|Zones]] we're binding the widgets to some sort of action in Reaper, whereas for FX Zones, we're binding them to a parameter value in an FX plugin (eg. a VST, etc)

## A simple ReaEQ example
As an example, I have one of my BCF2000's mapped to control ReaEQ. For each band I add, it maps an [[Encoder]] to control the Frequency, a [[Fader|Fader7Bit]] to control the Gain, and [[Shift plus the Encoder|Modifiers]] to control the Bandwidth/Q. 

[[images/reaeq.png]] 

## The .zon file
You'll need to create an "[plugin name].zon" file for each plugin you map. These are just .txt files with the extension changed to .zon and can be edited and created in any text editor. Do not put spaces or special characters in the .zon filenames. Using the example below, our file name would ultimately be: VST__ReaEQ__Cockos_.zon

And here's an example of the contents of the following zon file: 
````
Zone "VST: ReaEQ (Cockos)" ReaEQ 
    FocusedFXNavigator
    RotaryG11 FXParam 0 "Freq-Band 1"
    Fader1 FXParam 1 "Gain-Band 1"
    Shift+RotaryG11 FXParam 2 "Q-Band 1"
    RotaryG12 FXParam 3 "Freq-Band 2"
    Fader2 FXParam 4 "Gain-Band 2"
    Shift+RotaryG12 FXParam 5 "Q-Band 2"
    RotaryG13 FXParam 6 "Freq-Band 3"
    Fader3 FXParam 7 "Gain-Band 3"
    Shift+RotaryG13 FXParam 8 "Q-Band 3"
    RotaryG14 FXParam 9 "Freq-Band 4"
    Fader4 FXParam 10 "Gain-Band 4"
    Shift+RotaryG14 FXParam 11 "Q-Band 4"
    RotaryG15 FXParam 12 "Freq-Band 5"
    Fader5 FXParam 13 "Gain-Band 5"
    Shift+RotaryG15 FXParam 14 "Q-Band 5"
    RotaryG16 FXParam 15 "Freq-Band 6"
    Fader6 FXParam 16 "Gain-Band 6"
    Shift+RotaryG16 FXParam 17 "Q-Band 6"
    RotaryG17 FXParam 18 "Freq-Band 7"
    Fader7 FXParam 19 "Gain-Band 7"
    Shift+RotaryG17 FXParam 20 "Q-Band 7"
    RotaryG18 FXParam 21 "Freq-Band 8"
    Fader8 FXParam 22 "Gain-Band 8"
    Shift+RotaryG18 FXParam 23 "Q-Band 8"
ZoneEnd
````

Let's walk through the key pieces:
* The first line ``Zone "VST: ReaEQ (Cockos)" ReaEQ`` defines the plugin that this zone is for, and needs to **exactly** match the name of the plugin as it appears in caption of the plugin window (see the first screenshot above, before the hyphen). 

* Note: If you rename a plugin in Reaper, your FX zone will need to be updated to match exactly. Anyone you share the FX .zon file with would either also have to rename their plugin in Reaper to match, or edit the .zon file to go back to the original name.

* Don't worry about the second line above ("FocusedFXNavigator"), we'll deal with that below in the [[How to activate an FX Zone|FX-Zones#how-to-activate-an-fx-zone]] section.

* We then have one line for each widget/plugin parameter mapping we want to create. As an example, the first one looks like this ``RotaryG11 FXParam 0 "Freq-Band 1"`` and breaks down as:
  * ``RotaryG11`` - the name of the widget
  * ``FX Param 0`` - the index of the plugin's parameter (I'll show you how to find that in a second)
  * ``"Freq-Band 1"`` - this can be whatever name you want to give the parameter. Use this for your own reference in the .zon file. If you have a surface with displays and would like to learn how to see the FX Parameter Name and Parameter Values on the surface itself, see [[FX-Parameter-Mapping-Actions]].

Once you've created your .zon file, put it in the Zone directory for whatever Surface it refers to (the same folder you have your other Zone definitions for that surface. In theory you could even put it in the same file as the other zones for that surface, but this is not recommended. It's much easier, clearer and more reusable to go with one file per plugin. You may even create a dedicated "FXZones" subfolder within your surface zone just to separate FX .zon files from the surface .zon file.

### Finding Parameter Indexes
Hopefully that's pretty easy, except for that Param Index issue. Where do we find them? One easy way I've found is to bring up the FX window in Reaper, and then click the UI button. 

[[images/reaeq_params.png]]

As you can see, Reaper will display a list of all the params in can find for the plugin as it is currently setup. The first one is Index 0, the second one is index 1, etc. 

Note I said "as it is currently setup" just then. If you add another band to ReaEQ, you'll see you get more params added, so you might need to explore a little until you find all the params you need. 

### Generating a Raw FX zon file

* Run the Reaper action "CSI Toggle Write Params to /CSI/Zones/ZoneRawFXFiles when FX inserted"
* Insert the FX of interest

Result: if you navigate to your CSI\Zones\ZoneRawFXFiles folder, you should now see a "[plugin_name].txt" file. Open that file up, and you will see a list of all the FX Param numbers for every parameter in that plugin, along with the parameter name for easy reference. Here's an abbreviated example of what ReaEQ looks like...

```
Zone "VST: ReaEQ (Cockos)"
	SelectedTrackNavigator
	FXParam 0 "Freq-Low Shelf"
	FXParam 1 "Gain-Low Shelf"
	FXParam 2 "Q-Low Shelf"
ZoneEnd
```

## How to activate an FX Zone

So, now we have our zone, we need a way to activate it. With [[normal zones|Zones]], we could just [[GoZone|Zone-Actions]] to it using a widget, but with FX Zones it is slightly different. We have a couple of options:

* [[Activate all the FX Zones for the currently selected track|MapSelectedTrackFXToWidgets]]
* [[Activate the FX Zone for the currently selected/focused FX window|MapFocusedFXToWidgets]]

You can of course mix and match these, activating some FX Zones when the track is selected, and others only when the FX Window is focused, but any one FX Zone can only be activated by one or the other, not both.  

