## WidgetMode
WidgetMode is designed to send additional, specific, instructions to a given widget. For instance, on a typical MCU-style device, you can set the Rotary encoder feedback to vary between Dot, BoostCut, Fill, and Spread modes.
```
    Rotary|                     TrackPan
    Rotary|			WidgetMode Dot
    DisplayLower|      		TrackPanDisplay

    Toggle+Rotary|              TrackPanWidth
    Toggle+Rotary|		WidgetMode BoostCut
    Toggle+DisplayLower| 	TrackPanWidthDisplay
```

## SetWidgetMode
SetWidgetMode exists because you may want to set a Faderport display ScribbleStripMode, therefore, you will need SetWidgetMode since there is no other Action that updates the Widget, as there would be with, say, Rotary values and LED ring style.
```
    FPDisplay   SetWidgetMode
    FPDisplay   WidgetMode SomeWidgetMode
```

## Faderport WidgetModes
# New FaderPortDisplay functionality (FB_FP8ScribbleStripMode Feedback Processors, new widget modes)
CSI supports all the display functionality of the Presonus FaderPort8 and FaderPort16. The FaderPorts have multiple (9) display types to choose from, contains a ValueBar and can, depending on the display type, show the VU Meter. 

### Display Type
Display type is a per display setting and consists of a widget and an action to set the actual display type. In your .mst file this will look like:
```
// ===========================================
// SCRIBBLE STRIP MODE
// ===========================================
Widget ScribbleStripMode1
	FB_FP8ScribbleStripMode 0
WidgetEnd

Widget ScribbleStripMode2
	FB_FP8ScribbleStripMode 1
WidgetEnd
etc.....
```
The image below shows all the display types and the Id

![FaderPortDisplayTypes](https://user-images.githubusercontent.com/52307138/185772386-6217f370-9564-43b4-8ca3-79463b374c9d.jpg)

The default scribble strip mode is id **2** This is a version with 4 lines and in most cases the most versatile one.

For implementing the scribble strip mode you need to add 2 lines of code. The first one is adding the scribblescript widget to the file. It's value should be `SetWidgetMode`. This tells CSI the only value to use will be the WidgetMode. The second line sets the WidgetMode value. In this case that is the Scribble strip mode. The value is the ID of one of the layouts shown above.

In your zone file this will look like this:
```
Zone "Track"
  ScribbleStripMode|        SetWidgetMode
  ScribbleStripMode|        WidgetMode 8
```

### Scribble lines

Each of the 4 scribble lines requires it’s own widget in the .mst file.
For your .mst, here are the names for the FB generators that correspond to each line on the surface.

|  | **FaderPort 8** | **FaderPort 16** |
| --- | --- | --- |
| Line 1 | FB_FP8ScribbleLine1 | FB_FP16ScribbleLine1 |
| Line 2 | FB_FP8ScribbleLine2 | FB_FP16ScribbleLine2 |
| Line 3 | FB_FP8ScribbleLine3 | FB_FP16ScribbleLine3 |
| Line 4 | FB_FP8ScribbleLine4 | FB_FP16ScribbleLine4 |

And here is an example of how the `.mst` would be mapped out for Display 1 on the Faderport8.
```
// ===========================================
// SCRIBBLE LINES CHANNEL 1
// ===========================================
Widget ScribbleLine1_1
	FB_FP8ScribbleLine1 0
WidgetEnd

Widget ScribbleLine2_1
	FB_FP8ScribbleLine2 0
WidgetEnd

Widget ScribbleLine3_1
	FB_FP8ScribbleLine3 0
WidgetEnd

Widget ScribbleLine4_1
	FB_FP8ScribbleLine4 0
WidgetEnd

```

Per scribble line you're able to set ythe text align and you can set a line to inverted (changing back and foreground colour). These values are set with a WidgetMode.
* **TextAlign**: Possible values are **Center**, **Left** and **Right**. The default when not set is **Center**
* **Invert**: Pass the value **Invert** to invert. It will be normal when not passed.

Using this in a zone file will look like this:
```
Zone "Track"
  ScribbleLine1_|      TrackNameDisplay
  ScribbleLine1_|      WidgetMode "Left"

  ScribbleLine2_|      TrackNameDisplay
  ScribbleLine2_|      WidgetMode "Center Invert"
ZoneEnd
```
_In this example line 1 in the scribble text will be left aligned. Line 2 will be right aligned and inverted._

### Valuebar
The FaderPort 8 and FaderPort 16 have a Valuebar available in the scribble display. The ValueBar can be used for a visual representation of pan value, volume, pan width, etc. The valuebar has 5 different modes.

![FaderPortValueBar](https://user-images.githubusercontent.com/52307138/185798368-b404f2a3-945f-4c06-88e5-3088c663faed.jpg)

In the mst file it looks like:
```
// ===========================================
// VALUE BAR
// ===========================================
Widget ValueBar1
	FB_FPValueBar 0
WidgetEnd

Widget ValueBar2
	FB_FPValueBar 1
WidgetEnd
```
Setting the mode is the zone file is handled by a property. For this reason it is not possible changing this with a modifier key. In the zone file it looks like:
```
Zone "Track"
  ValueBar|     TrackPan
  ValueBar|     WidgetMode BiPolar