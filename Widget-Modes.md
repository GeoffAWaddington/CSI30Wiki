## Widget Modes
Widget Modes are designed to send additional, specific, instructions to a given widget and added after the action in a zone file. For instance, on a typical MCU-style device, you can set the Rotary encoder feedback to vary between Dot, BoostCut, Fill, and Spread modes.
```
    Rotary|                     TrackPan RingStyle=Dot
    DisplayLower|      		TrackPanDisplay

    Toggle+Rotary|              TrackPanWidth RingStyle=BoostCut
    Toggle+DisplayLower| 	TrackPanWidthDisplay
```

The new functionality is similarly dependent on the type of feedback processor used in your .mst for a given widget.

If using **FB_Encoder**, then the available options are:
```
RingStyle=Dot
RingStyle=BoostCut
RingStyle=Fill
RingStyle=Spread
```

If using **FB_FaderportValueBar**, then the available options are"
```
BarStyle=Normal
BarStyle=BiPolar
BarStyle=Fill
BarStyle=Spread
```

If using **FB_FP8ScribbleStripMode**, then the available options are:
```
Mode=0
Mode=1
Mode=2
....
Mode=8
```

If using **FB_FP8ScribbleLine1**, etc., then the available options are:
```
TextAlign=Center
TextAlign=Left
TextAlign=Right

TextInvert=Yes
TextInvert=No
```