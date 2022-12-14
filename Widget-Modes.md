## Widget Modes
Widget Modes are designed to send additional, specific, instructions to a given widget and added after the action in a zone file. For instance, on a typical MCU-style device, you can set the Rotary encoder feedback to vary between Dot, BoostCut, Fill, and Spread modes. Or turning off feedback for a given action.

### Feedback
CSI will default to only providing feedback for the first action in a CSI macro action (i.e. one button assigned to trigger more than action). If you want to report the feedback state of anything other than the first action in a macro list, you simply add the word "Feedback" to the end of the action you want to provide the feedback. There can only be one of these for each CSI macro action or the last in the list will be used.

Example 1. I only want feedback on the first action (default behaviour - no additional syntax required)....
```
    SomeButton SomeAction
    SomeButton AnotherAction
    SomeButton YetAnotherAction
```

Example 2. I want feedback on the middle action only; so I use the new Feedback widget mode to specify which action gets it....
```
    SomeButton SomeAction
    SomeButton AnotherAction Feedback 
    SomeButton YetAnotherAction
```

Example 3. I want feedback on the last action only....
```
    SomeButton SomeAction
    SomeButton AnotherAction
    SomeButton YetAnotherAction Feedback
```

Example 4. Erroneous definition. In this case only the 3rd Action gets feedback.
```
    SomeButton SomeAction
    SomeButton AnotherAction Feedback
    SomeButton YetAnotherAction Feedback
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
If using **FB_FaderportValueBar**, then the available options are:
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