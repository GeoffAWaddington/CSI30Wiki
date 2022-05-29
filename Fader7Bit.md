A Message Generator that represents a control that measures its current position using a 7bit value (ie. 128 possible values). While the name includes the word "Fader" this type of message would be used for both faders AND knobs with absolute values (i.e. a knob with a defined start and end location).

An example of a Fader7Bit widget with feedback would look like this:

```
Widget RotaryA1
     Fader7Bit b0 21 7f
     FB_Fader7Bit b0 21 7f
WidgetEnd
```

A message will be sent between b0 09 00 and b0 09 7f representing the current position of the fader.

### Notes:
When trying to decide if a Surface control should be represented using Fader7Bit or [[Encoder]], look at what values are actually being sent by the Surface. Fader7Bit expects an absolute value, whereas [[Encoder]] expects an increment/decrement value.

* Use the Reaper Action "CSI Toggle Show Input from Surfaces" to see what's actually being sent.   
* If you see a single value being sent when you turn the control clockwise and another when the control is turned anti-clockwise, go with Encoder.  
* If you see a full range of values (from 00 to 7f or vice versa) sent when you turn the control (increasing in one direction, decreasing in the other) go with Fader7Bit.