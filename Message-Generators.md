Message Generators allow you to define what MIDI messages the surface sends to CSI. The following types of Message Generators exist in CSI, and which you use will depend on your the control and your surface:


* [[Press|Message-Generators#press]] - a simple generator that sends a single MIDI message on press, and optionally sends another message when released. Often, but not limited to, a button.
* [[AnyPress|Message-Generators#anypress]] - a variation of Press needed for some devices.
* [[Fader7Bit|Message-Generators#fader7bit]] - sends a MIDI message in a range specifying the current absolute value of the control. Used for faders and knobs with defined start and points.
* [[Fader14Bit|Message-Generators#fader14bit]] - like Fader7Bit, but has a larger (or more fine grained) set of values.
* [[Control|Message-Generators#control]] - used for OSC controls
* [[Encoders|Message-Generators#encoders]] - unlike the Faders, an Encoder sends a relative value (increase/decrease) 
* [[MFTEncoder|Message-Generators#mftencoder]] - a special encoder only found on the MIDI Fighter Twister controller
* [[Encoder7Bit|Message-Generators#encoder7bit]] - another special encoder for the X-Touch Compact and X-Touch Mini. **Note: added to the CSI EXP Build as of August 19, 2022.**\

If you need to define a new .mst file, because one does not exist for your surface, the general idea here is to use the Reaper action **CSI Toggle Show Raw Input from Surfaces** to observe what values are sent when each control is manipulated. So for example, when I press one of the buttons on my controller, I see this:

```
IN <- XTouchOne 90  5b  7f 
IN <- XTouchOne 90  5b  00 
```

The first line showed up when I pressed, the second one when I let go, so a reasonable assumption is the MIDI Generator in my widget should be:

```
Press 90 5b 7f 90 5b 00
```

# Press
Press is typically used for message generators that send a message when pressed, and optionally send another message when released. 

Defined using the following syntax:     
````Press 90 5e 7f 90 5e 00````      

where:
* 90 5e 7f is the message sent when the widget is pressed
* 90 5e 00 is the message sent when the widget is released (optional)

However, we still need to name our widget. So lets presume this is a Play button. In our .mst file, that widget would look like this:
```
Widget Play
	Press 90 5e 7f
WidgetEnd
```

The Play Widget has been declared without a Release message -- by far the most common definition type.

Here is an example of a button with a release message.
````
Widget Shift
	Press 90 46 7f 90 46 00
WidgetEnd
````

The Shift Widget has been declared with a Release message -- good for Buttons you plan to use for Shift/Option/Control/Alt, etc. or if you wish to use the "Hold" modifier.

**Tip:** if your surface creates release messages, include them in your .mst file. It's better to have release messages and not need them, then need them and not have them.

For information on how to assign buttons using Press widgets to toggles and stepped parameters in a Zone file, see [Stepped Params and Toggles.](https://github.com/GeoffAWaddington/CSIWiki/wiki/Stepped-Parameters-and-Toggles)

## Using Press for jogwheels
Additionally, press messages can be used to split up a jogwheel into clockwise (CW) and counter-clockwise (CCW) turns which can then be assigned to different actions in your CSI zone files. See the example below from a typical MCU device:
```
Widget JogWheelRotaryCW
	Press b0 3c 01
WidgetEnd

Widget JogWheelRotaryCCW
	Press b0 3c 41
WidgetEnd
```

# AnyPress
AnyPress is Message Generator for use with surfaces whose buttons alternate between a press message (7f) on press, and on second press, a release message (00). Use AnyPress widgets for these types of surfaces. 

```` 
Widget BankLeft
	AnyPress 90 2e 7f
WidgetEnd
````

In case you're still not sure about when to use Press versus AnyPress, try this:

1. Run the Reaper Action "CSI Toggle Show Input from Surfaces"
2. Press the button on your surface once

**Result:**

If you see two messages like 90 2e 7f and 90 2e 00, then use a Press widget.

If you only see one message like 90 2e 7f, then press the button a second time...

* If the next message is also 90 2e 7f, then use a Press widget with a single message defined.

* If the next message is 90 2e 00, then you should use an AnyPress widget.

# Fader7Bit
A Message Generator that represents a control that measures its current position using a 7bit value (ie. 128 possible values). While the name includes the word "Fader" this type of message would be used for both faders AND knobs with absolute values (i.e. a knob with a defined start and end location).

An example of a Fader7Bit widget with feedback would look like this:

```
Widget RotaryA1
     Fader7Bit b0 21 7f
     FB_Fader7Bit b0 21 7f
WidgetEnd
```

A message will be sent between b0 09 00 and b0 09 7f representing the current position of the fader.

When trying to decide if a Surface control should be represented using Fader7Bit or [[Encoder]], look at what values are actually being sent by the Surface. Fader7Bit expects an absolute value, whereas [[Encoder]] expects an increment/decrement value.

* Use the Reaper Action "CSI Toggle Show Input from Surfaces" to see what's actually being sent.   
* If you see a single value being sent when you turn the control clockwise and another when the control is turned anti-clockwise, go with Encoder.  
* If you see a full range of values (from 00 to 7f or vice versa) sent when you turn the control (increasing in one direction, decreasing in the other) go with Fader7Bit.

# Fader14Bit
A Message Generator that represents a control that measures its current position using a 14 bit value (ie. 16384 possible values). 

It's defined using the following syntax:     
```
Widget Fader1
	Fader14Bit e0 7f 7f
	FB_Fader14Bit e0 7f 7f
WidgetEnd
```
A message will be sent between e0 00 00 and e0 7f 7f representing the current position of the fader.

## Adding Touch Messages
If your control surface supports "Touch" messages for faders and encoders, these should also be defined in the widget as shown in the example below:

```
Widget Fader1
	Fader14Bit e0 7f 7f
	FB_Fader14Bit e0 7f 7f
	Touch 90 68 7f 90 68 00
WidgetEnd
```

In this example, touching Fader1 generates a message of 90 68 7f, and releasing Fader1 generates a second message of 90 68 00. 

# Control
The Message Generator titled simple 'Control' exists for OSC devices where a control is being defined in an .ost file. Example: here is what a touch-sensitive fader with feedback may look like in an OSC surface. You see Control followed by the OSC address of the fader, and corresponding rows for feedback and touch.
```
Widget Fader1
	Control /Mixer/Fader1
	FB_Processor /Mixer/Fader1
	Touch /Mixer/Fader1/z
WidgetEnd
```

# Encoders
Encoders are typically referred to as "Endless Rotary" knobs or "Relative Encoders" (i.e. knobs with no absolute begin and end positions). While CSI typically requires the use of [[Fader7Bit|Message-Generators#fader7bit]] widgets for traditional rotary knobs that communicate absolute start and end values, Encoders are different in that they communicate a relative change in value (i.e. it sends one value or one set of values when it is increasing and one when it is decreasing).

One large benefit of encoders versus traditional Rotary knobs that would use a [[Fader7Bit|Message-Generators#fader7bit]] widget, is that encoders typically allow for much higher resolution than 7bit MIDI.

In order to support the widest possible range of hardware encoders, CSI now allows users to customize which values their encoders transmit as well as define acceleration ranges for encoders that support it. EncoderPlain and EncoderPlainReverse are available if your surface has no acceleration.

## Encoder Types

### MCU Encoder
Here's an example of a typical MCU **Encoder** widget in CSI.
```
Widget Rotary1
	Encoder b0 10 7f
	FB_Encoder b0 10 7f
WidgetEnd
```

### Non-MCU Encoders, no acceleration
Use this syntax only if your hardware encoder transmits a single-value for clockwise and counter-clockwise turns. Because each surface may be different, replace the "41" and "01" messages with the correct values for your hardware.

In this example, the hardware encoder only transmits a value of 41 when rotated counter-clockwise (decrement, as noted by the < symbol), and only transmits a value of 01 when being rotated clockwise (increment, as noted by the > symbol). 
```
Widget Rotary1
	Encoder b0 10 7f [ < 41 > 01 ]
	FB_Encoder b0 10 7f
WidgetEnd
```

### Encoders with discrete acceleration steps
Use this syntax when surface transmits different acceleration values for increment (CW) and decrement (CCW) turns. However, your surface may skip values (notice 48 and 49 are missing as are 08 09).
```
Widget Rotary1
	Encoder b0 10 7f [ < 41 42 43 44 45 46 47 4a > 01 02 03 04 05 06 07 0a ]
	FB_Encoder b0 10 7f
WidgetEnd
```

### Encoders with a continuous acceleration range
Use this syntax when your encoder transmits a continuous range of acceleration values between a defined start and end range with no breaks or jumps in the data. 

```
Widget Rotary1
	Encoder b0 10 7f [ < 41-4a > 01-0a ]
	FB_Encoder b0 10 7f
WidgetEnd
```

### MFTEncoder
The MIDI Fighter Twister by DJ Tech Tools can be configured to work as an Encoder with **Velocity Acceleration**. However, because it uses a non-standard set of values when turning the knob clockwise and counter-clockwise, a special widget was developed for the MIDI Fighter Twister. The correct syntax for a MFTwister encoder is shown below, including the full set of acceleration steps when using the Velocity Sensitive encoder setting. As you can see, there are 11 acceleration levels in each direction.

```
Widget RotaryA1
	MFTEncoder b0 00 7f [ < 3f 3e 3d 3c 3b 3a 39 38 36 33 2f > 41 42 43 44 45 46 47 48 4a 4d 51 ]
	FB_Fader7Bit b0 00 00
WidgetEnd
```

### Encoder7Bit
Encoder7Bit exists because some encoders, like those in the X-Touch Compact and X-Touch Mini devices, can be configured as [absolute] rotary knobs that continue to send messages when turned beyond their maximum ranges. The encoders will continue to send 00 messages when at minimum and turned counter-clockwise, and 7f messages when at maximum and turned clockwise. In between, it sends standard absolute MIDI messages for the full 0-127 range. This effectively allows them to function as both traditional knobs and encoders. Use this widget type to enable that behavior.
```
Widget Rotary1
    Encoder7Bit ba 01 7f
WidgetEnd
```

Thanks to [mschnell](https://forum.cockos.com/member.php?u=60721) for contributing a script that directly led to the development of this message generator.

## Encoder Customization

### Default Range, Default Step Size, Default Acceleration
If you would rather rely on CSI's default values for encoders, just map your FX Parameter or CSI action to your encoder widget exactly like you normally would any CSI action. The full 0.0 to 1.0 parameter range will be used, as will the default CSI delta, with absolutely no acceleration. 

```
Encoder1 FXParam 1 "Attack" 
```

### Custom Parameter Ranges
Now, let's say you do not want to map your encoder to the full range of a particular plugin. Instead, you want to limit the range to only the first 66% of the parameter's full range. Example: you have a compressor with a very wide range of time for attack and release and you've determined only the first two-thirds of the range is usable. You can now limit the parameter ranges in CSI with the below syntax.

```
Encoder1 FXParam 1 "Attack" [ 0.0>0.66 ]
```


### Custom Encoder Tick Size (Custom Delta)
Let's say you don't necessarily want acceleration for a particular parameter, but you do want to tweak how much each encoder tick changes the parameter value (i.e. the delta). You can define how fine or coarse you want each tick of the encoder to be. In this case, we've changed the step size to 0.003 (or .3%).

```
Encoder1 FXParam 1 "Attack" [ (0.003) ]
```

### Custom Acceleration Curves
In this example, we've added a custom acceleration curve. To begin to understand this, consider that all parameters in CSI have a range from 0.0 to 1.0 (except for DB based Actions, but forget those for now). So what we're seeing here is the range of change values (or "deltas") for the encoder acceleration you defined earlier in your widget. The slowest turn will result in a change of 0.001 (.1%) which is very fine, and the fastest turn you defined will result in a step change of 0.1 (or 10%). 

What makes this so powerful is that you can completely adjust these values to fit your preference at a per-parameter basis. 

```
Encoder1 FXParam 1 "Attack" [ (0.001,0.005,0.025,0.05,0.1) ]   
```

### Custom Parameter Range With Custom Step Size
You can even combine the examples above. In this example, we're only using the first two-thirds of the parameter, but we're using our custom-defined step size of 0.003 (or .3%) per encoder tick.

```
Encoder1 FXParam 1 "Attack" [ 0.0>0.66 (0.003) ]
```

### Custom Parameter Range With Acceleration
Here's an example of how to limit the parameter range AND use a custom acceleration curve.

```
Encoder1 FXParam 1 "Attack" [ 0.0>0.66 (0.001,0.0025,0.033,0.05) ]
```

### Mapping Stepped Parameters To Encoders
In this FX zone, there are 3 Character modes, whose parameter values correspond to 0.0, 0.5, and 1.0, there are 10 Ensemble options (which are .1 step apart), and bypass is a simple on/off toggle (either 0.0 or 1.0). So to make this work with our Encoder widgets in an fx.zon file, they need to be mapped in our .zon file as such:

```
Zone "VST: Sonsig Rev-A (Relab Development)"
FocusedFXNavigator
Encoder1 FXParam "3" "Character" [ 0.0 0.5 1.0 ]
Encoder2 FXParam "4" "Ensemble" [ 0.0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1.0 ]
Encoder3 FXParam "0" "Bypass" [ 0.0 1.0 ]
ZoneEnd
```

### Overriding the Tick Count on Stepped Parameters
When using stepped parameters, you may find that you want to specify more, or fewer, encoder "ticks" to move the parameter to the next defined step. This is great for decreasing (or increasing) the sensitivity of the encoder when combined with stepped parameters. For instance, let's say a very small turn of your encoder is moving your parameter from the minimum to maximum values far too quickly. You may want to slow that down.

In this example, the "(12)" defines the number of encoder ticks (messages) that CSI must receive before it will move to the next parameter step. For example, if you're turning your encoder clockwise, CSI will need to count 12 consecutive increase values before the parameter moves from 0.0 to 0.1. Another 12 increase values will move it from 0.1 to 0.2, and 12 counter-clockwise messages will move it back down from 0.2 to 0.1.

```
Encoder2 FXParam 4 "Ensemble" [ (12) 0.0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1.0 ]
```

### Using Acceleration With Stepped Parameters
You can even create custom encoder acceleration values to work with stepped parameters. Looking at the below example, we see a list in parenthesis, followed by our traditional stepped parameters outside of the parenthesis. The values in the parenthesis relate to the encoder speed defined in your widget. So the slowest acceleration value will require 20 encoder ticks to move up or down to the next step, and fastest encoder acceleration will require 3 encoder ticks to move to the next parameter. 

```
Encoder1 FXParam  "4" "Ensemble" [ (20,15,12,6,3) 0.0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1.0 ]
```

Note: if your surface transmits 5 values for acceleration, the list in parenthesis must have the same number of values values as your hardware. So if your encoder transmits 11 acceleration values, the list in parenthesis should be 11 values long, etc. This way the acceleration speed always corresponds to a value produced by the hardware. Copy and paste is your friend.

## Additional Notes

- Some encoders like the MIDI Fighter Twister or Behringer BCR-2000 may require a FB_Fader7Bit processor instead of FB_Encoder to work properly. If you're not getting feedback, or your encoder is otherwise not working, you may want to try FB_Fader7Bit.

- When trying to determine the values, don't pay attention to what is displayed on the plugin's GUI. Example: a decay time of 2.4 seconds will **not** equal a parameter value of 2.4. One trick is to enter Write mode and begin creating automation data for the parameter you're trying to map, then clicking on the nodes and making note of where the parameter steps jump and what their real automation values are (remember: they will be between 0.0 and 1.0).

- When trying to decide if a Surface control should be represented using [[Fader7Bit]] or [[Encoder]], look at what values are actually being sent by the Surface. [[Fader7Bit]] expects an absolute value, whereas [[Encoder]] expects an increment/decrement value.

    *Use the Reaper Action "CSI Toggle Show Input from Surfaces" to see what's actually being sent.*     
    *If you see a single value being sent when you turn the control clockwise and another when the control is turned anti-clockwise, go with Encoder.*    
    *If you see a range of values sent when you turn the control (increasing in one direction, decreasing in the other) go with Fader7Bit.*    