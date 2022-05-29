If you have a surface with multiple displays, you can setup an FXMenu to allow you to select which FX to control and engage the FX .zon's. Pre-condition: the FX still need to be mapped with their own fx .zon files and those .zon files would need to use the SelectedTrackNavigator. 

The way this works is your surface .zon file needs to include the MapSelectedTrackFXToMenu CSI action, which will then list out the FX on the selected track on your surfaces displays. Here's an example using the "Plugin" button on a stock MCU device.

```
Zone "Buttons"
    Plugin     MapSelectedTrackFXToMenu
ZoneEnd
``` 

Now, you need to also add the FXMenu zone in your surface .zon file as well. Note: FXMenu is a special type of zone and do not rename it.

This is what a typical MCU FXMenu zone looks like. The upper display will show the name of the FX in each slot of the selected track. Pressing the corresponding RotaryPush will engage the fx.zon for that FXSlot via the GoFXSlot action. GoFXSlot says "for this track, activate whatever FX Zone (e.g. Pro-C_fabfilter.zon) is located at this slot."  

```
Zone "FXMenu"
    FXMenuNavigator
    DisplayUpper|               FXMenuNameDisplay
    RotaryPush|                 GoFXSlot |
ZoneEnd
```

To disable the mapping, you'd just need a way of getting back to your Home zone like...

```
    GlobalView                  GoZone Home
```