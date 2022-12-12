## Widget Modes
Widget Modes are designed to send additional, specific, instructions to a given widget and added after the action in a zone file. For instance, on a typical MCU-style device, you can set the Rotary encoder feedback to vary between Dot, BoostCut, Fill, and Spread modes. Or turning off feedback for a given action.

### Feedback (For Turning Off Feedback on an Action)
If you want to disable feedback, or turn off button lights, or need to resolve an issue with misbehaving lights on a surface, you may want to add **Feedback=No** to the end of one or more actions to disable feedback.

Example:
```
    SomeButton SomeAction Feedback=No
```

...or even something like this:
```
    SomeButton SomeAction Feedback=No
    SomeButton AnotherAction 
    SomeButton YetAnotherAction Feedback=No
```

### RingStyle (For Use With Encoders)
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

### BarStyle (For Use With FaderPort8/16 Value Bars)
If using **FB_FaderportValueBar**, then the available options are"
```
BarStyle=Normal
BarStyle=BiPolar
BarStyle=Fill
BarStyle=Spread
```

### FaderPort Display Mode (For Use With FaderPort8/16 Stribble Strips)
If using **FB_FP8ScribbleStripMode**, then the available options are:
```
Mode=0
Mode=1
Mode=2
....
Mode=8
```

### TextAlign (For Use With FaderPort8/16 Stribble Strips)
If using **FB_FP8ScribbleLine1**, etc., then the available options are:
```
TextAlign=Center
TextAlign=Left
TextAlign=Right

TextInvert=Yes
TextInvert=No
```